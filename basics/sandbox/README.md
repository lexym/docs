# Sandbox

## Sandbox API keys

There 3 ways to generate a bunq sanbox API key:

1. create in from [the sandbox app](https://doc.bunq.com/#/android-emulator)
2. connect to Tinker _\(it will auto connect you to the sandbox\)_
3. run the cURL command

`curl https://public-api.sandbox.bunq.com/v1/sandbox-user -X POST --header "Content-Type: application/json" --header "Cache-Control: none" --header "User-Agent: curl-request" --header "X-Bunq-Client-Request-Id: $(date)randomId" --header "X-Bunq-Language: nl_NL" --header "X-Bunq-Region: nl_NL" --header "X-Bunq-Geolocation: 0 0 0 0 000"`

## Sandbox money

Without money, it's not always sunny in the sandbox world. Fortunately, getting money on the bunq sandbox is easy. All you need to do is ask Sugar Daddy for it.

Send a [**POST v1/request-inquiry**](https://doc.bunq.com/#/request-inquiry/Create_RequestInquiry_for_User_MonetaryAccount) request passing _sugardaddy@bunq.com_  in the `counterparty_alias` field. Specify the `type` for the alias and set the `allow_bunqme` field.

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

