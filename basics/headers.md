# Headers

HTTP headers allow your client and bunq to pass additional information along with the request or response.

Though headers are already implemented in our [SDKs](https://github.com/bunq), we recommend that you follow these instructions to make sure you set appropriate headers when calling the bunq API directly.

## Request Headers

### Mandatory request headers

#### **Cache-Control**

`Cache-Control: no-cache`

The standard HTTP Cache-Control header is required for all requests.

#### **User-Agent**

`User-Agent: bunq-TestServer/1.00 sandbox/0.17b3`

The User-Agent header field should contain information about the user agent originating the request. There are no restrictions on the value of this header.

#### **X-Bunq-Language**

`X-Bunq-Language: en_US`

The X-Bunq-Language header must contain a preferred language indication. The value of this header is formatted as a ISO 639-1 language code plus a ISO 3166-1 alpha-2 country code, separated by an underscore.

Currently only the languages en\_US and nl\_NL are supported. Anything else will default to en\_US.

#### **X-Bunq-Region**

`X-Bunq-Region: en_US`

The X-Bunq-Region header must contain the region \(country\) of the client device. The value of this header is formatted as a ISO 639-1 language code plus a ISO 3166-1 alpha-2 country code, separated by an underscore.

#### **X-Bunq-Client-Request-Id**

`X-Bunq-Client-Request-Id: a4f0de`

This header must specify an ID with each request that is unique for the logged in user. There are no restrictions for the format of this ID. However, the server will respond with an error when the same ID is used again on the same DeviceServer.

#### **X-Bunq-Geolocation**

`X-Bunq-Geolocation: 4.89 53.2 12 100 NL`

`X-Bunq-Geolocation: 0 0 0 0 000`

This header must specify the geolocation of the device. The format of this value is longitude latitude altitude radius country. The country is expected to be formatted of an ISO 3166-1 alpha-2 country code. When no geolocation is available or known the header must still be included but can be zero valued.

#### **X-Bunq-Client-Signature**

`X-Bunq-Client-Signature: XLOwEdyjF1d+tT2w7a7Epv4Yj7w74KncvVfq9mDJVvFRlsUaMLR2q4ISgT+5mkwQsSygRRbooxBqydw7IkqpuJay9g8eOngsFyIxSgf2vXGAQatLm47tLoUFGSQsRiYoKiTKkgBwA+/3dIpbDWd+Z7LEYVbHaHRKkEY9TJ22PpDlVgLLVaf2KGRiZ+9/+0OUsiiF1Fkd9aukv0iWT6N2n1P0qxpjW0aw8mC1nBSJuuk5yKtDCyQpqNyDQSOpQ8V56LNWM4Px5l6SQMzT8r6zk5DvrMAB9DlcRdUDcp/U9cg9kACXIgfquef3s7R8uyOWfKLSNBQpdVIpzljwNKI1Q`

The signature header is included for all API calls except for POST /v1/installation. See the signing page for details on how to create this signature.

#### **X-Bunq-Client-Authentication**

`X-Bunq-Client-Authentication: 622749ac8b00c81719ad0c7d822d3552e8ff153e3447eabed1a6713993749440`

The authentication token is used to authenticate the source of the API call. It is required by all API calls except for POST /v1/installation. It is important to note that the device and session calls are using the token from the response of the installation call, while all the other calls use the token from the response of the session-server call

### Attachment headers

#### **Content-Type**

`Content-Type: image/jpeg`

This header should be used when uploading an attachment to pass its MIME type. Supported types are: image/png, image/jpeg and image/gif.

#### **X-Bunq-Attachment-Description**

X-Bunq-Attachment-Description: Check out these cookies. This header should be used when uploading an Attachment's content to give it a description.

## Response Headers

### All Responses

#### **X-Bunq-Client-Request-Id**

`X-Bunq-Client-Request-Id: a4f0de`

The same ID that was provided in the request's X-Bunq-Client-Request-Id header. Is included in the response \(and request\) signature, so can be used to ensure this is the response for the sent request.

#### **X-Bunq-Client-Response-Id**

`X-Bunq-Client-Response-Id: 76cc7772-4b23-420a-9586-8721dcdde174`

A unique ID for the response formatted as a UUID. Clients can use it to add extra protection against replay attacks.

#### **X-Bunq-Server-Signature**

`X-Bunq-Server-Signature: XBBwfDaOZJapvcBpAIBT1UOmczKqJXLSpX9ZWHsqXwrf1p+H+eON+TktYksAbmkSkI4gQghw1AUQSJh5i2c4+CTuKdZ4YuFT0suYG4sltiKnmtwODOFtu1IBGuE5XcfGEDDSFC+zqxypMi9gmTqjl1KI3WP2gnySRD6PBJCXfDxJnXwjRkk4kpG8Ng9nyxJiFG9vcHNrtRBj9ZXNdUAjxXZZFmtdhmJGDahGn2bIBWsCEudW3rBefycL1DlpJZw6yRLoDltxeBo7MjgROBpIeElh5qAz9vxUFLqIQC7EDONBGbSBjaXS0wWrq9s2MGuOi9kJxL2LQm/Olj2g==`

The server's signature for this response. See the signing page for details on how to verify this signature.

### Warning header

#### **X-Bunq-Warning**

`X-Bunq-Warning: "You have a negative balance. Please check the app for more details."`

Used to inform you on situations that might impact your bunq account and API access.  


