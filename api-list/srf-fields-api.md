# SRF Fields API

The SRF Fields API will return a schema that describes the
fields available for entry on service request forms. The
response will be formatted according to the JSON Schema
specification, which is documented here:

    http://json-schema.org/

Client applications should use the returned JSON schema to
format API requests for the creation and modification of
service requests.

> During the initial testing phase of the LOS API, the field
> list returned by this endpoint will be substantially complete,
> but some fields may not be present. These fields will be
> added to the schema as integration with client customizations
> to EDR's SRF functionality continues.
> 
> Any SRF fields not included in the JSON schema must be entered
> manually via the Collateral 360 web application.

The schema returned by this endpoint will contain basic data
type validation rules. More specific validation rules (_e.g._
data formatting for complex data types) will be added over time
in order to assist client application developers in providing
improved user experiences in their applications. Client
applications should be tolerant of additions to the schema.

## The JSON Schema Model

The following example JSON Schema represents the minimal data
requirements and the required fields. String values ending with
ellipses ("`...`") represent metasyntactic descriptions of the
contents of their containing elements, and should not be
considered literal examples of the data in these elements.

```json
{
  "$id": "service-request-form",
  "type": "object",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "properties": {
    "meta": {
      "$id": "/properties/meta",
      "type": "object",
      "properties": {
        "cabinet": {
          "$id": "/properties/meta/items/properties/cabinet",
          "type": "string",
          "title": "The Cabinet Schema",
          "$ref": "#/definitions/cabinets"
        },
        "currency": {
          "$id": "/properties/meta/items/properties/currency",
          "type": "string",
          "title": "The Currency Schema",
          "$ref": "#/definitions/currency"
        },
        "requestedBy": {
          "$id": "/properties/meta/items/properties/requestedBy",
          "type": "string",
          "title": "The Created By Schema",
          "format": "email"
        }
      }
    },
    "transaction": {
      "$id": "/properties/transaction",
      "type": "object",
      "properties": {
        "Transaction Properties as Objects..."
      },
      "required": [
        "Required Transaction Fields..."
      ]
    },
    "collaterals": {
      "$id": "/properties/collaterals",
      "type": "array",
      "items": {
        "$id": "/properties/collaterals/items",
        "type": "object",
        "properties": {
          "Collateral Properties as Objects..."
        },
        "dependencies": {
          "propertyType": ["propertySubType"],
          "propertySubType": ["propertyType"]
        },
        "definitions": {
          "propertyTypes": {
            "$id": "/properties/collaterals/items/definitions/propertyTypes",
            "type": "object",
            "items": {
              "$id": "/properties/collaterals/items/definitions/propertyTypes/items",
              "type": "object",
              "properties": {

              }
            }
          }
        }
        "required": [
          "Required Collateral Fields..."
        ]
      }
    }
  },
  "definitions": {
    "services": {
      "$id": "/definitions/services",
      "type": "object",
      "default": "",
      "enum": [
        "Services as Objects..."
      ]
    },
  }
}
```

<div style="page-break-after: always;"></div>

## Available API Endpoints

The following endpoints are defined by this API subsystem:

### <span style="background-color: #72b566; font-weight: bold; color: #ffffff; padding: 3px 10px; border-radius: 14px;">GET</span> **Service Request Details**

```text
/api/v1/serviceRequest/fields
```

Accessing this endpoint via an HTTP `GET` method will return a
representation of the fields available as part of your SRF.

The fields will broadly be delineated into three groups, each under their
own data element:

* The `meta` group contains descriptions of fields that describe the SRF,
  but which are not actually visible SRF fields within Collateral 360.
  These may represent data that are normally handled behind-the-scenes
  or automatically within Collateral 360, and that therefore may require
  special consideration by your application.
  
  For example, if your organization uses cabinets in Collateral 360 to
  organize your service requests, cabinet selection is ordinarily performed
  in the user interface prior to entering data into the service request form
  screen. When creating a service request via the API, the schema for the
  cabinet selection field will accordingly appear in the `meta` section.
  
* The `transaction` section contains schemata for fields that are only
  entered once, regardless of how many collateral properties are part of
  the service request.
  
  For example, a loan principal amount would typically appear here, since
  each loan only has one principal amount irrespective of how many real
  properties are used to collateralize it. 
  
