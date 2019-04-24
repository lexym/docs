# Opening a Session

So, you want to start using the bunq API, awesome! To do this, you have to open a session in which you will be making those calls.

## Getting an API key

To connect to the API, you have to make sure you have received an API key.

For the production environment, you can generate your own keys in the bunq app \(under 'Profile' -&gt; 'Security'\).

For the sandbox environment you can get an API key from tinker and android emulator as [described above](https://doc.bunq.com/#topic-android-emulator).

Alternative you can do a curl request: `curl https://public-api.sandbox.bunq.com/v1/sandbox-user -X POST --header "Content-Type: application/json" --header "Cache-Control: none" --header "User-Agent: curl-request" --header "X-Bunq-Client-Request-Id: $(date)randomId" --header "X-Bunq-Language: nl_NL" --header "X-Bunq-Region: nl_NL" --header "X-Bunq-Geolocation: 0 0 0 0 000"`. That'll create a sample user and return an associated API key for you.

Note that production API key is only usable on production and sandbox key is only usable on sandbox. Sandbox key has a `sandbox_` prefix while production key does not have any noticeable prefixes.

## Call Sequence

The calls you need to perform to set up a session from scratch are the following:

### 1. POST installation

Each call needs to be signed with your own private key. An Installation is used to tell the server about the public key of your key pair. The server uses this key to verify your subsequent calls.

Start by generating a 2048-bit RSA key pair. You can find examples by looking at the source code of the sdk's located at github.

#### **Headers**

On the headers page you can find out about the mandatory headers. Take care that if you are in the sandbox environment, you set an `Authorization` header. Specific to the `POST /installation`call, you shouldn't use the `X-Bunq-Client-Authentication` or the `X-Bunq-Client-Signature`headers.

#### **Body**

Post your public key to the Installation endpoint \(use `\n` for newlines in your public key\).

#### **Response**

Save the Installation token and the bunq API's public key from the response. This token is used in the `Authentication` header to register a `DeviceServer` and to start a `SessionServer`. The bunq API's public key should be used to verify future responses received from the bunq API.

### 2. POST device-server

Further calls made to the server need to come from a registered device. `POST /device-server`registers your current device and the IP address\(es\) it uses to connect to the bunq API.

#### **Headers**

Use the token you received from `POST /installation` in the `X-Bunq-Client-Authentication`header. Make sure you sign your call, passing the call signature in `X-Bunq-Client-Signature`header.

#### **Body**

For the secret, use the API key you received. If you want to create another API key, you can do so in the bunq sandbox app \(or production app for the production environment\). Login, go to Profile &gt; Security and tap 'API keys'. The freshly created API key can be assigned to one or multiple IP addresses using `POST device-server` within 4 hours before becoming invalid. As soon as you start using your API key, it will remain valid until the next sandbox reset.

For the secret, use the API key you received.

### 3. POST session-server

To make any calls besides `installation` and `device-server`, you need to open a session.

#### **Headers**

Use the token you received from `POST /installation` in the `X-Bunq-Client-Authentication`header. Make sure you sign your call, passing the call signature in `X-Bunq-Client-Signature`header.

#### **Body**

For the secret, use the API key you received.

#### **Response**

The token received in the response to `POST /session-server` should be used to authenticate your calls in this session. Pass this session's token in the `X-Bunq-Client-Authentication` header on every call you make in this session.

