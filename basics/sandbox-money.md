# Sandbox money

Without money, it's not always sunny in the sandbox world. Fortunately, getting money on the bunq sandbox is easy. All you need to do is ask Sugar Daddy for it.

Send a [**POST v1/request-inquiry**](https://doc.bunq.com/#/request-inquiry/Create_RequestInquiry_for_User_MonetaryAccount) request passing _sugardaddy@bunq.com_  in the `counterparty_alias` field. Specify the `type` for the alias and set the `allow_bunqme` field.

{ "amount\_inquired": { "value": "100", "currency": "EUR" }, "counterparty\_alias": { "type": "EMAIL", "value": "sugardaddy@bunq.com", "name": "Sugar Daddy" }, "description": "Gimme some", "allow\_bunqme": false }

```text
{
    "amount_inquired": {
        "value": "100",
        "currency": "EUR"
    },
    "counterparty_alias": {
        "type": "EMAIL",
        "value": "sugardaddy@bunq.com",
        "name": "Sugar Daddy"
    },
    "description": "Gimme some",
    "allow_bunqme": false
}
```

