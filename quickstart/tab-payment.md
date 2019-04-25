# Tab payment

{% hint style="info" %}
A **tab payment** is a payment made from a browser tab. 
{% endhint %}

This tutorial will help you create a tab that can be paid once by a single user \(a so-called TagUsageSingle\). It explains two ways to make the Tab visible to your customers:

* QR code from the CashRegister
* QR code from the Tab.

### Before you start

* [ ] Opened a session 
* [ ] Pass the session _Token_ in the `X-Bunq-Client-Authentication` header in every following request of this session.

## Call Sequence

### 1. POST /attachment-public

Start by creating an attachment that will be used for the avatar for the cash register.

#### **Header**

* Make sure you set the `Content-Type` header to match the MIME type of the image. It is also required you pass a description of the image via the `X-Bunq-Attachment-Description` header.

#### **Body**

The payload of this request is the binary representation of the image file. Do not use any JSON formatting.

#### **Response**

Save the `uuid` of the posted attachment. You'll need it to create the avatar in the next step.

### 2. POST avatar

Make an avatar using the public attachment you've just created.

#### **Body**

The payload of this request is the `uuid` of the attachment public.

#### **Response**

In response, you’ll receive the UUID of the avatar created using the attachment. Save this UUID. You’ll use it as the avatar for the cash register you're about to create.

### 3. LIST monetary-account

Get a listing of all available monetary accounts. Choose one, and save the id of the monetary account you want your cash register to be connected to. Each paid tab for the cash register will transfer the money to this account.

### 4a. POST cash-register

Create a cash register. Use the `id` of the monetary account you want to connect the cash register to in the URL of the request.

#### **Body**

In the body provide the `uuid` of the avatar you created for this cash register. Also make sure to provide a unique name for your cash register. Set the status to `PENDING_APPROVAL`.

#### **Response**

The response contains the `id` of the cash register you created. Save this `id`. You will need it to create subsequent tabs and tab items.

### 4b. Wait for approval

On the production environment, a bunq admin will review and approve your cash register. In the sandbox environment, your cash register will be automatically approved.

### 5. POST tab-usage-single

Create a new tab that is connected to your cash register. Use the id of the cash register you want to connect this tab to in the URL of your request.

#### **Body**

Give the tab a name in `merchant_reference`. Create the tab with status `OPEN`, and give the tab a starting amount. You can update this amount later.

#### **Response**

The response contains the uuid of the tab you created.

### 6. POST tab-item \(optional\)

You can add items to a tab. For instance, if a customer will be paying for multiple products via this tab, you can decide to add an item for each of these. Adding items to a tab is optional, and adding them will not change the total amount of the tab itself. However, if you've added any tab items the sum of the amounts of these items must be equal to the `total_amount` of the tab when you change its status to `WAITING_FOR_PAYMENT`.

### 7. PUT tab-usage-single

Update the status of the tab to `WAITING_FOR_PAYMENT` if you want the costumer to pay the tab, and you're done adding any tab items. You can use this request to make the tab visible for your costumers.

#### **Visibility**

To decide how you are going to make your tab visible, pass a visibility object in the payload.

Setting `cash_register_qr_code` to true will connect this tab to the QR code from the cash register. If this cash register does not have a QR code yet, one will be created. Only one Tab can be connected to the cash register’s QR code at any given time.

Setting `tab_qr_code` to true will create a QR code specifically for this tab. This QR code can not be linked to anything else.

