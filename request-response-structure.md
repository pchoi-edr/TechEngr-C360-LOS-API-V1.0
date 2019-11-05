# Appendix 2 - API Data Structures

With the exception of authentication-related API endpoints,
the HTTP body contents of every request to--or response
from--the LOS API conforms to the following general JSON structure:

```
{
    "meta" : {},
    "data" : {}
}
```

However, note that some API endpoints use an HTTP method that
expects no body content (_e.g._ `GET` requests). In these cases,
you should not supply any HTTP body content to the API server.

Note also that the order of fields in the response is subject
to change. Your application should never assume that fields will
always be returned in the same order unless this specification
explicitly indicates that a particular data element will always
be returned in a given order by design.

## Handling Unknown JSON Attributes

This specification defines the JSON attributes that the
API server expects to receive. However, it is possible
for a client application to supply extra JSON attributes
in addition to those that are defined in this
specification.

If the API server receives any unknown JSON attributes,
it will ignore them, and continue processing the request
without considering the presence of these unknown
attributes to be an error.

This is by design, since in the future EDR may choose
to include these extra attributes in API responses in
order to facilitate chaining invocations of our API
endpoints with invocations of other services. In some
application architectures, these sorts of extraneous
metadata are intended for consumption by systems
invoked later in such a chain.

Client application developers should be aware that the
use of this feature does carry the following risks:

* An extraneous JSON attribute metadatum may use the
  same name as an attribute defined in a future version
  of the API. If your application relies on an attribute
  that experiences a namespace collision of this sort,
  then this may cause forward-compatibility difficulties
  when updating the application to newer versions of the
  API.
  
* This architectural model is prone to client software
  bugs where typos in attribute names remain unnoticed.
  If a client application accidentally misspells an
  optional attribute that would otherwise be understood
  by the API, then the API will assume this misspelled
  attribute is an extra metadatum, and it will be ignored.
  Whatever expected effect the attribute would have had
  will therefore not occur.
  
  Care should be taken by client application developers
  to minimize the likelihood of this scenario. For example:
  
  * It is always good practice to centralize the code
    where API requests are constructed in order to
    reduce the number of different locations where JSON
    attributes are set. Repetition of code should
    generally be avoided, since this will reduce the
    number of locations where attribute name typos
    may hide.
    
  * Depending on your language, it may be good practice
    to define constants, enumerations, or similar
    immutable data structures to refer to all attribute
    names, and to use them exclusively.
    
  * Depending on your architecture, it may be good
    practice to validate your API requests against a
    schema before sending them to the API server.

## General Data Types

The LOS API uses the following primitive data types:

  * **Strings**
  
    Unless otherwise noted, all character strings should
    be assumed to be Unicode encoded via UTF-8.
    
    The empty string "" is a valid string, and is permissible
    in contexts where empty string values are not otherwise
    rejected by specific input validation rules.
    
  * **Integers**
  
    These are numbers with no fractional components. negative
    integers are preceded by a single negative sign ("-"), but
    positive integers should omit their leading plus sign ("+").
    
    While negative zero ("-0") is a valid integer in some
    numbering schemes used in computing, it should be avoided
    in the LOS API. Its presence may cause undesired behavior,
    since some inputs may issue a validation error when it is
    encountered.
    
  * **Floating Point Numbers**
  
    These are numbers with a decimal component. As with
    integers, negative values are preceded by a negative
    sign ("-"), but positive values should omit the
    leading plus sign ("+").
    
    The LOS API understands the IEEE-754 floating point
    specification internally, but client applications
    should avoid the following values:
    
    * Negative infinity.
    * Positive infinity.
    * Negative zero ("-0.0").
    * Not a number ("NaN").
    
  * **Booleans**
  
    Boolean values represent truth or falsity. The system
    expects these to be supplied as one of the following
    first-class primitive data types:
    
      * true
      * false
      
    Boolean values should not be supplied as the integer
    values _1_ or _0_, and the values _true_ and _false_
    should not be quoted as if they were strings.
 
  * **Null**
  
    The null value represents the absence of a value, or the
    inapplicability of a datum to a use case.
    
The LOS API also understands the following complex data types:

  * **Arrays**
  
    These are collections of arbitrary values that are indexed
    numerically, by sequential, zero-based integers.
    
  * **Objects**
  
    In the LOS API, "object" refers to an associative map of
    arbitrary keys to values. These keys can be integers or
    strings.
    
    Though the term "object" is used, no object-oriented
    features are available to values of this data type.
    For example, there are no associated functions or methods,
    and no use of inheritance or encapsulation. The terminology
    is loosely derived from JavaScript, where associative maps
    have historically been implemented as properties of objects.
    This terminology is consistent with its usage in the
    specifications for JSON (_i.e._ JavaScript Object Notation).
    
