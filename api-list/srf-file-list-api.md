# SRF File List API

This API provides a list of downloadable files associated
with a given service request, loan, or location. The available files
can be downloaded using the [SRF Download API](srf-file-download-api.md).

If a location ID is specified, then the results will be
limited to only that single location. No other locations
will be present in the data structure returned to the client.

## Available Endpoints

The following endpoints are made available by this API, depending on
the desired identifier used to query the server:

### <span style="background-color: #72b566; font-weight: bold; color: #ffffff; padding: 3px 10px; border-radius: 14px;">GET</span> **Service Request Details**

```text
/api/v1/serviceRequest/files/list/serviceRequestID/:serviceRequestID
```

```text
/api/v1/serviceRequest/files/list/loanID/:loanID
```

```text
/api/v1/serviceRequest/files/list/locationID/:locationID
```

The API endpoints are accessible via the HTTP `GET` method.

#### Request

##### Path Parameters

| Path Parameter | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| :serviceRequestID | Integer | Yes* | A service request ID. |
| :loanID | Integer | Yes* | A loan ID. |
| :locationID | Integer | Yes* | A location ID. |

\* Each of these parameters is required, but due to the nature
of the available endpoint URLs only one can be supplied at a time.

##### Body Parameters

This endpoint does not accept and HTTP body parameters.

#### Response

##### Expected HTTP Response Code

When operating normally, this API endpoint will return
an HTTP response code of `200` ("OK").

##### Example JSON Response

```json
{
  "meta": {
    "date": "2018-04-28 12:23:23",
    "function": "get",
    "responseCode": 200,
    "responseID": "e3733640-789c-11e8-9dfc-81c439846400",
    "success": true,
    "warnings": []
  },
  "data": {
    "files": [
      {
        "uploadID": <uploadID>,
        "locationID": <locationID>,
        "filename": "instructions-for-appraisal.txt",
        "type": "text/plain",
        "size": 314159,
        "uploadedAt": "2015-05-23T15:23:06Z",
        "uploadedBy": "user@example.com",
        "status": "Final Report",
        "documentType": "Instruction Letter",
        "serviceGroups": [
          "Valuation"
        ],
        "serviceTypes": [
          {
            "serviceType": "Appraisal",
            "name": "External Appraisal Report",
            "serviceGroup": "Valuation"
          }
        ],
        "attributes": [
          {
            "name": "Prepared By",
            "value": "Mr. Example"
          },
          {
            "name": "Report Date",
            "value": "2018-09-05"
          },
          {
            "name": "Report Title",
            "value": "Special Instructions for Appraisal"
          }
        ]
      }
    ]
  }
}
```

#### Response Structure

Each entry in the _files_ array represents a file that has been
uploaded to a service request and associated with one of its
locations. The `uploadID` field is the unique identifier for
each uploaded file.

Most fields follow the naming conventions and data structure
used by Collateral360's File Manager screen, but the following
fields require special explanation:

  * The **filename** field represents the original filename of
    the uploaded document.

  * The **type** field represents the media type of the file
    (formerly--but still commonly--known as the MIME type). This
    should conform to section 3.1.1.1 of RFC 7231.
    
  * The **size** field represents the size of the file, in eight-bit
    bytes (_i.e._ in octets). Client applications may wish to convert
    this into a more readily human-readable form for display, typically
    one of the following:
    
      * Kilobytes or kibibytes.
      * Megabytes or mebibytes.
    
Further, it should be noted that the `attributes` field contains
an array of optional metadata associated with the file. This
list is subject to expansion over time, so client applications
should be tolerant of new attributes.

For the benefit of LOS API integrators, the data types
of the major foregoing fields are as follow:

| JSON Attribute | Data Type | Size | Size Unit* | Notes |
| :--- | :--- | :--- | :--- | :--- |
| `uploadID` | Integer (Unsigned) | 4 | Bytes | |
| `locationID` | Integer (Unsigned) | 4 | Bytes | |
| `filename` | String | 255 | Characters | |
| `type` | String | 255 | Characters | |
| `size` | String | 255 | Characters | Values will be either non-negative integers or the empty string. Note that Collateral360 limits uploaded files to well below 1 GiB in size, so this field will never substantially approach its maximum allocated length. |
| `uploadedBy` | String | 100 | Characters | |
| `status` | String | 50 | Characters | |
| `documentType` | String | 255 | Characters | |
| `serviceGroups` | Array of Strings | 60 | Characters | Size is per array element. |

For the JSON elements inside the `serviceTypes` element:

| JSON Attribute | Data Type | Size | Size Unit* | Notes |
| :--- | :--- | :--- | :--- | :--- |
| `serviceType` | String | 50 | Characters | |
| `name` | String | 255 | Characters | |
| `serviceGroup` | String | 60 | Characters | |

For the JSON elements inside the `attributes` element:

| JSON Attribute | Data Type | Size | Size Unit* | Notes |
| :--- | :--- | :--- | :--- | :--- |
| `name` | String | 255 | Characters | |
| `value` | String | 255 | Characters | |

\* _N.b._ all character lengths assume a UTF-8 encoding,
  and therefore require a maximum of four octets per
  character. All bytes are assumed to contain eight bits,
  as is usual on nearly all modern general-purpose
  computing hardware.