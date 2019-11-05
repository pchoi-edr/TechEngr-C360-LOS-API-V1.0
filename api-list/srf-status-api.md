# SRF Status API

This API provides basic identifying information about
service requests, their component collateral properties,
and the services to be performed on those collateral
properties. Results can be obtained by service request,
loan, or location.

If a location ID is specified, then the results will be
limited to only that single location. No other locations
will be present in the data structure returned to the client.

## Available Endpoints

The following API endpoints are available as part of this
subsystem, depending on the desired identifier used to
query the server:

### <span style="background-color: #72b566; font-weight: bold; color: #ffffff; padding: 3px 10px; border-radius: 14px;">GET</span> **Service Request Details**

```text
/api/v1/serviceRequest/status/serviceRequestID/:serviceRequestID
```

```text
/api/v1/serviceRequest/status/loanID/:loanID
```

```text
/api/v1/serviceRequest/status/locationID/:locationID
```

The API endpoints are accessible via the HTTP `GET` method.

#### Request

##### Path Parameters

The following URL path parameters are supported:

| Path Parameter | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| :serviceRequestID | Integer | Yes* | A service request ID. |
| :loanID | Integer | Yes* | A loan ID. |
| :locationID | Integer | Yes* | A location ID. |

\* Each of these parameters is required, but due to the nature
of the available endpoint URLs only one can be supplied at a time.

#### Response

##### Expected HTTP Response Code

When operating normally, this API endpoint will return
an HTTP response code of `200` ("OK").

##### Example JSON Response

The following represents an example JSON response for a
service request that is no longer in the draft status.
In this example, any value surrounded by angled brackets
would be replaced by a unique identifier in an actual
response. Thus, `<serviceRequestID>` would be an actual
service request ID.

Note that every service request will contain one or more
locations, and each location can contain zero or more
services. (If your company has custom workflow rules
implemented for its service request form, then your
data may conform to stricter requirements.)

```json
{
  "meta": {
    "date": "2018-04-28T12:23:23Z",
    "function": "get",
    "responseCode": 200,
    "responseID": "da288fe6-7991-11e8-adc0-fa7ae01bbebc",
    "success": true,
    "warnings": []
  },
  "data": {
    "serviceRequestID": <serviceRequestID>,
    "loanID": <loanID>,
    "draft": false,
    "locations": [
      {
        "locationID": <locationID>,
        "jobNumber": "<jobNumber>",
        "complete": false,
        "address": {
          "street1": "1600 Pennsylvania Avenue NW",
          "street2": "Oval Office",
          "city": "Washington",
          "state": "DC",
          "postalCode": "20500",
          "country": "US",
          "latitude": 38.879146,
          "longitude": -76.981882
        },
        "services": [
          {
            "serviceType": "PhaseI",
            "name": "Phase I Environmental Site Assessment",
            "serviceGroup": "Environmental",
            "locationServiceID": <locationServiceID>,
            "jobNumber": "<jobNumber>",
            "status": "New",
            "currency": "USD",
            "fee": 0.0,
            "primaryAssignee": "user1@example.com",
            "secondaryAssignees": [
                "user2@example.com",
                "user3@example.com"
            ],
            "startDate": "2018-12-13T08:10:10Z",
            "dueDate": "2018T-12-1615:10:10Z",
            "completionDate": "2018-12-14T10:10:10Z",
            "created": "2018-12-10T08:34:16Z",
            "updated": null
          }
        ]
      }
    ],
    "created": "2018-12-10T08:34:16Z"
  }
}
```

For each location:

  * The `complete` field will be `true` if the location
    has been marked as complete, or `false` if it is
    marked as incomplete.
    
  * Most values in the `address` section will be strings;
    if a value is missing, it will be an empty string.
    However, the geographical coördinates will be floating
    point numbers, or `null` if no coördinates are present.
    
  * The `country` field is not guaranteed to be a standard
    ISO-3166-1 alpha-2 country code. This value can be
    overridden by manual user input in the Collateral 360
    user interface, so client applications that consume the
    LOS API should be tolerant of literal country names,
    including variations and misspellings.
 
