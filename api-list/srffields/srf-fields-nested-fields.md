# SRF Fields API - Nested Fields

In the JSON Schema data structure, C360 Service Request Lender Data Fields have related fields that are always not pre-determined. So when we pull Lender Data Fields for a companyID, some secondary related data fields have rules defined in C360 that are not available as an object of the Lender Data Fields. Instead, based on the rules defined in C360, a related field is dynamically created to support the primary field the related field is created for. 

For example, some fields have a unit classification which needs to have selections appropriate for the field. Like *BuildingSize* is a common field in C360 Service Request Forms submission. In order for the data to make sense, it must be suffixed with a Unit measurement, whether its Square Feet or Square Meters. 

This section will outline what the JSON Schema field representation will be, and what the JSON template structure will be. 

## The JSON Schema Model

The following example JSON Schema represents the minimal data
requirements and the required fields. String values ending with
ellipses ("`...`") represent metasyntactic descriptions of the
contents of their containing elements, and should not be
considered literal examples of the data in these elements. 

Some fields like 

- "BuildingSize": "string"
- "BuildingSizeCompleted": "string"
- "LandSize": "string"
- "ExcessLand": "string"

Have a related data field which measures Units. 

```json
...
    "BuildingSize": {
        "$id": "/properties/collaterals/items/properties/BuildingSize",
        "description": "C360 Lender Two Tab SRF - Collateral",
        "title": "Improvement Size As Is",
        "type": "object",
        "properties": {
            "BuildingSize": {
                "$id": "/properties/collaterals/items/properties/BuildingSize/properties/BuildingSize",
                "type": "string",
                "description": "C360 Lender Two Tab SRF - Collateral",
                "title": "Improvement Size As Is",
                "field": "BuildingSize",
                "maxLength": 195
            },
            "BuildingSizeUnits": {
                "$id": "/properties/collaterals/items/properties/BuildingSize/properties/BuildingSizeUnits",
                "type": "string",
                "description": "C360 Lender Two Tab SRF - Collateral",
                "title": "Improvement Size As Is",
                "field": "BuildingSizeUnits",
                "enum": ["SF"]
            }
        },
        "dependencies": {
            "BuildingSizeUnits": ["BuildingSize"]
        },
        "required": ["BuildingSize", "BuildingSizeUnits"]
    },
...
```

In this example *BuildingSize*, the field requires a Unit measurement. So the LOS creates a related data field called *BuildingSizeUnits* with an enumerated array of acceptable values. In this case and example, the only acceptable Unit value is *"SF"*. 

### Dependencies

In this example *BuildingSize*, *BuildingSizeUnits* is dependent on *BuildingSize*.

### Required

In this example *BuildingSize*, is not required to submit the Service Request Form, so its not required as part of the Service Request Form submission, however, within the *BuildingSize* data field, if *BuildingSize* is filled out, *BuildingSizeUnits* is now required. 