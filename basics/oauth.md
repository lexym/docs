# OAuth

{% hint style="info" %}
[OAuth 2.0](https://www.oauth.com/oauth2-servers/getting-ready/) is a protocol that will let your app connect to bunq users in a safe and easy way. Please be aware that if you will gain access to account information of other bunq users or initiate a payment for them, you require a PSD2 permit.
{% endhint %}

## Get started with OAuth

Follow these steps to get started with OAuth:

1. Register your OAuth Client in the bunq app, you will find the option within _Profile → Security & Settings → Developers ._
2. Add one or more Redirect URLs.
3. Get your Client ID and Client Secret from the bunq app.
4. Redirect your users to the OAuth authorization URL as described [here](https://doc.bunq.com/###authorization-request).
5. If the user accepts your Connection request then he will be redirected to the previously specified `redirect_uri` with an authorization Code parameter.
6. Use the [token endpoint](https://doc.bunq.com/###token-exchange) to exchange the authorization Code for an Access Token.
7. The Access Token can be used as a normal API Key, open a session with the bunq API or use our SDKs and get started!

## What can my apps do with OAuth?

We decided to launch OAuth with a default permission that allows you to perform the following actions:

* Read only access to the _Monetary Accounts_.
* Read access to _Payments_ & _Transactions_.
* Create new _Payments_, but only between _Monetary Accounts_ belonging to the same user.
* Create new _Draft-Payments_.
* Change the primary monetary to which a _Card_ is linked to.
* Read only access to _Request-Inquiries_ and _Request-Responses_.

## Authorization Request

Your web or mobile app should redirect users to the following URL:

`https://oauth.bunq.com/auth`

The following parameters should be passed:

* `response_type` - bunq supports the authorization code grant, provide `code` as parameter \(required\)
* `client_id` - your Client ID, get it from the bunq app \(required\)
* `redirect_uri` - the URL you wish the user to be redirected after the authorization, make sure you register the Redirect URL in the bunq app \(required\)
* `state` - a unique string to be passed back upon completion \(optional\)

#### **Authorization request example:**

```text
https://oauth.bunq.com/auth?response_type=code
&client_id=1cc540b6e7a4fa3a862620d0751771500ed453b0bef89cd60e36b7db6260f813
&redirect_uri=https://www.bunq.com
&state=594f5548-6dfb-4b02-8620-08e03a9469e6
```

#### **Authorization request response:**

```text
https://www.bunq.com/?code=7d272be434a75933f40c13d56aef6c31496005b653074f7d6ac57029d9995d30
&state=594f5548-6dfb-4b02-8620-08e03a9469e6
```

## Token Exchange

If everything went well then you can exchange the authorization Code that we returned you for an Access Token to use with the bunq API.

Make a POST call to the following endpoint:

`https://api.oauth.bunq.com/v1/token`

The following parameters should be passed as GET variables:

* `grant_type` - the grant type used, `authorization_code` for now \(required\)
* `code` - the authorization code received from bunq \(required\)
* `redirect_uri` - the same Redirect URL used in the authorisation request \(required\)
* `client_id` - your Client ID \(required\)
* `client_secret` - your Client Secret \(required\)

**Token request example:**

```text
https://api.oauth.bunq.com/v1/token?grant_type=authorization_code
&code=7d272be434a75933f40c13d56aef6c31496005b653074f7d6ac57029d9995d30
&redirect_uri=https://www.bunq.com/
&client_id=1cc540b6e7a4fa3a862620d0751771500ed453b0bef89cd60e36b7db6260f813
&client_secret=184f969765f6f74f53bf563ae3e9f891aec9179157601d25221d57f2f1151fd5
```

{% hint style="info" %}
The request only contains URL parameters.
{% endhint %}

**Example successful response:**

```text
{
    "access_token": "8baec0ac1aafca3345d5b811042feecfe0272514c5d09a69b5fbc84cb1c06029",
    "token_type": "bearer",
    "state": "594f5548-6dfb-4b02-8620-08e03a9469e6"
}
```

**Example error response:**

```text
{
    "error": "invalid_grant",
    "error_description": "The authorization code is invalid or expired."
}
```

## Using the Connect button

All good? Ready to connect to your bunq users? Refer to our style guide and use the following assets when implementing the **Connect to bunq** button.

* [Style guide](https://bunq.com/info/oauth-styleguide)
* [Connect button assets](https://bunq.com/info/oauth-connect-buttons)

{% hint style="info" %}
#### What's next?

The `access_token` you've received can be used as a normal API key. Please continue with [Authentication](https://doc.bunq.com/#authentication).

Visit us on [together.bunq.com](https://together.bunq.com), share your creations, ask question and build your very own bunq app!
{% endhint %}

