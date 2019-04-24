# Errors

Familiar HTTP response codes are used to indicate the success or failure of an API request.

Generally speaking, codes in the 2xx range indicate success, while codes in the 4xx range indicate an error having to do with provided information \(e.g. a required parameter was missing, insufficient funds, etc.\).

Finally, codes in the 5xx range indicate an error with bunq servers. If this is the case, please stop by the support chat and report it to us.

## Response Codes

| Code | Error | Description |
| :--- | :--- | :--- |
| 200 | OK | Successful HTTP request |
| 399 | NOT MODIFIED | Same as a 304, it implies you have a local cached copy of the data |
| 400 | BAD REQUEST | Most likely a parameter is missing or invalid |
| 401 | UNAUTHORISED | Token or signature provided is not valid |
| 403 | FORBIDDEN | You're not allowed to make this call |
| 404 | NOT FOUND | The object you're looking for cannot be found |
| 405 | METHOD NOT ALLOWED | The method you are using is not allowed for this endpoint |
| 429 | RATE LIMIT | Too many API calls have been made in a too short period |
| 490 | USER ERROR | Most likely a parameter is missing or invalid |
| 491 | MAINTENANCE ERROR | bunq is in maintenance mode |
| 500 | INTERNAL SERVER ERROR | Something went wrong on bunq's end |

All errors 4xx code errors will include a JSON body explaining what went wrong.

## Rate Limits

If you are receiving the error 429, please make sure you are sending requests at rates that are below our rate limits.

Our rate limits per IP address per endpoint:

* GET requests: 3 within any 3 consecutive seconds
* POST requests: 5 within any 3 consecutive seconds
* PUT requests: 2 within any 3 consecutive seconds

We have a lower rate limit for `/session-server`: 1 request within 30 consecutive seconds.