For each service:

  * The `serviceType` is the semi-human-readable code
    used to identify the type of service performed.
    
  * The `name` is the human-readable name of the service.
  
  * The `serviceGroup` is the broad category under which
    the service is organized:. Common values include the
    following (though this list may be appended to over
    time):
    
    * `Environmental`
    * `Valuation`
    * `Flood`
    * `Inspection`
    * `Construction`
    * `Additional Services`
    
  * The `locationServiceID` is an integer value that
    uniquely identifies each service associated with
    a location. This value is unique across all of the
    following criteria:
    
      * The location (identified by `locationID`).
      * The type of service (identified by 
         `serviceType`).
      * The number of times the same type of service
        has been requested for the same location.
         
    Thus, the `locationServiceID` value will be unique
    between any two services on different locations,
    as well as between any two services on the same
    location. Additionally, if a service (such as a
    Phase I Environmental Report, for example) is
    requested for a specific location, and later the
    same service is requested again for the same
    location, then both of these two otherwise
    identical services will nevertheless have unique
    `locationServiceID` values.
  
  * If your organization does not use service-level
    job numbers, then the `jobNumber` field will
    be `null`.

  * If no currency is assigned to a service request,
    then the `currency` field will be `null`.
     
  * If no `fee` value is present, it will be `null`.
    Otherwise, it will be a three-character alphabetic
    ISO 4217 currency code. 
  
    * The `fee` can be returned with a maximum of five
      decimal places, which is sufficient to represent
      every currency currently recognized as having a
      minor unit by the ISO 4217 standard. This includes
      all recognized nondecimal currencies, though of
      course some decimal values expressed in these
      currencies will represent invalid monetary amounts
      that are between valid quantized values).
 
  * If no `primaryAssignee` exists, it will be `null`.
       
  * If any date is missing, it will be `null`.

<div style="page-break-after: always;"></div>

For the benefit of LOS API integrators, the data types
of the major foregoing service-related fields are as
follow:

| JSON Attribute | Data Type | Size | Size Unit* | Notes |
| :--- | :--- | :--- | :--- | :--- |
| `serviceType` | String | 50 | Characters | |
| `name` | String | 255 | Characters | |
| `serviceGroup` | String | 60 | Characters | |
| `locationServiceID` | Integer (Unsigned) | 4 | Bytes | |
| `jobNumber` | String | 767 | Characters | Actual values will be much shorter, and will conform to an algorithm supplied by each client financial institution. |
| `status` | String | 50 | Characters | |
| `primaryAssignee` | String | 100 | Characters | |
| `secondaryAssignees` | Array of Strings | 100 | Characters | Size is per array element. |

\* _N.b._ all character lengths assume a UTF-8 encoding,
  and therefore require a maximum of four octets per
  character. All bytes are assumed to contain eight bits,
  as is usual on nearly all modern general-purpose
  computing hardware.

The data structure of an API response that represents
a draft service request will differ from the foregoing
example. Instead, it will look like the following:

```json
{
  "meta": {
    "date": "2018-04-28T12:23:23Z",
    "function": "get",
    "responseCode": 200,
    "responseID": "d65e6997-d73d-46e5-97f0-f12c989850f8",
    "success": true,
    "warnings": []
  },
  "data": {
    "serviceRequestID": <serviceRequestID>,
    "loanID": null,
    "draft": true,
    "locations": [
      {
        "address": {
          "street1": "1600 Pennsylvania Avenue NW",
          "street2": "Oval Office",
          "city": "Washington",
          "state": "DC",
          "postalCode": "20500",
          "country": "US",
          "latitude": 38.879146,
          "longitude": -76.981882
        },
        "services": [
          {
            "serviceType": "PhaseI",
            "name": "Phase I Environmental Site Assessment",
            "serviceGroup": "Environmental"
          }
        ]
      }
    ],
    "created": "2018-12-10T08:34:16Z"
  }
}
```

The major differences are as follow:

  * No loan ID will be assigned yet (`loanID` will be `null`).
  
  * The `draft` field will be `true`.
  
  * Locations will not have their location IDs assigned
    yet (`locationID` will be `null`).
  
  * Locations will not have any `complete` status.
  
  * Each service will only have its basic identifying
    information (indicating what type of service it is).
