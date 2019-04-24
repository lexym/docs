# Payment request

You want to offer bunq payments on a website or in an application.

### Scenario

In this use case the consumer and the merchant both have a bunq account. The consumer wants to pay with bunq and enters their alias in the bunq payment field at checkout. The merchant sends the request for payment to the consumer when the consumer presses enter. The consumer agrees to the request in the bunq mobile app and the merchant has immediate confirmation of the payment. 

{% hint style="info" %}
Please be aware that if you will gain access to account information of other bunq users or initiate a payment for them, you require a PSD2 permit.
{% endhint %}

### Before you start

Make sure that you have opened a session and that for any call you make after that, you pass the session’s token in the X-Bunq-Client-Authentication header.

## Call Sequence

The consumer is at checkout and selects the bunq payment method. This would be a logical time to open a session on the bunq server.

### 1. LIST monetary-account

When a request for payment is accepted, the money will be deposited on the bank account the request for payment is connected to. Let’s start by finding all your available bank accounts. Pick one of them to make the request for payment with and save its `id`.

### 2. POST monetary-account attachment \(optional\)

Optionally, you can attach an image to the request for payment.

#### **Headers**

Make sure you set the `Content-Type` header to match the MIME type of the image. It’s also required you pass a description of the image via the `X-Bunq-Attachment-Description` header.

#### **Body**

The payload of this request is the binary representation of the image file. Do not use any JSON formatting.

#### **Response**

Save the `id` of the posted attachment. You’ll need it to attach it to the request for payment.

### 3. POST request-inquiry

Next, create a request inquiry. A request inquiry is the request for payment that your customer can respond to by accepting or rejecting it.

#### **Body**

Pass the customer’s email address, phone number or IBAN in the `counterparty_alias`. Make sure you set the correct `type` for the alias, depending on what you pass. When providing an IBAN, a name of the `counterparty_alias` is required. You can provide the `id` of the created attachment.

#### **Response**

You will receive the `id` of the created request inquiry in the response. Save this `id`. You will need it to check if the customer has responded to the request yet.

### 4. GET request-inquiry

After you’ve sent the request for payment, its status can be checked.

#### **Response**

When the `status` is `ACCEPTED`, the customer has accepted and paid the request, and you will have received the money on the connected monetary account. If the `status` is `REJECTED`, the customer did not accept the request.

