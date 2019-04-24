# Authentication

* We use encryption for all API calls. This means that all requests must use HTTPS. The HTTP standard calls will fail. You should also use SSL Certificate Pinning and Hostname Verification to ensure a secure connection with bunq.
* In order to make API calls you need to register a device and open a session.
* We use RSA Keys for signatures headers and encryption.
* API calls must contain a valid authentication token in the headers.
* The auto logout time that you've set for your user account is also effective for your sessions. If a request is made 30 minutes before a session expires, the session will automatically be extended.

## Device Registration

#### Using our SDKs

1. In order to start making calls with the bunq API, you must first register your API key and device and create a session.
2. In the SDKs, we group these actions and call it "creating an API context".
3. You can find more information on our [GitHub](https://github.com/bunq) page.

#### Using our API

1. Create an _Installation_ with the installation POST call and provide a new public key. After doing so you receive an authentication token which you can use for the API calls in the next steps.
2. Create a _DeviceServer_ with the device-server POST call and provide a description and API key.
3. Create a _SessionServer_ with the session-server POST call. After doing so you receive a new authentication token which you can use for the API calls during this active Session.​

#### IP addresses

When using a standard API Key the DeviceServer and Installation that are created in this process are bound to the IP address they are created from. Afterwards it is only possible to add IP addresses via the Permitted IP endpoint.

Using a Wildcard API Key gives you the freedom to make API calls from any IP address after the POST device-server. You can switch to a Wildcard API Key by tapping on “Allow All IP Addresses” in your API Key menu inside the bunq app. You can also programatically switch to a Wildcard API Key by passing your current ip and a `*` \(asterisk\) in the `permitted_ips` field of the device-server POST call. E.g: `["1.2.3.4", "*"]`.

