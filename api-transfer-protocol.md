# Appendix 1 - API Transfer Protocol

Requests and responses issued as part of the LOS API are
transferred across the public Internet via the World Wide Web.
The requirements of this process are detailed in this section.

## Communication Protocols

Properly formatted API requests and responses are communicated
over HTTP (Hypertext Transfer Protocol), and are cryptographically
secured using TLS (Transport Layer Security). The `https` protocol
specifier is used within the LOS API system's URLs instead of
`http` because of the presence of this encryption.

The LOS API will issue HTTP response codes that are only
understood by HTTP 1.1-compliant user agents. As such, your
client application must be capable of communicating using
HTTP 1.1. 

## Authentication

Requests issued to the LOS API are only acted upon if they
contain a token indicating that the sender has been
authenticated as a known client application registered with
EDR. These tokens expire after a certain amount of time,
so client applications must periodically re-authenticate with
the LOS API system.

The authentication framework used by the LOS API is OAuth2.
The details of the authentication process are described fully
in the [Authentication](authentication.md) section of this
document. 

## Character Encodings

The LOS API uses Unicode code points encoded as UTF-8 for all
requests and responses. Its underlying system currently uses
three-byte encoding instead of the full four-byte encoding
necessary to represent all Unicode code points, so client
applications must not send code points above `U+FFFF`.

### Code Point Limitations

In technical terms, the LOS API includes support for all
characters in Unicode's basic multilingual plane, but does
not support code points above this range (_i.e._ the
supplementary multilingual plane). Pragmatically, this
means that the majority of characters currently in common
use by most human languages is handled by the API, including
the most commonly used CJK (Chinese, Japanese, and Korean)
characters. However, less commonly used CJK characters are
unsupported, as are several historical scripts, various symbols
used in mathematics, and emoji.

If your client application accepts characters with code points
higher than `U+FFFF`, then these must be stripped or otherwise
transliterated into a valid code point range before they are
sent to the LOS API.

Note particularly that any client application that allows
smart phone access (or that processes text that was originally
submitted via a smart phone) may more commonly experience
this issue, even if the client application is only localized
for languages fully supported by code points ≤ `U+FFFF`. This
is because emoji can readily be inputted via most smart
phones, and no emoji characters are currently supported by
the LOS API. 

### Limitations on the Use of Unicode

Despite the presence of Unicode support in the LOS API, certain
HTTP header values are required to be encoded in ASCII according
to web standards. For example, RFC 7231, section 3.1.1.1.
dictates that HTTP `Content-Type` headers must only contain
characters whose code points fall within range permitted by
seven-bit ASCII. Your application must respect all such
specifications when constructing HTTP requests.

## Limitations on Email Address Lengths

There is currently no universally accepted maximum length
for an email address. RFC 2821 limits the total length of
an email address to 320 octets. However, this theoretical
maximum is further limited in practice by the same RFC
to 254 octets (due to the combination of the `Path` token's
Augmented Backus-Naur Form definition in section 4.1.2 and
its length limitation defined in section 4.5.3.1).

In general, the maximum practical limit on the length of an
email address is probably 254 characters.

The LOS API does not universally use even this lower
maximum theoretical length in its internal representations
of email addresses. However, due to the impracticality of
such extremely long email addresses, no difficulty in
representing email addresses currently in use in the wild
has yet been encountered.

For most purposes where an email address is used within
the LOS API, a general limit of 100 octets can be
assumed, since this is the limit enforced for registered
users of the Collateral360 platform.
