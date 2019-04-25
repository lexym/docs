# Signing

To avoid modification of API call data while in transit \(i.e. man-in-the-middle attacks\), we use a request/response signing system. The signature ensures that the data is coming from the party that has the correct private key.

While this system is already implemented in our SDKs, you should always follow the guidelines on this page when using the bunq API to make sure you correctly sign your calls.

The signatures are created using the SHA256 cryptographic hash function and included \(encoded in base 64\) in the `X-Bunq-Client-Signature` request header and `X-Bunq-Server-Signature`response header. The data to sign is the following:

<table>
  <thead>
    <tr>
      <th style="text-align:left">WHERE</th>
      <th style="text-align:left">DETAILS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Requests</td>
      <td style="text-align:left">
        <p></p>
        <ul>
          <li>the request method, capitalized</li>
          <li>request endpoint URL (including the query string, if any)</li>
          <li>do not use the full URL
            <ul>
              <li><code>POST /v1/user</code> will works</li>
              <li><code>POST https://sandbox.public.api.bunq.com/v1/user</code> will not.</li>
            </ul>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Responses</td>
      <td style="text-align:left">
        <p></p>
        <ul>
          <li>the response code</li>
          <li>a <code>\n</code> (linefeed) newline separator</li>
          <li></li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>* Headers, sorted alphabetically by key, with key and value separated by `:` \(a colon followed by a space\) and only including `Cache-Control`, `User-Agent` and headers starting with `X-Bunq-`. The headers should be separated from each other with a `\n` \(linefeed\) newline. For a full list of required call headers, see the headers page.
* Two `\n` \(linefeed\) newlines \(even when there is no body\).
* The request or response body.
* For signing requests, the client must use the private key corresponding to the public key that was sent to the server in the installation API call. That public key is what the server will use to verify the signature when it receives the request. In that same call the server will respond with a server side public key, which the client must use to verify the server's signatures. The generated RSA key pair must have key lengths of 2048 bits and adhere to the PKCS \#8 standard.

## Request signing example

Consider the following request, a `POST` to `/v1/user/126/monetary-account/222/payment` \(the JSON is formatted with newlines and indentations to make it more readable\):

| Header | Value |
| :--- | :--- |
| Cache-Control:     | no-cache |
| User-Agent: | bunq-TestServer/1.00 sandbox/0.17b3 |
| X-Bunq-Client-Authentication: | f15f1bbe1feba25efb00802fa127042b54101c8ec0a524c36464f5bb143d3b8b |
| X-Bunq-Client ****Request-Id: | 57061b04b67ef |
| X-Bunq-Client-Signature: | UINaaJELGHekiye4JExGx6TCs2lKMta74oVlZlwVNuVD6xPpH7RS6H58C21MmiQ75/MSVjUePC8gBjtARW2HpUKN7hANJqo/UtDb7mgDMsuz7Cf/hKeUCX0T55w2X+NC3i1T+QOQVQ1gALBT1Eif6qgyyY1wpWJUYft0MmCGEYg/ao9r3g026DNlRmRpBVxXtyJiOUImuHbq/rWpoDZRQTfvGL4KH4iKV4Lcb+o9lw11xOl4LQvNOHq3EsrfnTIa5g80pg9TS6G0SvjWmFAXBmDXatqfVhImuKZtd1dQI12JNK/++isBsP79eNtK1F5rSksmsTfAeHMy7HbfAQSDbg== |
| X-Bunq-Geolocation: | 0 0 0 0 NL |
| X-Bunq-Language: | en\_US |
| X-Bunq-Region: | en\_US |



```text
{
    "amount": {
        "value": "12.50",
        "currency": "EUR"
    },
    "counterparty_alias": {
        "type": "EMAIL",
        "value": "bravo@bunq.com"
    },
    "description": "Payment for drinks."
}
```

Let's sign that request. First create a variable `$dataToSign`, starting with the type and endpoint url. Follow that by a list of headers only including `Cache-Control`, `User-Agent` and headers starting with `X-Bunq-`. Add an extra \(so double\) linefeed after the list of headers. Finally end with the body of the request:

`POST /v1/user/126/monetary-account/222/payment`

| Header | Value |
| :--- | :--- |
| Cache-Control: | no-cache |
| User-Agent: | bunq-TestServer/1.00 sandbox/0.17b3 |
| X-Bunq-Client-Authentication: | f15f1bbe1feba25efb00802fa127042b54101c8ec0a524c36464f5bb143d3b8b |
| X-Bunq-Client-Request-Id: | 57061b04b67ef |
| X-Bunq-Client-Signature: | UINaaJELGHekiye4JExGx6TCs2lKMta74oVlZlwVNuVD6xPpH7RS6H58C21MmiQ75/MSVjUePC8gBjtARW2HpUKN7hANJqo/UtDb7mgDMsuz7Cf/hKeUCX0T55w2X+NC3i1T+QOQVQ1gALBT1Eif6qgyyY1wpWJUYft0MmCGEYg/ao9r3g026DNlRmRpBVxXtyJiOUImuHbq/rWpoDZRQTfvGL4KH4iKV4Lcb+o9lw11xOl4LQvNOHq3EsrfnTIa5g80pg9TS6G0SvjWmFAXBmDXatqfVhImuKZtd1dQI12JNK/++isBsP79eNtK1F5rSksmsTfAeHMy7HbfAQSDbg== |
| X-Bunq-Geolocation: | 0 0 0 0 NL |
| X-Bunq-Language: | en\_US |
| X-Bunq-Region: | en\_US |