* The `collaterals` section contains schemata for fields that are entered
  for each collateral property associated with the service request.
  
  For example, a property street address would appear here, since each
  real property used to collateralize a loan has its own address. 

#### Request

##### Path Parameters

This endpoint does not accept any URL path parameters.

##### Body Parameters

This endpoint does not accept any HTTP body parameters.

#### Response

##### Expected HTTP Response Code

When operating normally, this API endpoint will return
an HTTP response code of `200` ("OK").

##### Example JSON Response (Abbreviated)

A successful invocation of this endpoint should return a
response with a structure similar to the following.
String values ending with ellipses ("`...`") represent
metasyntactic descriptions of the contents of their
containing elements, and should not be considered literal
examples of the data in these elements.

```json
{
  "meta": {
    "date": "2018-04-28T12:23:23Z",
    "function": "get",
    "responseCode": 200,
    "responseID": "5e28d8e0-7984-11e8-adc0-fa7ae01bbebc",
    "success": true,
    "warnings": []
  },
  "data": {
    "model": {
      "jsonSchema": {
        "JSON Object Formatted using JSON Schema Draft-7..."
      },
    },
    "dataTemplates": {
      "json": {"JSON Object Format..."},
      "xml": "XML in String Format..."
    }
  }
}
```

##### Example JSON Response (Full)

For reference, a more complete version of the preceding
example response follows.

```json
{
  "meta": {
      "date": "2018-04-28T12:23:23Z",
      "function": "get",
      "responseCode": 200,
      "responseID": "05da8aac-7985-11e8-adc0-fa7ae01bbebc",
      "success": true,
      "warnings": []
  },
  "data": {
    "model": {
      "jsonSchema": {
        "$id": "service-request-form",
        "type": "object",
        "$schema": "http://json-schema.org/draft-07/schema#",
        "properties": {
          "meta": {
            "$id": "/properties/meta",
            "type": "object",
            "properties": {
              "cabinet": {
                "$id": "/properties/meta/items/properties/cabinet",
                "type": "string",
                "title": "The Cabinet Schema ",
                "$ref": "#/definitions/cabinets"
              },
              "currency": {
                "$id": "/properties/meta/items/properties/currency",
                "type": "string",
                "title": "The Currency Schema ",
                "$ref": "#/definitions/currency"
              },
              "requestedBy": {
                "$id": "/properties/meta/items/properties/requestedBy",
                "type": "string",
                "title": "The Requested By Schema ",
                "format": "email"
              }
            },
            "required": [
              "cabinet",
              "currency",
              "requestedBy"
            ]
          },
          "transaction": {
            "$id": "/properties/transaction",
            "type": "object",
            "properties": {
              "Transaction Properties as Objects..."
            },
            "required": [
              "Required Transaction Fields..."
            ]
          },
          "collaterals": {
            "$id": "/properties/collaterals",
            "type": "array",
            "items": {
              "$id": "/properties/collaterals/items",
              "type": "object",
              "properties": {
                "Collateral Properties as Objects..."
              },
              "required": [
                "Required Collateral Fields..."
              ]
            }
          }
        },
        "definitions": {
          "services": {
            "$id": "/definitions/services",
            "type": "object",
            "default": "",
            "enum": [
              "Services as Objects..."
            ]
          }
        },
        "required": [
          "Required Main Fields..."
        ]
      }
    },
    "dataTemplates": {
      "JSON": {
        "meta": {
          "cabinet": "string",
          "currency": "USD",
          "requestedBy": "email"
        },
        "transaction": {
          "Transaction Fields as Objects"
        },
        "collaterals": [
          {
            "Collaterals as Array of Objects",
            "services": [
              "Array of Services..."
            ]
          }
        ]
      },
      "XML": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>
        <serviceRequestForm>
          <meta>
            <cabinet />
            <currency />
            <requestedBy />
          </meta>
          <transaction />
          <collaterals>
            <collateral />
          </collaterals>
        </serviceRequestForm>"
    }
  }
}
```

## Cabinets

The available cabinet names will be provided in the
JSON schema returned by this endpoint, so that client
applications can present the list of cabinets to their
end users for selection.

<div style="page-break-after: always;"></div>
