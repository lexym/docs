# Release notes

## Upcoming changes

The _type_ field is becoming mandatory for [_card-debit_](https://doc.bunq.com/#/card-debit) __**starting from July 2, 2019.**

We are deprecating the _applied\_limit_ field from the [mastercard\_action](https://doc.bunq.com/#/mastercard-action) responses on **July 2, 2019**.

We are removing the _activation\_code_ field from [CardUpdate](https://doc.bunq.com/#/card/Update_Card_for_User). Instead of using the field, set the status ACTIVE when the card has been ACCEPTED\_FOR\_PRODUCTION.

We are deprecating the following share-invite-bank endpoints on **July 9, 2019**:

* [share-invite-bank-inquiry](https://doc.bunq.com/#/share-invite-bank-inquiry)
* share-invite-bank-inquiry/{ID}/{amount-used\)
* [share-invite-bank-response](https://doc.bunq.com/#/share-invite-bank-response)

You can use the following endpoints instead:

* share-invite-monetary-account-inquiry
* share-invite-monetary-account-inquiry/ID/amount-used
* share-invite-monetary-account-response

## 2019-04-18

### Added

* The old card limit fields in the [_card_](https://doc.bunq.com/#/card) and [_card-batch_](https://doc.bunq.com/#/card-batch) objects have been deprecated and replaced with new ones

![](../.gitbook/assets/screenshot-2019-04-10-at-12.34.11%20%282%29.png)

* The default limit of €1,000 is now applied to all new cards.



