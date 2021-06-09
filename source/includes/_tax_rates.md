## Tax Rates

This API will allow you to customize your tax rates and calculate tax rates for a particular transaction. 

Take into account that taxes are calculated based on your [tax IDs](#tax_ids), but you can override these tax rates for greater control over the taxes you charge. If that's the case, use this API to create your custom tax rates.

### Create a tax rate

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_rates \
  -u YOUR_API_KEY:x \
  -d jurisdiction_id=1 \
  -d name="VAT" \
  -d value=20
```
> The above command returns JSON structured like this:

```json
{
  "id": 456987213,
  "country": "FR",
  "name": "TVA",
  "tax_class": "standard",
  "valid_from": "2020-01-01",
  "valid_until": null,
  "value": 20.0,
  "created_at": 1611330350
}
```

#### HTTP Request

`POST /tax_rates`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`jurisdiction_id`       | integer   | The [tax jurisdiction](#tax-jurisdictions). **Required**
`name`                  | string    | The tax rate's name. **Required**
`value`                 | decimal   | The tax rate's value for the selected `tax_code`. **Required**
`tax_code`              | string    | The [tax code](#tax-codes). Defaults to the account's default tax code. 
`valid_from`            | date      | The tax rate's validity date. Defaults to the beginning of 3 years ago.





### Retrieve a tax rate

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_rates/TAX_RATE_ID \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
{
  "id": 456987213,
  "country": "FR",
  "name": "TVA",
  "tax_class": "standard",
  "valid_from": "2020-01-01",
  "valid_until": null,
  "value": 20.0,
  "created_at": 1611330350
}
```

#### HTTP Request

`GET /tax_rates/TAX_RATE_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`TAX_RATE_ID`           | integer   | The ID of the tax rate to retrieve. **Required**




### Update a tax rate

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_rates/TAX_RATE_ID \
  -u YOUR_API_KEY:x \
  -X PUT \
  -d value=25
```
> The above command returns JSON structured like this:

```json
{
  "id": 456987213,
  "country": "FR",
  "name": "TVA",
  "tax_class": "standard",
  "valid_from": "2020-01-01",
  "valid_until": null,
  "value": 25.0,
  "created_at": 1611330350
}
```

#### HTTP Request

`PUT /tax_rates/TAX_RATE_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`TAX_RATE_ID`           | integer   | The ID of the tax rate to update. **Required**
`jurisdiction_id`       | integer   | The [tax jurisdiction](#tax-jurisdictions). **Required**
`name`                  | string    | The tax rate's name. **Required**
`value`                 | decimal   | The tax rate's value for the selected `tax_code`. **Required**
`tax_code`              | string    | The [tax code](#tax-codes). Defaults to the account's default tax code. 
`valid_from`            | date      | The tax rate's validity date. Defaults to 3 years ago.
`valid_until`           | date      | The tax rate's expiration date. Use null if the tax rate has no expiration date.




### Delete a tax rate

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_rates/TAX_RATE_ID \
  -u YOUR_API_KEY:x \
  -X DELETE
```

#### HTTP Request

`DELETE /tax_rates/TAX_RATE_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`TAX_RATE_ID`           | integer   | The ID of the tax rate to delete. **Required**





### List all tax rates

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_rates \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 88,
    "type": "default",
    "jurisdiction": {
      "id": 50,
      "country": "US",
      "name": "United States – Texas",
      "region": "TX"
    },
    "name": "Sales tax",
    "tax_code": "eservice",
    "valid_from": "2016-01-01",
    "valid_until": null,
    "value": 6.25,
    "created_at": 1622640370
  },
  ...
]
```

#### HTTP Request
`GET /tax_rates`

This will return all the account's tax rates.

#### Filtering by tax jurisdiction

Filtering is available using the query parameter `jurisdiction_id`. Learn more about jurisdictions [here](https://developers.quaderno.io/api/#tax-jurisdictions).

> Filtering by tax jurisdiction:

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_rates/?jurisdiction_id=50 \
  -u YOUR_API_KEY:x
```

## Calculating a tax rate

<aside class="notice">
Tax calculations are based on your tax settings. You can collect taxes only in tax jurisdictions where you're registered for tax collection. Please check your <a href="https://quadernoapp.com/settings/jurisdictions" target="_blank">tax jurisdictions</a> in your Quaderno account. If you try to calculate taxes in a jurisdiction you're not registered, Quaderno will return a 0% tax rate. 
</aside>

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_rates/calculate?to_country=US&to_postal_code=90210&tax_code=standard&amount=10 \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
{
  "city": "BEVERLY HILLS",
  "country": "US",
  "county": "LOS ANGELES",
  "currency": "EUR",
  "name": "Sales tax",
  "notes": null,
  "rate": 9.5,
  "region": "CA",
  "tax_behavior": "exclusive",
  "tax_code": "standard",
  "taxable_part": 100.0,
  "taxable_base": 10.0,
  "tax_amount": 0.95,
  "total_amount": 10.95
}
```

The new tax calculation endpoint allows you to send the transaction's amount before taxes. Quaderno will use that amount to calculate the final tax-included amount you should charge to your customer. This is completely optional.

> To understand the endpoint response, the formulae are:

```
taxable_base = amount * taxable_part
tax_amount   = taxable_base * rate/100
total_amount = amount + tax_amount
```

> Taxable base is the amount you must use to calculate the tax amount. It's usually 100% (taxable_part) of the amount of your product before taxes but there are a few exceptions. For example, in Texas the taxable base is 80% for SaaS products.

> Note that an `extra_rate` field may appear in the response, for those jurisdictions that need to apply two tax rates (e.g. some Canadian provinces).

#### HTTP Request

`GET /tax_rates/calculate`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`to_country`            | string    | The customer's country. 2-letter [ISO country code](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes). **Required**
`to_postal_code`        | string    | The customer's postal code. Required whether the customer is based in the US or Canada.
`to_city`               | string    | The customer's city. It provides more accurate calculations in the US.
`tax_id`                | string    | The customer's tax ID.
`from_country`          | string    | The country where the order shipped from. 2-letter [ISO country code](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes). Defaults to the account's country.
`from_postal_code`      | string    | The postal code where the order shipped from. Defaults to the account's postal code.
`amount`                | decimal   | The transaction's amount. 
`tax_code`              | string    | The transaction's [tax code](#tax-codes). Defaults to the account's default tax code. 
`tax_behavior`          | string    | Specifies whether the price is considered inclusive of taxes or exclusive of taxes. Can be `inclusive` or `exclusive`. Defaults to `exclusive`. 
`product_type`          | string    | Specifies whether the product is a good or a service. Can be `good` or `service`. Defaults to `service`. 
`date`                  | string    | The transaction's date. Defaults to today.
`currency`              | string    | The transaction's currency. Three-letter [ISO currency code](https://en.wikipedia.org/wiki/ISO_4217), in uppercase. Defauts to the account's default currency.
