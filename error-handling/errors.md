# Handling Errors

Unless a catastrophic error occurs, LOS API responses should
always contain an HTTP response status code indicating success.
Any errors encountered by the API server will be identified by
the value of the API response's `responseCode` datum in its
top-level `meta` element, which will be equal to an HTTP
response status code that describes the error that occurred.

## Additional Error Metadata

When an error occurs, the API response's top-level `meta`
element will usually contain the following additional data:

* The `errors` element will be an array consisting of all
  relevant error messages. Each error message will be an
  associative structure with the following two elements:
  
  *  A `description` attribute (as a string) that
     contains the text of the error message,
   
  * A `field` attribute (as a string) that identifies
    the path to the field in the request that caused
    the error. If no specific field was the cause of
    the error, then this will instead be the empty
    string).
  
  EDR may add more attributes to each error message over
  time. Your application should be written in such a way
  that the addition of these new attributes will not cause
  an error.

  An example of the `errors` datum follows:
  
  ```
  "errors" : [
    {
      "description": "The \"First Name\" field is required.",
      "field": "data.firstName"
    },
    {
      "description": "The \"Surname\" field is required.",
      "field": "data.surname"
    }
  ]
  ```

* The `reason` datum will contain a brief textual description
  of the `responseCode` element's value (for example, if the
  `responseCode` is 404, then the `reason` may be equal to
  "Not Found").
  
  The specific text is subject to change over time, and should
  be considered informational only. While applications may
  choose to display or record this text as part of an error
  message or debugging capability, the application's logic
  should only rely on the `responseCode` field's corresponding
  integer value. 

### Field References in Error Attributes

As described earlier, each element in the `errors`
attribute will contain a `field` attribute that
identifies the path to the field in the request
that caused the error.

The format of this attribute follows the following rules:

1. If no specific field was responsible for the error,
   then the path will be an empty string.
  
2. Fields are identified from the root element to the
   element that caused the error in the string, read
   from left to right.
  
3. If an attribute is an array, and the field that caused
   the error is within one of its array elements, then
   its zero-indexed position in the array will be added
   to the path.
  
4. Field names will be delimited by periods.
  
5. If a field name contains a literal period or
   backslash, then each such character will be escaped
   by a preceding backslash.
   
6. A field that is not present in the supplied request
   data may be referred to (_e.g._ if a required field
   is missing from the request).

An example can be used to illustrate these rules. Given
the following request:

```
{
  "meta": {},
  "data": {
    "collection": [
      {
        "goodField": "Good Value"
      },
      {
        "badField": "Bad Value"
      }
    ]
  }
}
```

. . . if we assume that the `badField` element is the
cause of an error, then the corresponding error might
look like so:

```
{
  "description": "The field value is just plain bad.",
  "field": "data.collection.1.badField"
}
```

If the invalid field was instead named `full.name`, then
the `field` attribute would look like this (due to the
escaping rules for periods):

```
"data.collection.1.full\\.name"
```

Note the presence of two backslashes, since the
single backslash that escapes the period is also
escaped by an additional backslash when in a
JSON string. When rendered outside the context of
JSON, this would appear as
`data.collection.1.full\.name` instead.

If the field were instead `full\name`,
then the escaping would turn out like this (since
the original backslash and the additional backslash
the LOS API uses to escape it would both be escaped
in the JSON string by their own backslashes):

```
"data.collection.1.full\\\\name"
```

Similarly, when rendered outside the context of JSON,
this would appear as `data.collection.1.full\\name`
instead.
