# Authentication

Here is what you need to know about how the bunq authentication works:

* We use encryption for all API calls. This means that all requests must use HTTPS. The HTTP  calls will fail. 
* Use SSL Certificate Pinning and Hostname Verification to ensure your connection with bunq is secure.
* To make API calls, you need to register a device and open a session.
* API calls must contain a valid authentication token in the headers.
* The auto logout time that you set applies to all your sessions. If a request is made 30 minutes before a session expires, the session will automatically be extended.

{% hint style="info" %}
We use [asymmetric cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) for signing requests and encryption.

The client _\(you\)_ and the server _\(bunq\)_ must have a pair of keys: a private key and a public key. You need to pre-generate your own pair of 2048-bit [RSA keys](https://en.wikipedia.org/wiki/RSA_%28cryptosystem%29) in the [PEM format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail).

The parties _\(you and bunq\)_ exchange their public keys in the first step of the API context creation flow. All the following requests must be signed by both your application and the server. Pass your signature in the _X-Bunq-Client-Signature_ header, and the server will return its signature in the _X-Bunq-Server-Signature_ header.
{% endhint %}

## Creating API Context 

Before you can start calling the bunq API, you must do the following:

* register your API key and IP address\(es\);
* register your device;
* create a session. 

We call this intro "creating an API context". There are a couple of ways to carry out the sequence of steps listed in the :

#### Using our SDKs

1. Go to our [GitHub](https://github.com/bunq) page.
2. Choose the SDK in your language of choice.
3. Find the part dedicated to creating an API context.

#### Using our API directly

1. Create an _Installation_ with the installation POST call and provide a new public key. After doing so you receive an authentication token which you can use for the API calls in the next steps.
2. Create a _DeviceServer_ with the device-server POST call and provide a description and API key.
3. Create a _SessionServer_ with the session-server POST call. After doing so you receive a new authentication token which you can use for the API calls during this active Session.​

#### IP addresses

When using a standard API Key the DeviceServer and Installation that are created in this process are bound to the IP address they are created from. Afterwards it is only possible to add IP addresses via the Permitted IP endpoint.

Using a Wildcard API Key gives you the freedom to make API calls from any IP address after the POST device-server. You can switch to a Wildcard API Key by tapping on “Allow All IP Addresses” in your API Key menu inside the bunq app. You can also programatically switch to a Wildcard API Key by passing your current ip and a `*` \(asterisk\) in the `permitted_ips` field of the device-server POST call. E.g: `["1.2.3.4", "*"]`.

