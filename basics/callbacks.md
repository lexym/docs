# Callbacks

Callbacks are used to send information about events on your bunq account to a URL of your choice, so that you can receive real-time updates.

## Notification Filters

In order to receive notifications for certain activities on your bunq account, you have to create notification filters. These can be set for your UserPerson or UserCompany, MonetaryAccount or CashRegister.

The `notification_filters` object looks like this:

```text
{
    "notification_filters": [
        {
            "notification_delivery_method": "URL",
            "notification_target": “{YOUR_CALLBACK_URL}",
            "category": "REQUEST"
        },
        {
            "notification_delivery_method": "URL",
            "notification_target": "{YOUR_CALLBACK_URL}",
            "category": "PAYMENT"
        }
    ]
}
```

### Notification Filter Fields

* `notification_delivery_method`: choose between URL \(sending an HTTP request to the provided URL\) and PUSH \(sending a push notification to user's phone\). To receive callbacks, a notification has to be set for URL.
* `notification_target`: provide the URL you want to receive the callbacks on. This URL must use HTTPS.
* `category`: provides for which type of events you would like to receive a callback.

### Callback categories

| Category | Description |
| :--- | :--- |
| BILLING | notifications for all bunq invoices |
| CARD\_TRANSACTION\_SUCCESSFUL | notifications for successful card transactions |
| CARD\_TRANSACTION\_FAILED | notifications for failed card transaction |
| CHAT | notifications for received chat messages |
| DRAFT\_PAYMENT | notifications for creation and updates of draft payments |
| IDEAL | notifications for iDEAL-deposits towards a bunq account |
| SOFORT | notifications for SOFORT-deposits towards a bunq account |
| MUTATION | notifications for any action that affects a monetary account’s balance |
| PAYMENT | notifications for payments created from, or received on a bunq account \(doesn’t include payments that result out of paying a Request, iDEAL, Sofort or Invoice\). Outgoing payments have a negative value while incoming payments have a positive value |
| REQUEST | notifications for incoming requests and updates on outgoing requests |
| SCHEDULE\_RESULT | notifications for when a scheduled payment is executed |
| SCHEDULE\_STATUS | notifications about the status of a scheduled payment, e.g. when the scheduled payment is updated or cancelled |
| SHARE | notifications for any updates or creation of Connects \(ShareInviteBankInquiry\) |
| TAB\_RESULT | notifications for updates on Tab payments |
| BUNQME\_TAB | notifications for updates on bunq.me Tab \(open request\) payments |
| SUPPORT | notifications for messages received from us through support chat |

### Mutation Category

A Mutation is a change in the balance of a monetary account. So, for each payment-like object, such as a request, iDEAL-payment or a regular payment, a Mutation is created. Therefore, the `MUTATION`category can be used to keep track of a monetary account's balance.

### Receiving Callbacks

Callbacks for the sandbox environment will be made from different IP's at AWS.  
Callbacks for the production environment will be made from 185.40.108.0/22.

_The IP addresses might change_. We will notify you in a timely fashion if such a change would take place.

### Retry mechanism

When the execution of a callback fails \(e.g. if the callback server is down or the response contains an error\) it is tried again for a maximum of 5 times, with an interval of one minute between each try. If your server is not reachable by the callback after the 6th total try, the callback is not sent anymore.

## Certificate Pinning

We recommend you use certificate pinning as an extra security measure. With certificate pinning, we check the certificate of the server on which you want to receive callbacks against the pinned certificate that has been provided by you and cancel the callback if that check fails.

#### How to set up certificate pinning

Retrieve the SSL certificate of your server using the following command:

1. `openssl s_client -servername www.example.com -connect www.example.com:443 < /dev/null | sed -n "/-----BEGIN/,/-----END/p" > www.example.com.pem`
2. `POST` the certificate to the certificate-pinned endpoint.

Now every callback that is made will be checked against the pinned certificate that you provided. Note that if the SSL certificate on your server expires or is changed, our callbacks will fail.