Finally, the LOS API understands the following derived data type
formats:

  * **Dates**
  
    A date may contain a time component. Every date should be
    formatted according to ISO 8601, and most dates will be
    expressed in UTC (Universal Coördinated Time) with a Zulu
    suffix. For example, 3:56 PM on January 12, 2018 would be
    expressed like so:
                                       
      2018-01-12T15:56:00Z
    
    In general, if only a date is known, then the system will
    currently omit the timezone, and the date should be
    interpreted as belonging to a timezone appropriate to its
    context. For example, if a date with no time component
    represents the due date for completion of a task, then
    the timezone would likely refer to the geographic location
    where the office responsible for verifying task completion
    is located. This logic must be applied according to each
    client's specific business rules.
    
    The preceding example date would be expressed as follows
    without a time component:
    
      2018-01-12
      
  * **Emails**
  
    The system validates emails generally according to
    RFC 822, but with the following exceptions:
    
      * Comments are not supported.
      * Whitespace folding is not supported.
      * Dotless domains are not supported. 

## Contents of the `meta` and `data` Elements.

The contents of the `meta` and `data` elements will always be
specific to the individual API endpoint being invoked. As such,
you must refer to each API endpoint's documentation for the
specific values that are expected in these elements. 

The distinction between the contents of the _meta_ and _data_
elements is a semantic one that depends on the context of each
API endpoint. If general, the `data` element will contain the
primary data being sent or received, and the `meta` element will
contain supporting metadata that is in some way ancillary to
or descriptive of the data in the `data` element.

## API Request `Content-Type` Header

All API requests must contain a valid HTTP `Content-Type` header
with a format that the API server supports. Currently, this must
be as follows:

    Content-Type: application/json

## HTTP Response Status Codes of API Responses

The API server will do its best to process any request it
receives. Whenever possible, it will return its responses
with HTTP response status codes of 200 (indicating success).

Note that this is also true if an error occurs (inasmuch as
this is feasible). For example, if an API request is
malformed but was received by the API server, then an HTTP
server would normally return 400 HTTP response status code
(representing a "Bad Request" error). However, the LOS API
will instead return a 200 HTTP response status code (indicating
success), and will instead supply a 400 HTTP response status
code in the `responseCode` datum in its `meta` element. 

# API Response Structure

Every API response that contains HTTP body content will have
the following values in its `meta` element:

* The `date` datum will contain the date when the API response
  was processed, per ISO 8601. This date will always be
  expressed in UTC (Universal Coördinated Time) with a Zulu
  suffix. For example:
  
      2018-06-21T21:57:57Z
  
* The `function` datum is a semantically meaningful,
  human-readable string that describes the nature of either
  the API endpoint or the specific response. This is currently
  for descriptive purposes and to facilitate debugging client
  applications, but EDR may decide to assign permanent meaning
  to this field in the future. It should be treated as an
  opaque string by your application, and this specification
  neither defines its values nor guarantees they will not
  change (even within a single version of the API).
  
* The `responseCode` datum contains an integer code that
  describes how the API request was processed. This code
  represents an HTTP response status code, which may differ
  from the actual HTTP response status code sent back in the
  response's HTTP headers.
  
  Note that, just as in the HTTP specification, a `responseCode`
  value below 400 indicates a successful response. However,
  your application may wish to check the `success` datum instead
  of performing this calculation.
   
* The `responseID` datum will contain a unique identifier for
  the response. This will change every time a new request is
  made, even if the request was identical to a previous request.
  
  This value is currently a UUID (Universally Unique Identifier),
  but the structure of this value should not be assumed to
  remain constant over time. This value should be treated as an
  opaque string by your application.
  
* The `success` datum is a Boolean value that indicates whether
  the API request was successfully handled or not.
  
* The `warnings` datum is an array of warnings. If the API
  request generated no conditions that warrant a warning,
  then this array will be empty. Each warning is a data
  structure whose format is identical to an error (see the
  [Warnings](error-handling/warnings.md) section of this
  document for details).

Note that an API response that indicate the occurrence of an
error will typically have some additional data in its `meta`
element, as described in the
[Handling Errors](error-handling/errors.md) section of this
document. 
