# SRF Fields API - Property Types

Different companies have different needs when it comes to Property Types data field. Some will require a 2 tier data fields and other will require multiple tier data fields. This section will describe and explain what the 2-tier and multi-tier data fields will look like and how they will function. 

## The JSON Schema Model

The following example JSON Schema represents the minimal data
requirements and the required fields. String values ending with
ellipses ("`...`") represent metasyntactic descriptions of the
contents of their containing elements, and should not be
considered literal examples of the data in these elements. 

> Sample JSON-Schema with __**PropertyType**__ and __**PropertySubType**__
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
        ...
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
            "PropertyType": {
                "$id": "/properties/collaterals/items/properties/PropertyType",
                "description": "C360 Lender Two Tab SRF - Collateral",
                "title": "Property Type",
                "type": "object",
                "properties": {
                    "PropertyType": {
                        "$id": "/properties/collaterals/items/properties/PropertyType/properties/PropertyType",
                        "type": "string",
                        "description": "C360 Lender Two Tab SRF - Collateral",
                        "title": "Property Type",
                        "field": "PropertyType",
                        "enum": ["Agriculture", ...],
                        "minLength": 1,
                        "maxLength": 255
                    },
                    "PropertySubType": {
                        "$id": "/properties/collaterals/items/properties/PropertyClass/properties/PropertySubType",
                        "type": "object",
                        "description": "C360 Lender Two Tab SRF - Collateral",
                        "title": "Property Class",
                        "field": "PropertySubType",
                        "items": {
                            "$id": "/properties/collaterals/items/properties/PropertyType/properties/propertySubType/items",
                            "type": "object",
                            "properties": {
                                "Agriculture": {
                                    "$id": "/properties/collaterals/items/properties/PropertyClass/properties/PropertySubType/items/properties/Agriculture",
                                    "type": "string",
                                    "title": "Agriculture",
                                    "enum": ["Cropland", ...]
                                },
                            }
                        }
                    },
                },
                "dependencies": {
                    "propertySubType": ["propertyType"]
                },
                "required": ["PropertyType", "propertySubType"]
            }
        },
        "definitions": {
            ...
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
    },
  }
}
```

###	__**2 Tier Property Type**__

The 2-tiered Property Type is predicated on the basis of a __**PropertyType**__ and __**PropertySubType**__ only.

Property Type is an object in the __*"collaterals"*__ properties family of data fields. Since __*PropertyType*__ has a relationship with __*PropertySubType*__, the data structure will have a parent object of __**"PropertyType"**__ defined as an object, with the children __**PropertyType**__ and __**PropertySubType**__. 


```json
...
    "collaterals": {
      "$id": "/properties/collaterals",
      "type": "array",
      "items": {
        "$id": "/properties/collaterals/items",
        "type": "object",
        "properties": {
            ...
            "PropertyType": {
                "$id": "/properties/collaterals/items/properties/PropertyType",
                "description": "C360 Lender Two Tab SRF - Collateral",
                "title": "Property Type",
                "type": "object",
                "properties": {
                    "PropertyType": {
                        "$id": "/properties/collaterals/items/properties/PropertyType/properties/PropertyType",
                        "type": "string",
                        "description": "C360 Lender Two Tab SRF - Collateral",
                        "title": "Property Type",
                        "field": "PropertyType",
                        "enum": ["Agriculture", ...],
                        "minLength": 1,
                        "maxLength": 255
                    },
                    "PropertySubType": {
                        ...
                    }
                }
            }
        }
    }
...
```

Notice that the __**PropertyType**__ data field initiates an object with its own __**properties**__ with both __**PropertyType**__ and __**PropertySubType**__. The relationship between __**PropertyType**__ and __**PropertySubType**__ is denoted inside the parent __**PropertyType**__ object.

__**PropertyType**__ (the parent) has an enumerated value to select from. The selection of the __**PropertyType**__ value will dictate what is selectable as the __**PropertySubType**__. __**PropertySubType**__ is an object with an *items/properties* which will have a parent/child data structure based on your selection in __**PropertyType**__. 

```json
...
    "PropertyType": {
        "$id": "/properties/collaterals/items/properties/PropertyType/properties/PropertyType",
        "type": "string",
        "description": "C360 Lender Two Tab SRF - Collateral",
        "title": "Property Type",
        "field": "PropertyType",
        "enum": ["Agriculture", "Air Transport", ...],
        "minLength": 1,
        "maxLength": 255
    },
    "PropertySubType": {
        "$id": "/properties/collaterals/items/properties/PropertyType/properties/PropertySubType",
        "type": "object",
        "description": "C360 Lender Two Tab SRF - Collateral",
        "title": "Property Sub Type",
        "field": "PropertySubType",
        "items": {
            "$id": "/properties/collaterals/items/properties/PropertyType/properties/propertySubType/items",
            "type": "object",
            "properties": {
                "Agriculture": {
                    "$id": "/properties/collaterals/items/properties/PrPropertyTypeopertyClass/properties/PropertySubType/items/properties/Agriculture",
                    "type": "string",
                    "title": "Agriculture",
                    "enum": ["Cropland", "Grove/Orchard", "Mixed Use", "Organic Farm", "Pastureland", "Vineyard", "Other Agricultural"]
                },
                "Air Transport": {
                    "$id": "/properties/collaterals/items/properties/PropertyType/properties/PropertySubType/items/properties/Air Transport",
                    "type": "string",
                    "title": "Air Transport",
                    "enum": ["Airport Hangar", "Airport Infrastructure", "Airport Terminal", "Other Airport Facilities"]
                },
                ...
            }
        }
    }
...
```

As in the example above, selecting *"Agriculture"* as the __**PropertyType**__, limits your __**PropertySubType**__ selection to *PropertySubTypes/items/properties/Agriculture* "enum" values. Likewise selecting *"Air Transport"* limits the selection of __**PropertySubType**__ to *PropertySubTypes/items/properties/Air Transport*. 

The __**PropertySubType**__ properties are all strings therefore only one enumerated value selection is allowed.

###	__**Multi Tiered Property Type**__

The multi-tiered Property Type is predicated on the basis of a __**PropertyType**__ either as the parent with multiple sub-tiers under it or a parent super class before __**PropertyType**__ and one or more sub-tier under __**PropertyType**__. 

With the exception of more sub-tiers, multi-tiered Property Type is not different than the way a 2-tiered Property Type data fields are utilized. Basically selecting an enumerated value in the parent class dictates the selectable enumerated value in the direct child data field. 

```json
...
    "PropertyClass": {
        "$id": "/properties/collaterals/items/properties/PropertyClass/properties/PropertyClass",
        "type": "string",
        "description": "C360 Lender Two Tab SRF - Collateral",
        "title": "Property Class",
        "field": "PropertyClass",
        "enum": ["Agriculture", "Air Transport", ...],
        "minLength": 1,
        "maxLength": 255
    },
    "PropertyType": {
        "$id": "/properties/collaterals/items/properties/PropertyClass/properties/PropertyType",
        "type": "object",
        "description": "C360 Lender Two Tab SRF - Collateral",
        "title": "Property Type",
        "field": "PropertyType",
        "items": {
            "$id": "/properties/collaterals/items/properties/PropertyClass/properties/PropertyType/items",
            "type": "object",
            "properties": {
                "Agriculture": {
                    "$id": "/properties/collaterals/items/properties/PropertyClass/properties/PropertyType/items/properties/Agriculture",
                    "type": "string",
                    "title": "Agriculture",
                    "enum": ["Aquaculture", "Auction/Market", "Dairy", "Equestrian Facility", "Feedlot", "Grain Elevator", "Greenhouse/Nursery", "Livestock Production", "Ranch", "Winery", "Winery & Vineyard", "Other Agriculture"]
                },
                "Assembly Meeting Place": {
                    "$id": "/properties/collaterals/items/properties/PropertyClass/properties/PropertyType/items/properties/Assembly Meeting Place",
                    "type": "string",
                    "title": "Assembly Meeting Place",
                    "enum": ["Club/Lodge", "Community/Recreation Center", "Convention Center", "Reception/Banquet Hall", "Religious Facility", "Other Meeting Place"]
                },
                ...
            }
        }
    },
    "PropertySubType": {
        "$id": "/properties/collaterals/items/properties/PropertyClass/properties/PropertySubType",
        "type": "object",
        "description": "C360 Lender Two Tab SRF - Collateral",
        "title": "Property Sub Type",
        "field": "PropertySubType",
        "items": {
            "$id": "/properties/collaterals/items/properties/PropertyClass/properties/propertySubType/items",
            "type": "object",
            "properties": {
                "Aquaculture": {
                    "$id": "/properties/collaterals/items/properties/PropertyClass/properties/PropertySubType/items/properties/Aquaculture",
                    "type": "string",
                    "title": "Aquaculture",
                    "enum": ["N/A"]
                },
                "Club/Lodge": {
                    "$id": "/properties/collaterals/items/properties/PropertyClass/properties/PropertySubType/items/properties/Club/Lodge",
                    "type": "string",
                    "title": "Club/Lodge",
                    "enum": ["N/A"]
                },
                ...
            }
        }
    }
...
```

In this example above, we have a property super type, __**PropertyClass**__, with a child which is the __**PropertyType**__, which in turn has its own child, __**PropertySubType**__.

In this example, selecting *"Agriculture"* in __**PropertyClass**__ dictates a selection in __**PropertyType**__ in */properties/collaterals/items/properties/PropertyClass/properties/PropertyType/items/properties/Agriculture*. 

```json
...
    "Agriculture": {
        "$id": "/properties/collaterals/items/properties/PropertyClass/properties/PropertyType/items/properties/Agriculture",
        "type": "string",
        "title": "Agriculture",
        "enum": ["Aquaculture", "Auction/Market", "Dairy", "Equestrian Facility", "Feedlot", "Grain Elevator", "Greenhouse/Nursery", "Livestock Production", "Ranch", "Winery", "Winery & Vineyard", "Other Agriculture"]
    },
...
```

The selections available based on __**PropertyClass**__ selection are displayed above. Under */properties/collaterals/items/properties/PropertyClass/properties/PropertyType/items/properties/*, look up *"Agriculture"* , which have the selectable options for __**PropertyType**__

```json
...
    "enum": ["Aquaculture", "Auction/Market", "Dairy", "Equestrian Facility", "Feedlot", "Grain Elevator", "Greenhouse/Nursery", "Livestock Production", "Ranch", "Winery", "Winery & Vineyard", "Other Agriculture"]
...
```

Selecting from __**PropertyType**__ dictates you find in __**PropertySubType**__ the selection item in __**PropertyType**__ selected, in this case *"Aquaculture"*, in __**PropertySubType**__. 

So under __**PropertySubType**__, *items/properties*, you will find *"Aquaculture"*

```json
...
    "Aquaculture": {
        "$id": "/properties/collaterals/items/properties/PropertyClass/properties/PropertySubType/items/properties/Aquaculture",
        "type": "string",
        "title": "Aquaculture",
        "enum": ["Farm"]
    },
...
```

After selecting *"Agriculture"* in __**PropertyClass**__, you select *"Aquaculture"* under *"Agriculture"* in __**PropertyType**__, then you can select one of the selectable options in __**PropertySubType**__, in this case only option is *"Farm"*.

### Dependencies

As part of the data fields envelop for *2-tiered* or *multi-tiered* two additional identifier and definer objects exist __*dependencies*__ and __*required*__. 

```json
...
    "PropertyType": {
        "$id": "/properties/collaterals/items/properties/PropertyType",
        "description": "C360 Lender Two Tab SRF - Collateral",
        "title": "Property Type",
        "type": "object",
        "properties": {
            "PropertyType": {
                ...
            },
            "PropertySubType": {
                ...
            },
        },
        "dependencies": {
            "propertySubType": ["propertyType"]
        },
        "required": ["PropertyType", "propertySubType"]
    }
...
```

For a 2-tiered Property Type the *dependencies* will look like the following

```json
...
    "dependencies": {
        "propertySubType": ["propertyType"]
    },
...
```

where *"propertySubType"* has a dependency on *"propertyType"*.

For multi-tiered Property Type the *dependencies* will look like the following

```json
...
    "dependencies": {
        "propertyType": ["propertyClass"],
        "propertySubType": ["propertyType"]
    },
...
```

where *"propertyType"* has a dependency on *"propertyClass"* and *"propertySubType"* has a dependency on *"propertyType"*.

### Required

As part of the data fields envelop for *2-tiered* or *multi-tiered* two additional identifier and definer objects exist __*dependencies*__ and __*required*__. 

For Property Type, whether 2-tiered or multi-tiered, the parent data field is always required, therefore the children fields are also required. So the *required* object will look like

```json
...
    "required": ["PropertyType", "propertySubType"]
...
```

or 

```json
...
    "required": ["PropertyClass", "PropertyType", "propertySubType"]
...
```

### JSON Template

The representation in JSON Schema is slightly different than in the JSON Template used to submit the Service Request. 

Although the Property Types is nested in the JSON Schema, in the JSON Template, the data structure is flattened. 

This is primarily to support backwards compatibility in the Service Request Form submission. In a future version of the LOS API, this flattened data structure will be sun setted and replaced with a nested JSON template structure. 

So in the JSON template, it will look like the following

```json
...
    "collaterals": [{
        ...
        "PropertyType": "string",
        "PropertySubType": "string",
        ...
    }]
...
```

or 

```json
...
    "collaterals": [{
        ...
        "PropertyClass": "string",
        "PropertyType": "string",
        "PropertySubType": "string",
        ...
    }]
...
```