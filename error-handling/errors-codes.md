# Common Error Codes

The following list represents the error codes you are most
likely to encounter as a user of the LOS API, and what they
are likely to represent within the specific context of the
LOS API system.

This list is not exhaustive. Error codes are mapped to HTTP
response status codes, so refer to the HTTP specification for
the full list of possible values. 

| **Name** | **Code** | **Description** |
| --- | --- | --- |
| Bad Request | 400 | The API request's body content was malformed, or its format was correct but its contents failed to pass validation checks. This may also occur if a dynamic value in the URL was missing or invalid. |
| Unauthorized | 401 | The API request either lacked authentication credentials, or the authentication credentials expired. Obtain a new access token and try again. |
| Forbidden | 403 | The API request attempts to acces or modify a resource, but the client does not have permission to do so. |
| Not Found | 404 | A requested resource was not found. This is most likely to occur when attempting to download a resource via the download API. |
| Method Not Allowed | 405 | The specified HTTP method is not allowed for the API endpoint. For example, an HTTP `GET` request was made to an endpoint that only accepts `POST` requests. |
| Too Many Requests | 429 | Too many API requests have been issued in a period of time. Wait for a while and then try to resubmit the request. If your application submits multiple requests in a large batch, consider introducing a delay or velocity limitation. |
| Internal Server Error | 500 | A technical error occured in the API server. This indicates a problem within the API server itself. |
| Not Implemented | 501 | The API request attempted to perform an action that is not yet implemented on EDR's servers. This is only likely to occur during a closed beta test of new features. |
