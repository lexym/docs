# API Conventions

Make sure to follow these indications when using the bunq API or get started with our SDKs.

### Responses

All JSON responses have one top level object. In this object will be a Response field of which the value is always an array, even for responses that only contain one object.

Example response body:

```text
{
    "Response": [
        {
            "DataObject": {}
        }
    ]
}
```

### Errors

* Error responses also have one top level Error object.
* The contents of the array will be a JSON object with an error\_description and error\_description\_translated field.
* The error\_description is an English text indicating the error and the error\_description\_translated field can be shown to end users and is translated into the language from the X-Bunq-Language header, defaulting to en\_US.
* When using bunq SDKs, error responses will be always raised in form of an exception.

Example response body

```text
{
    "Error": [
        {
            "error_description": "Error description",
            "error_description_translated": "User facing error description"
        }
    ]
}
```

### Object Type Indications

When the API returns different types of objects for the same field, they will be nested in another JSON object that includes a specific field for each one of them. Within bunq SDKs a BunqResponse object will be returned as the top level object.

In this example there is a field content, which can have multiple types of objects as value such as — in this case — ChatMessageContentText. Be sure to follow this convention or use bunq SDKs instead.

```text
{
    "content": {
        "ChatMessageContentText": {
            "text": "Hi! This is an automated security message. We saw you just logged in on an My Device Description. If you believe someone else logged in with your account, please get in touch with Support."
        }
    }
}
```

### Time Formats

Times and dates being sent to and from the API are in UTC. The format that should be used is `YYYY-MM-DD hh:mm:ss.ssssss`, where the letters have the meaning as specified in ISO 8601. For example: `2017-01-13 13:19:16.215235`.  


