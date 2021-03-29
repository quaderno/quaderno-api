## Taxes (legacy)

<aside class="warning">
The following endpoints are legacy and will be discontinued on 1st July 2021. Please use the new endpoints for <a href="#calculate-a-tax-rate">tax calculation</a> and <a href="#validate-a-tax-id">tax ID validation</a>.
</aside>

One of the killer features Quaderno provides is efficient and easy tax management. As part of this, we offer an API for easy tax calculation.

### Calculate Taxes

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/taxes/calculate?country=US&postal_code=94010 \
  -u YOUR_API_KEY:x \
```
> The above command returns JSON structured like this:

```json
{
    "name": "Sales tax",
    "rate": 9.5,
    "extra_name":null,
    "extra_rate":null,
    "country":"US",
    "region":"CA",
    "transaction_type":"eservice",
    "notes":null
}
```

#### HTTP Request

`GET /taxes/calculate`

#### Parameters

Parameter          | Type      | Description
-------------------|-----------|------------------------------------------------------------------------------------------------
`country`          | string    | Customer's country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes)). **Required**
`postal_code`      | string    | Customer's postal code / zip
`city`             | string    | Customer's city (for US sales tax)
`tax_id`           | string    | Customer's Tax ID. Quaderno can validate tax IDs from the EU, United Kingdom, Switzerland, Québec (Canada), Australia, and New Zealand.
`transaction_type` | string    | Can be `eservice`, `ebook`, `standard`, `reduced`, `saas`. Defaults to the account's `default_tax_class`.






### Validating Tax IDs

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/taxes/validate?country=IE&tax_id=IE6388047V \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
{
    "valid":true
}
```

#### HTTP Request

`GET /taxes/validate`

#### Parameters

Parameter    | Type      | Description
-------------|-----------|------------------------------------------------------------------------------------------------
`country`    | string    | Customer's country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes)). **Required**
`tax_id`     | stirng    | Customer's tax ID. Quaderno can validate tax IDs from the EU, United Kingdom, Switzerland, Québec (Canada), Australia, and New Zealand. **Required**

Please keep in mind that tax IDs are not actually validated while using the [Sandbox](https://developers.quaderno.io/#introduction-the-sandbox). For testing purposes you can use the following IDs:

- **DE111111111**: invalid number, returns `false`
- **IE222222222**: node down, returns `null`
