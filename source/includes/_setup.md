## Authentication

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






## Authorization

```shell
curl https://quadernoapp.com/api/authorization \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
{
  "identity": {
    "id": "999",
    "name": "Sheldon Cooper",
    "email": "s.cooperphd@yahoo.com",
    "publishable_key":"pk_1111111111111111",
    "href": "http://nippur-999.quadernoapp.com/api/"
  }
}
```

If you don't know the `ACCOUNT_NAME` for your target account, you can get it with the `authorization` API call.

This returns the following as a JSON payload:

Parameter     | Description
--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
`id`              | An identity, which is *not* used for determining who this user is within Quaderno. The `id` field should therefore *not* be used for submitting data within Quaderno's API.
`name`            | The user's full name.
`email`           | The user's email address.
`publishable_key` | The users's publishable key.
`href`            | The custom API endpoint URL for the user, providing the sought-after `ACCOUNT_NAME` between `http://` and `.quadernoapp...`.


<aside class="notice">
  Note that in all cases, if the user does not have permission to do something, you'll get a 401 Unauthorized error.
</aside>








## Rate Limiting

To make it easier to determine if your application is being rate-limited, or is approaching that level, we have the following HTTP headers on our successful responses:

- `X-RateLimit-Reset`
- `X-RateLimit-Remaining`

There is a limit of **100 API calls per 15 seconds**.

We reserve the right to tune the limitations, but we promise to keep them high enough to allow a well-behaving interactive app to do it's job.

If you exceed the limit you will receive a `HTTP 429 (Too Many Requests)`.





## Pagination

### Cursor based pagination

> A call with the `created_before` parameter set:

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/contacts.json?created_before=42 \
  -u YOUR_API_KEY:x
```

As of API version `20210316`, pagination is performed with a `created_before` parameter. This parameter takes an existing object ID value and returns objects listed after the named object, in reverse chronological order.

The HTTP header `X-Pages-HasMore` indicates whether more records can be fetched by using the same query with a lower `created_before`.

The HTTP header `X-Pages-NextPage` contains the URL that should be used to fetch the next page of records. It is only present if more records exist.

Bear in mind that Quaderno paginates `GET` index results.

You can change the number of objects to be returned with the `limit` parameter, defaulting to `25`. This value is capped at 100 objects.

### Page parameter (legacy)

> **Legacy**: A call with the `page` parameter set:

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/contacts.json?page=2 \
  -u YOUR_API_KEY:x
```

<aside class="warning">
The following use case is legacy and will be discontinued on 1st July 2021. You can keep using it requesting to <a href="#versioning">use the API version `20170914`</a>. Please update your code to use the new <a href="#cursor-based-pagination">cursor based pagination</a>.
</aside>

These HTTP headers inform you about the page context:

- `X-Pages-CurrentPage`
- `X-Pages-TotalPages`

You can change the page by passing the `page` parameter, defaulting to `1`.

You can change the number of objects to be returned with the `limit` parameter, defaulting to `25`. This value is capped at 100 objects.







## Errors

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





## Versioning

> Example of API version override

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/contacts.json?created_before=2048 \
  -u YOUR_API_KEY:x \
  -H 'Accept: application/json; api_version=20160602'
