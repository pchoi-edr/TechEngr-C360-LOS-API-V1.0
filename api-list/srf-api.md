# SRF API

The SRF API is used to create or replace a draft service request.
Within Collateral 360, draft service requests are considered
works-in-progress, and therefore can be saved with incomplete
information. They are designed to record the user's work during
the time period when data about a loan or its collateral properties
are still being acquired, or when decisions are still being made
regarding the services to perform for each collateral property.

The fields accepted by this API are defined by the JSON schema
returned by the [SRF Fields API](srf-fields-api.md).

The API request format is largely identical for both the creation
of new service requests and the modification of existing ones.
The general format is described in the following sections. Any
differences in syntax or usage between these two use cases will
be noted.

## Handling of Invalid Data

Each draft service request is considered to be a
work-in-progress, and most data validation rules are not
executed while a service request is in the draft status.

Accordingly, each invalid field value supplied to the draft
SRF API endpoints will cause one of the following results
to occur:

  * The invalid value will be recorded in the draft
    service request.
    
  * The invalid value will result in a blank field in
    the draft service request.
    
In either case, the system may generate a warning in the
API response.

Note that, due to internal implementation details of the API
system, an invalid user account email address should always
generate a warning if it is not used for determining access
according to permissions rules (in which case an error will
be generated instead).

## API Request Structure

API requests issued to create or modify a service request all have
the same high-level structure.

### Metadata Field Definitions

The following fields should be included in the API request's
top-level `meta` element:

| Metadata Field | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| cabinet | Yes | String | The name of the cabinet that will contain the draft SRF. |
| currency | Yes | String | A three-letter ISO 4217 currency code. This must be in uppercase. |
| requestedBy | Yes | String | The email address of the user who is issuing this request (for an HTTP `POST` operation, this will be the service request creator). This must be an email address associated with an appropriate user account in Collateral 360. |

### Data Field Definitions

The following elements will be expected within the API request's
top-level `data` element:

| Data Field | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| transaction | Yes | Object | General data fields that describe the service request. |
| collaterals | Yes | Array | An array where each element contains the data fields that describe a single real property used as collateral. |

Note that, depending on your configuration options in Collateral 
360, draft SRFs may only be visible to the user who created them.
In this case, the `requestedBy` field defines this user within
the original API call used to create a service request.

Unless otherwise noted, all monetary amounts in the request will be
denominated in the specified currency.

### Transaction Field Definitions

The values in the API request's `transaction` element will
vary by client. Each value represents a data field that describes
the overall service request independently of any collateral (such
as a loan principal amount).

The fields that can be included in this section are defined in the
JSON schema returned by the SRF Fields API.

### Collateral Field Definitions

The API request's `collaterals` element consists of an array where
each element represents an individual real property used to
collateralize a loan. As such, the fields in this section can
be repeated between each element.

Similarly to the `transaction` element's contents, the fields in
each element of the `collaterals` array are defined in the JSON 
schema returned by the SRF Fields API. However, one additional field
is present:

| Data Field | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| services |Yes| Array | An array of services to perform on the collateral property. |

Each element in the `services` array is an object that represents a
service to perform on a collateral property, such as the following
examples (your set of services may be different):

* Phase I Environmental Report

* Property Appraisal Inspection

Each service is associated with a _feature_, which represents the broad
category of services it belongs to. Typical features may include
environmental services, appraisal services, construction services, _etc._

You may wish to segregate or group services according to their
associated features within your application's user interface to
improve its intuitiveness.

Each element in the `services` array must be an object that has
the following values, which are defined in the JSON schema
returned by the SRF Fields API:

| Data Field | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| serviceType | Yes | String | The unique value used to identify each type of service. |

<div style="page-break-after: always;"></div>

## Available API Endpoints

The following endpoints are supported by the SRF API subsystem:

### <span style="background-color: #ebb747; font-weight: bold; color: #ffffff; padding: 3px 10px; border-radius: 14px;">POST</span> **Service Request Form**

```text
/api/v1/serviceRequest/form
```

This endpoint is used to create a new draft service request, and
is invoked via the HTTP `POST` method.

#### Request

##### Path Parameters

This endpoint does not accept any path parameters in its URL.

##### Body Parameters

The API request body conforms to the standard API request
format (_i.e._ a `meta` and a `data` element), per the
[API Data Structures](../request-response-structure.md)
section of this document.

The request's top-level `data` element should contain the
following elements, each of which must conform to the
requirements described above.

* `transaction`

* `collaterals`

The contents of these elements collectively define the contents of
the service request created by a successful invocation of this
endpoint.

If the specified service request is successfully created by this
endpoint, then the API response's `data` element will contain
a `serviceRequestID` element, which will be an integer that
uniquely identifies the newly created service request. 

##### Example JSON Request

A request to create a service request might look like the following
in general structure (replacing example values with values appropriate
to your specific application, per the JSON schema returned by the
SRF Fields API):

```json
{
  "meta": {
    "cabinet": "Example Cabinet Name",
    "currency": "USD",
    "requestedBy": "creator@example.com"
  },
  "data": {
    "transaction": {
      "exampleTransactionField": "Example Value"
    },
    "collaterals": [
      {
        "exampleCollateralField": "Example Value",
        "services": [
          {
            "siteType": "Appraisal"
          },
          {
            "siteType": "Environmental"
          }
        ]
      }
    ]
  }
}
```

#### Response

##### Expected HTTP Response Code

When operating normally, this API endpoint will return
an HTTP response code of `201` ("Created").

##### Example JSON Response

A successful API request to create a new service request will
yield an API response similar to the following:

```json
{
    "meta": {
        "date": "2018-04-28T12:23:23Z",
        "function": "create",
        "responseCode": 201,
        "responseID": "3b811014-7984-11e8-adc0-fa7ae01bbebc",
        "success": true,
        "warnings": []
    },
    "data": {
        "serviceRequestID": 1234567
    }
}
```

The `serviceRequestID` element contains a value that uniquely
identifies the newly created service request. Your application
should record this value, since any subsequent API requests you
may issue to modify the new service request must use this
value to identify which service request to update.

### <span style="background-color: #5493dc; font-weight: bold; color: #ffffff; padding: 3px 10px; border-radius: 14px;">PUT</span> **Service Request Form**

```text
/api/v1/serviceRequest/form/:serviceRequestID
```

This API endpoint is used to replace an existing service
request, and is invoked via the HTTP `PUT` method.

Note that the entire contents of the service request are
replaced with the new values. As such, your application
should always supply every field you do not wish to be
converted into a blank value. This operation essentially
represents a complete replacement of all values, rather
than a partial update on a field-by-field basis.

#### Request

##### Path Parameters

This endpoint accepts the following parameters in its
URL path:

| Path Parameter | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| :serviceRequestID | Yes | Integer | The value that uniquely identifies the service request to update. |

##### Body Parameters

This endpoint accepts the same body parameters as the
corresponding SRF creation endpoint (accessible via
an HTTP `POST` operation).

##### Example JSON Request

API requests to this endpoint will be identical in
structure to those sent to the SRF creation endpoint
(accessible via an HTTP `POST` operation).

#### Response

##### Expected HTTP Response Code

When operating normally, this API endpoint will return
an HTTP response code of `200` ("OK").

##### Sample JSON Response

API responses returned by this endpoint will be identical
in structure to those sent to the SRF creation endpoint
(accessible via an HTTP `POST` operation).

<div style="page-break-after: always;"></div>
