# Authentication

Access to the LOS API system is limited to only registered
institutions and individuals, and the authentication process
for the LOS APIs is handled by an OAuth2 server. The full OAuth
2 framework is defined by the Internet Engineering Task Force
in RFC 6749:

    https://tools.ietf.org/html/rfc6749
    
Note that an exhaustive knowledge of OAuth2 is not required to
begin using our APIs, though you may find some background
information about the process helpful or instructive.

For authentication via OAuth2, EDR uses only the "client
credentials" authorization grant. A general OAuth2
workflow can be seen in the following diagram.

<div style="text-align: center; border: 1px solid #ccc; padding: 20px">
    <img src="./auth-seq.png" width="400">
</div>

## Obtaining a Client ID and Secret Key from EDR.

Every institution or individual allowed to access the LOS API
system is identified by two separate data:

* A _client ID_ identifies the specific application that
  is connecting to the API. Each client ID is tied to a
  single individual or institution, and each distinct
  software program that uses the LOS API should have a
  separate client ID.
  
* A corresponding _secret key_ must be supplied in conjunction
  with each client ID. This value should be kept confidential
  by the individual or organization who is responsible for
  its associated client ID.

During the API's initial closed testing phase, these access
credentials will be provided by EDR's technology team on an
_ad hoc_ basis. After this phase of the project is complete,
EDR will publish updated instructions on how to request
and receive access credentials.

## Obtaining a Time-Limited Access Token

Every API request sent to EDR's servers must contain an access
token, which is used to prove that the sender has successfully
authenticated itself as an application acting on behalf of a
registered user of the system.

For security reasons, access tokens are only valid for a
limited amount of time. If an API request is made with a
valid access token, but the API server responds with a 401
status code in the `meta` element's `responseCode` element,
then this indicates that the access token has expired. The
API client should obtain a new access token and repeat the
failed request.

An example of this type of response follows:

```
{
  "meta": {
    "date": "2018-07-16T18:52:21Z",
    "errors": [
      {
        "description": "Authorization not successful"
      }
    ],
    "function": "error",
    "reason": "Unauthorized",
    "responseCode": 401,
    "responseID": "5ef14290-8929-11e8-85d7-6d1f9ead714f",
    "success": false
  },
  "data": {}
}
```

Obtaining an access token is simple. Just send an HTTP `POST`
request to the following URL (_n.b._ this example uses the 
UAT environment; the appropriate hostname may differ, with
available hostnames listed later in this section):

    https://<hostname>/api/v1/oauth/token/
    
. . . where `<hostname>` refers to an appropriate LOS API server.
Depending on the phase of development your client application
is in, the apropriate hostname may be a UAT environment or a
production environment. A list of available hostnames is
presented further in this section.

As always, be sure to include the following HTTP header:

    Content-Type: application/json
    
The body of the request should consist of the following
JSON-encoded data (replacing `EXAMPLE_CLIENT_ID` and
`EXAMPLE_SECRET_KEY` with the correct values for your
application):

```
{
  "grant_type": "client_credentials",
  "client_id": "EXAMPLE_CLIENT_ID",
  "client_secret": "EXAMPLE_SECRET_KEY",
  "scope": "*"
}
```

If you are using curl to test your API endpoints, the preceding
API request can be performed from the command line like so:

```
curl https://<hostname>/api/v1/oauth/token/ \
  -H "Content-Type: application/json" \	
  --request POST \	
  --data '{ \
      "grant_type":"client_credentials", \
      "client_id":"EXAMPLE_CLIENT_ID", \
      "client_secret":"EXAMPLE_SECRET_KEY", \
      "scope":"*" \
    }'
```

. . again replacing `<hostname>` with an appropriate LOS API
server's hostname. If your request was successful, then you
should receive a response similar to the following:

```
{
  "meta": {
    "date": "2018-06-21T21:57:57Z",
    "function": "oauth",
    "responseCode": 200,
    "responseID": "27ee95e0-759e-11e8-bf70-5b1c7de16096",
    "success": true
  },
  "data": {
    "token_type": "Bearer",
    "expires_in": 3600,
    "access_token": "EXAMPLE_ACCESS_TOKEN"
  }
}
```

In the `data` section, the `expires_in` value identifies how
many seconds will elapse until the token is no longer valid,
while `access_token` contains the actual access token value.

## Performing an API Request

Once you have obtained an access token, you are ready to perform
your first API request. After identifying the API call you wish
to make, there are only two requirements for successfully
invoking the chosen API endpoint:

* A valid HTTP `Content-Type` header must be supplied,
  describing the format of the API request. Currently,
  only a value of `application/json` is supported.
  
* The access token must be supplied in an HTTP `Authorization`
  header with a case-sensitive value like the following:
  
  `Bearer EXAMPLE_ACCESS_TOKEN`
  
  There must be exactly one space between the word `Bearer` and
  the access token's value. Be careful to ensure that the "B" in
  "Bearer" is capitalized.
  
Note that the access token is supplied as a bearer token.
The usage of this type of token is defined in RFC 6750,
available at the following URL:

    https://tools.ietf.org/html/rfc6750

If the API request is successful, then you should receive an API
response data structure in the body of the HTTP response. The
API should return an HTTP status code of 200 (indicating
success).

If you are using curl to test the functionality of the API, then
assume you wish to access the following endpoint via an HTTP `GET`
request:

**UAT:**

    https://uatlosapi-mt.collateral360.com/api/v1/serviceRequest/fields

**Production (_to be determined, but tentatively_):**

    https://losapi.collateral360.com/api/v1/serviceRequest/fields

To do so, you would invoke curl at the command line like so
(assuming you have acquired an access token with the value
`EXAMPLE_ACCESS_TOKEN`):

**UAT:**

 ```
curl https://uatlosapi-mt.collateral360.com/api/v1/serviceRequest/fields \
  -H "Authorization: Bearer EXAMPLE_ACCESS_TOKEN" \
  -H "Content-Type: application/json"
```

**Production (_to be determined, but tentatively_):**

 ```
curl https://losapi.collateral360.com/api/v1/serviceRequest/fields \
  -H "Authorization: Bearer EXAMPLE_ACCESS_TOKEN" \
  -H "Content-Type: application/json"
```
