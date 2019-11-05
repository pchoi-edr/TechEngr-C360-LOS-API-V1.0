# Document Information

This section contains information about this document itself.
It does not refer to the API described by this document.

## Revision History

The following changelog contains the revision history of this document:

| Revision | Date | Primary Authors | Overview of Changes |
| :--- | :--- | :--- | :--- |
| 0.9.5 | Oct. 9, 2019 | Philip Choi | Fixed typo in diagram under Structure of a Service Request  |
| 0.9.4 | Apr. 20, 2019 | Philip Choi | Added PropertyType and its parent child relationship in regard to the values and options available on selection of the PropertyType. Added the handling of 2-tiered and multi-tiered PropertyType. Added nested data structure to conjoin relational child data fields with their parent counterpart. |
| 0.9.2 | Mar. 20, 2019 | Randall Betta | Added the `locationServiceID` field to the non-draft "SRF Status" API results. Documented selected JSON attribute data types for the "SRF Status" and "SRF File List" API results. Added the "Limitations on Email Address Lengths" section. |
| 0.9.1 | Jan. 30, 2019 | Randall Betta | Corrected the alphabetic case of the draft SRF creation endpoint. Added the "Structure of a Service Request" section. Added minor clarifications to the "Business Workflow Overview" section. |
| 0.9 | Sep. 26, 2018 | Randall Betta | Corrected output fields in the "SRF File List" API; redesigned the "SRF File Download" API; removed references to HTTP redirection response status codes; corrected the example JSON request for the SRF API. |
| 0.8 | Sep. 7, 2018 | Randall Betta | Corrected example URLs in the authentication section; added a preface to the list of APIs that describes the section's conventions; changed the structure of the endpoint URLs for the "SRF File List" and "SRF Status" APIs. |
| 0.7 | Sep. 7, 2018 | Randall Betta | Documented supported data types; changed endpoint URL paths for consistency; updated the "SRF File List" response data structure; removed the "serviceRequestID" parameter from the "SRF File Download" API endpoint URL. |
| 0.6 | Aug. 27, 2018 | Randall Betta | Documented structural and semantic changes to the SRF status API response; documented the new `warnings` element returned in all API responses.|
| 0.5 | Aug. 14, 2018 | Randall Betta | Renamed the `createdBy` element to `requestedBy` so it can be consistent across future endpoints. |
| 0.4 | Aug. 3, 2018 | Randall Betta | Changed the JSON data structure used to add services to draft service requests; documented expected HTTP response codes for all endpoints; promoted the description of cabinets to its own section; added the new `field` attribute to error responses; documented how unknown JSON attributes are handled. |
| 0.3 | Jul. 16, 2018 | Randall Betta | Updated API specifications to match changes in data structures; expanded information about character encoding and character set support. Added document information. |
| 0.2 | Jun. 27, 2018 | Randall Betta | Updated API specifications to match changes in data structures; restructured document; rewrote all major sections; added extensive technical and business overview information. |
| 0.1 | Jun. 12, 2018 | Philip Choi | Initial document creation: defined technical specifications for all API endpoints. |