```text
{
    "amount": {
        "value": "12.50",
        "currency": "EUR"
    },
    "counterparty_alias": {
        "type": "EMAIL",
        "value": "bravo@bunq.com"
    },
    "description": "Payment for drinks."
}
```

Next, create the signature of `$dataToSign` using the SHA256 algorithm and the private key `$privateKey` of the Installation's key pair. In PHP, use the following to create a signature. The signature will be passed by reference into `$signature`.

`openssl_sign($dataToSign, $signature, $privateKey, OPENSSL_ALGO_SHA256);`

Encode the resulting `$signature` using base64, and add the resulting value to the request under the header `X-Bunq-Client-Signature`. You have now signed your request, and can send it!  


## Response verifying example

The response to the previous request is as follows \(the JSON is formatted with newlines and indentations to make it more readable\):

| Header | Value |
| :--- | :--- |
| Access-Control-Allow-Origin: | \* |
| Content-Type: | application/json |
| Date: | Thu, 07 Apr 2016 08:32:04 GMT |
| Server: | APACHE |
| Strict-Transport-Security: | max-age=31536000 |
| Transfer-Encoding: | chunked |
| X-Bunq-Client-Response-Id: | 89dcaa5c-fa55-4068-9822-3f87985d2268 |
| X-Bunq-Client-Request-Id: | 57061b04b67ef |
| X-Bunq-Server-Signature: | ee9sDfzEhQ2L6Rquyh2XmJyNWdSBOBo6Z2eUYuM4bAOBCn9N5vjs6k6RROpagxXFXdGI9sT15tYCaLe5FS9aciIuJmrVW/SZCDWq/nOvSThi7+BwD9JFdG7zfR4afC8qfVABmjuMrtjaUFSrthyHS/5wEuDuax9qUZn6sVXcgZEq49hy4yHrV8257I4sSQIHRmgds4BXcGhPp266Z6pxjzAJbfyzt5JgJ8/suxgKvm/nYhnOfsgIIYCgcyh4DRrQltohiSon6x1ZsRIfQnCDlDDghaIxbryLfinT5Y4eU1eiCkFB4D69S4HbFXYyAxlqtX2W6Tvax6rIM2MMPNOh4Q== |
| X-Frame-Options: | SAMEORIGIN |

```text
{
    "Response": [
        {
            "Id": {
                "id": 1561
            }
        }
    ]
}
```

Now we need to verify that this response actually came from the server and not from a man-in-the-middle. So, first we built the data that is to be verified, starting with the response code \(200\). Follow this by a list of the bunq headers \(sorted alphabetically and excluding the signature header itself\). Note: you should only include headers starting with X-Bunq-, so omit headers like Cache-Control for the verification of the response. Finally, add two line feeds followed by the response body. Note: The headers might change in transit from `X-Header-Capitalization-Style` to `x-header-non-capitalization-style`. Make sure you change them to `X-Header-Capitalization-Style` before verifying the response signature.

```text
200
X-Bunq-Client-Request-Id: 57061b04b67ef
X-Bunq-Server-Response-Id: 89dcaa5c-fa55-4068-9822-3f87985d2268

{"Response":[{"Id":{"id":1561}}]}
```

Now, verify the signature of `$dataToVerify` using the SHA256 algorithm and the public key `$publicKey` of the server. In PHP, use the following to verify the signature.

`openssl_sign($dataToVerify, $signature, $publicKey, OPENSSL_ALGO_SHA256);`

## Troubleshooting

If you get this error `The request signature is invalid`, please check the following:

* There are no redundant characters \(extra spaces, trailing line breaks, etc.\) in the data to sign.
* In your data to sign, you have used only the endpoint URL, for instance POST /v1/user, and not the full url, for instance `POST https://sandbox.public.api.bunq.com/v1/user`
* You only added the headers `Cache-Control`, `User-Agent` and headers starting with `X-Bunq-`.
* In your data to sign, you have sorted the headers alphabetically by key, ascending.
* There is a colon followed by a space `:` separating the header key and value in your data to sign.
* There is an extra line break after the list of headers in the data to sign, regardless of whether there is a request body.
* Make sure the body is appended to the data to sign exactly as you're adding it to the request.
* In your data to sign, you have not added the `X-Bunq-Client-Signature` header to the list of headers \(that would also be impossible\).
* You have added the full body to the data to sign.
* You use the data to sign to create a SHA256 hash signature.
* You have base64 encoded the SHA256 hash signature before adding it to the request under `X-Bunq-Client-Signature`.