```

When we add functionality to our API, we release a new version number which you can change in your calls.
By default if you don't specify a version, we'll use the one stored in your Quaderno account, under Profile → API Keys.

To override this value you can pass the version in the `Accept` HTTP header like this: `Accept: application/json; api_version=API_VERSION_NUMBER`

Exceptionally, we release versions that break backwards compatibility, to take full advantage of new features or to make the API faster for you. In these rare cases, we give a transition period in which you will be able to select which API version to use. After the deprecation date, the old API versions will not be available. These are the cases for versions <strong>20210316</strong> and <strong>20210701</strong>.


<aside class="info">
  The current version is <strong>20210316</strong>, which contains breaking changes to pagination and deprecates the legacy Taxes API. It's currently into transitioning time and backwards compatibility will be disabled on 1 July.

  Also on 1 July version <strong>20210701</strong> will be released, which contains breaking changes to the Invoice API. Please refer to the following guides on safely upgrading API versions </a> and the <a href='#changelog'>changelog</a> for details.
</aside>

### Safely upgrading to API version 20210701

In our goal to make Quaderno compliant with tax rules worldwide, we're going to block the edition of invoices and credit notes via API as of 1 July.

From that date, an invoice will be made final (not editable) when any of the following occur:

- The invoice is sent to the customer. All sent invoices are final and cannot be edited.
- The invoice was generated by an integration. All automated invoices are final and cannot be edited.
- The invoice is manually made final in the invoice option menu. This option blocks the invoice and prevents future modifications.

If you need to make any changes to invoice items, date or customer's tax ID, you'll need to issue a credit note and create a new invoice.

You will still be allowed to edit the customer's street address, custom metadata, and add tags and additional notes.

What this breaking change implies:

- `DELETE /invoices/INVOICE_ID` will cease to exist and return a `410 Gone`.
- `PUT /invoices/INVOICE_ID` will only allow modification of the parameters `notes`, `tag_list`, `custom_metadata` and all those related to the customer's address: `street_line_1`,`street_line_2`,`city`,`region`,`postal_code` and `country`.

Note that on 1 July the transition period for *20210316* ends as well, and the legacy endpoints for [taxes](#taxes-legacy) and [pagination](#page-parameter-legacy) will also respond `410 Gone`.

If you're using these endpoints, you'll have to include extra logic in your code to issue a credit note and create a new invoice.

However, given your use case is one that automatically issues paid invoices (for instance, your customer's pays and then your system issues the invoice, not the other way around) and you're up for a challenge, our recommendation to face this change is to migrate to the new [Transactions API](#transactions).

Why? Because you can create the contact (if doesn't exists yet), create the invoice, and insert the payment information, all in only one performant API call 🎉!

### Safely upgrading to API version 20210316

Most changes simply add functionality, thus maintaining backwards compatibility. Occasionally, we release a version with breaking changes, such as the current 20210316.

<aside class="info">
We are providing a transition time in which you will be able to use both 20170914 and 20210316. After <strong>July 1st</strong> the legacy endpoints for <a href='#taxes-legacy'>taxes</a> and <a href='#page-parameter-legacy'>pagination</a> will respond a <code>410 Gone</code>.
<br><br>
Note that <a href="https://developers.quaderno.io/#quaderno-js">Quaderno.js</a> is not affected by this change. If you're only using Quaderno.js to calculate taxes from your frontend, you don't have to do anything.
</aside>

> Example of the new Tax Rates endpoint

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_rates/calculate?to_country=US&to_postal_code=90210&tax_code=standard&amount=10 \
  -u YOUR_API_KEY:x
```

> Example of using the deprecated Taxes API endpoint after upgrading the API Version globally on your Quaderno Account

```shell
curl "https://ACCOUNT_NAME.quadernoapp.com/api/taxes/calculate?country=US&postal_code=94010" \
  -u YOUR_API_KEY: \
  -H 'Accept: application/json; api_version=20170914'
```

> Note you'll get a 0.0 `rate` along with this `notice` in the response for **both cases**, unless you are registered on the tax jurisdiction:

```json
"notice": "You haven't registered this tax jurisdiction in your Quaderno account. If you have to collect taxes here, please set it up in https://ACCOUNT_NAME.quadernoapp.com/settings/jurisdictions ."
```

Do not fret, the transition to 20210316 is as easy as pie 🥧.

For **calculating tax rates**, altough the [new endpoint](#calculate-a-tax-rate) accepts many more parameters, just changing the URL from `/taxes/calculate` to `/tax_rates/calculate` will do. Well, not so fast. It may seem it worked quickly, but you're probably getting a 0% tax rate. That's because now you can only collect taxes on Tax Jurisdictions where you're registered. So don't be tempted to [create a new custom Tax Rate](#create-a-tax-rate), just head to your Quaderno Account and update the [tax jurisdictions](https://quadernoapp.com/settings/jurisdictions) where your business is registered in. Now we're talking! Quaderno will always provide updated worldwide tax rates seamlessly for you.

Alternatively, you can **register jurisdictions programatically**, first [listing all jurisdictions](#list-all-jurisdictions) and then [registering your Tax IDs](#create-a-tax-id) on the jurisdictions. You can read more about Tax Jurisdictions [here](https://support.quaderno.io/tax-jurisdictions/).

For **paginating objects**, we switched to a faster cursor based pagination approach and you'll need to change the `page` parameter for `created_before`, which references the ID of the object instead of the page number. You'll get a maximum of 100 objects in reverse chronological order (lower IDs means older objects). Note the response headers changed from `X-Pages-CurrentPage` and `X-Pages-TotalPages` to `X-Pages-HasMore` and `X-Pages-NextPage`.


You can test all this changes in our [sandbox](#the-sandbox). There are two approaches:

- Simply change the API version globally in Profile → API Keys. Note once you change it you cannot change it back, but you can override a concrete call sending the `Accept` HTTP header with: `Accept: application/json; api_version=20170914`
- Sending the `Accept` HTTP header with: `Accept: application/json; api_version=20210316` on every call.

And that's basically it 🎉! Now you can go focus on your business while we deal with your taxes.
