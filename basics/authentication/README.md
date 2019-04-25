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

The parties _\(you and bunq\)_ exchange their public keys in the first step of the [API context creation flow](https://lexy.gitbook.io/bunq/basics/authentication#creating-api-context). All the following requests must be signed by both your application and the server. Pass your signature in the _X-Bunq-Client-Signature_ header, and the server will return its signature in the _X-Bunq-Server-Signature_ header.
{% endhint %}

## Creating API Context 

Before you can start calling the bunq API, you must do the following:

* register your API key and IP address\(es\);
* register your device;
* create a session. 

We call this intro "creating an API context". There are a couple of ways to carry out the sequence of steps. Let's dwell on each of them.

### Using our SDKs

1. Go to our [GitHub](https://github.com/bunq) page.
2. Choose the SDK in your language of choice.
3. Find and use the part dedicated to creating an API context.

### Using our API directly

1. Create an _Installation_ by calling `POST v1/installation` and passing your pre-generated public key. You will receive an installation _Token._ Use it when making the two following API calls.
2. Create a _DeviceServer_ via `POST v1/device-server`. Provide a description and a secret \(API key in this case\).
3. Create a _SessionServer_ by executing `POST v1/session-server`. You will receive an authentication _Token._ Use it in the API requests in this active session.​

{% hint style="info" %}
[Use Postman](https://github.com/bunq/postman) to use our pre-setup API context creation calls.
{% endhint %}

### Using Tinker

1. [Run a Tinker command](https://lexy.gitbook.io/bunq/quickstart/tinker).
2. Find and reuse the code that creates an API context.

### IP addresses

The DeviceServer and Installation you create in the first two steps of the API context creation flow, are bound to the IP address\(es\) you register them from. It is then only possible to add trusted IP addresses using the [IP endpoint](https://doc.bunq.com/#/ip/Create_Ip_for_User_CredentialPasswordIp).

{% hint style="info" %}
You can also use a [Wildcard API Key](https://together.bunq.com/d/1997-the-new-wildcard-api-key). It gives you the freedom to make API calls from any IP address after calling `POST v1/device-server`. 

To switch to a Wildcard API Key, tap on _Allow All IP Addresses_ in the API Key menu in the bunq app. You can also do it programatically. Just pass your current IP and a `*` \(asterisk\) in the `permitted_ips` field of the `POST v1/device-server`call. 

**Example:**`["1.2.3.4", "*"]`
{% endhint %}

