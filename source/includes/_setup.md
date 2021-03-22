# Authentication

> To authorize, use this code:

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/invoices.json \
  -u YOUR_API_KEY:x
```

> You can also `ping` to check if the service is up, your credentials are correct, or to know your remaining requests without doing an actual request:

```shell
curl https://quadernoapp.com/api/ping \
  -u YOUR_API_KEY:x
```


Quaderno uses an API key to authorise all requests, allowing access to the API in combination with the account name in the endpoint URL. For more info on finding your API key, check [here](https://support.quaderno.io/how-do-i-get-my-api-key/).

Quaderno expects the API key to be included via HTTP Basic Auth in all API requests to the server, in a header that looks like the following:

`-u YOUR_API_KEY:x`

<aside class="warning">
  Remember that anyone who has your private key can see and change everything that you have access to. Make sure to keep it, and your password, safe.
</aside>






# Authorization

```shell
curl https://quadernoapp.com/api/authorization \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
{
    "id": "999",
    "name": "Sheldon Cooper",
    "email": "s.cooperphd@yahoo.com",
    "href": "http://nippur-999.quadernoapp.com/api/"
}
```

If you don't know the `ACCOUNT_NAME` for your target account, you can get it with the `authorization` API call.

This returns the following as a JSON payload:

Parameter     | Description
--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
`id`          | An identity, which is *not* used for determining who this user is within Quaderno. The `id` field should therefore *not* be used for submitting data within Quaderno's API.
`name`        | The user's full name.
`email`       | The user's email address.
`href`        | The custom API endpoint URL for the user, providing the sought-after `ACCOUNT_NAME` between `http://` and `.quadernoapp...`.


<aside class="notice">
  Note that in all cases, if the user does not have permission to do something, you'll get a 401 Unauthorized error.
</aside>








# Rate Limiting

To make it easier to determine if your application is being rate-limited, or is approaching that level, we have the following HTTP headers on our successful responses:

- `X-RateLimit-Reset`
- `X-RateLimit-Remaining`

There is a limit of **100 API calls per 15 seconds**.

We reserve the right to tune the limitations, but we promise to keep them high enough to allow a well-behaving interactive app to do it's job.

If you exceed the limit you will receive a `HTTP 429 (Too Many Requests)`.





# Pagination

## Cursor based pagination

> A call with the `since_id` parameter set:

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/contacts.json?since_id=42 \
  -u YOUR_API_KEY:x
```

As of API version `20210316`, pagination is performed with a `since_id` parameter. This parameter takes an existing object ID value and returns objects listed after the named object, in reverse chronological order.

The HTTP header `X-Pages-HasMore` indicates whether more records can be fetched by using the same query with a lower `since_id`.

Bear in mind that Quaderno paginates `GET` index results.

You can change the number of objects to be returned with the `limit` parameter, defaulting to `25`. This value is capped at 100 objects.

## Page parameter (legacy)

> **Legacy**: A call with the `page` parameter set:

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/contacts.json?page=2 \
  -u YOUR_API_KEY:x
```

<aside class="warning">
The following use case is legacy and will be discontinued on 2nd August 2021. You can keep using it requesting to <a href="#versioning">use the API version `20170914`</a>. Please update your code to use the new <a href="#cursor-based-pagination">cursor based pagination</a>.
</aside>

These HTTP headers inform you about the page context:

- `X-Pages-CurrentPage`
- `X-Pages-TotalPages`

You can change the page by passing the `page` parameter, defaulting to `1`.

You can change the number of objects to be returned with the `limit` parameter, defaulting to `25`. This value is capped at 100 objects.







# Errors

We don't usually have any trouble on our end, but when we do we'll let you know!

The Quaderno API uses the following error codes:

Code  | Text                    | Description
------|-------------------------|------------------------------------------------------------------------------------------------------------------------
`400` | `Bad Request`           | Your request may be malformed
`401` | `Unauthorized`          | Your API key is wrong, or your user does not have access to this resource
`403` | `Forbidden`             | The record requested is hidden for administrators only
`404` | `Not Found`             | The specified record could not be found
`405` | `Method Not Allowed`    | You tried to access a record with an invalid method
`406` | `Not Acceptable`        | You requested a format that isn't JSON
`410` | `Gone`                  | The record requested has been removed from our servers
`422` | `Unprocessable Entity`  | The requested method cannot process for the record in question
`429` | `Too Many Requests`     | You're requesting too many records! Slow down!
`500` | `Internal Server Error` | We had a problem with our server. Try again later.
`502` | `Bad Gateway`           | We had a different problem with our server. Try again later.
`503` | `Service Unavailable`   | We're temporarily offline for maintenance, or you've exceeded the [rate limit](#rate-limiting). Please try again later.
`504` | `Gateway Timeout`       | Yep, you guessed it. We'll be back soon!

<aside class="warning">
Note that when API errors occur, it is up to you to retry your request - Quaderno does not keep track of failed calls.<br /><br />
If an error occurs on a call where the service is not down, we will return a JSON response and an error code that we hope will point you to the problem with your call.
</aside>





# Versioning

> Example of API version override

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/contacts.json?since_id=2048 \
  -u YOUR_API_KEY:x \
  -H 'Accept: application/json; api_version=20160602'
```

When we make breaking changes to our API, we release a new version number which you can change in your calls.
By default if you don't specify a version, we'll use the one stored in your Quaderno account. To override this value you can pass the version in the `Accept` HTTP header like this: `Accept: application/json; api_version=API_VERSION_NUMBER`


<aside class="notice">
  The current version is <strong>20170914</strong>. Please refer to the <strong><a href='#changelog'>changelog</a></strong> to get a list of the changes.
</aside>
