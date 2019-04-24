# Pagination

In order to control the size of the response of a `LIST` request, items can be paginated. A `LIST`request is a request for every one of a certain resources, for instance all payments of a certain monetary account `GET /v1/user/1/monetary-account/1/payment`\). You can decide on the maximum amount of items of a response by adding a `count` query parameter with the number of items you want per page to the URL. For instance:

`GET /v1/user/1/monetary-account/1/payment?count=25`

When no `count` is given, the default count is set to 10. The maximum `count` you can set is 200.

With every listing, a `Pagination` object will be added to the response, containing the URLs to be used to get the next or previous set of items. The URLs in the Pagination object can be used to navigate through the listed resources. The Pagination object looks like this given a count of 25:

```text
{
    "Pagination": {
        "future_url": null,
        "newer_url": "/v1/user/1/monetary-account/1/payment?count=25&newer_id=249",
        "older_url": "/v1/user/1/monetary-account/1/payment?count=25&older_id=224"
    }
}
```

The `newer_url` value can be used to get the next page. The `newer_id` is always the ID of the last item in the current page. If `newer_url` is `null`, there are no more recent items before the current page.

The `older_url` value can be used to get the previous page. The `older_id` is always the ID of the first item in the current page. If `older_url` is `null`, there are no older items after the current page.

The `future_url` can be used to refresh and check for newer items that didn't exist when the listing was requested. The `newer_id` will always be the ID of the last item in the current page. `future_url`will be `null` if `newer_id` is not also the ID of the latest item.

