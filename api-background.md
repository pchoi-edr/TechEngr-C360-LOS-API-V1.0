
# Appendix 3 - Technical Review

EDR's APIs are made available over TLS-encrypted HTTPS. Client
authentication is performed using OAuth2.

The API protocol itself is formatted as JSON, and uses a consistent
envelope structure for most calls (to facilitate centralization of
envelope construction and parsing).

## The RESTful Architecture

API requests follow a RESTful design, with appropriate usage of HTTP
methods (_i.e._ verbs) that match the type of operation being performed.
As used by the LOS API system, this means the following:

| HTTP Method | Meaning in the RESTful Architecture |
| :--- | :--- |
| `GET` | Retrieve a resource (or information about a resource). |
| `POST` | Create a new resource. |
| `PUT` | Replace an existing resource. Depending on the API, a `PUT` request for a nonexistent resource may result in the creation of that resource. |
| `DELETE` | Delete a resource. |
| `PATCH` | Modify an existing resource. |

## Exceptions to RESTful Principles

Note that the length of most URLs is limited in practice, despite the HTTP
1.1 specification not requiring such a limit. The maximum length of a URL
may depend not only upon the server processing the request, but also on
the user agent from which a client performs an HTTP request. For example,
in common Web browsers the URL is typically limited to a few kilobytes in
length.

Due to the prevalence of arbitrary limitations on URL length, EDR may
choose to use an HTTP `POST` method in place of any other, more
appropriate HTTP method if EDR's technologists deem it desirable to
handle URLs of arbitrary length for the affected API endpoint. If so,
this decision will be noted in the documentation.

## API Endpoint Descriptions

The first set of APIs available to integration developers is the
Draft SRF API product. These APIs are designed to manage the creation
and editing of service request form data prior to the point where
the data are finalized and ready to be acted on.

At this point in the due diligence process, information on-hand may
be incomplete or in flux, and decisions on exactly what services
need to be performed on real estate collateral may not yet be finalized.
Accordingly, the Draft SRF APIs allow incomplete data to be recorded
as they are obtained from borrowers or other sources.

Available API endpoints are as follow:

### SRF Fields

This endpoint returns a full representation of all draft SRF data
fields available for entry and modification by the API. The fields
are provided in the JSON Schema format, which is documented here:

    http://json-schema.org/

### SRF

The SRF endpoint allows you to create a new draft service request
form, or update an existing one.

### SRF Status

The Status endpoint returns basic details about an existing service
request. Data are available at the following levels:
* Information about a service request.
* The real property used as collateral in a service request.
* The services to be performed on each collateral property.

### SRF File List

This endpoint returns a list of downloadable files for a given SRF.

### SRF File Download

Invoking this endpoint downloads a specific SRF file.