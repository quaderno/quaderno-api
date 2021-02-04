# TAXES 

## Tax IDs

Quaderno calculates taxes based on your tax settings. You need to set all your tax IDs from the jurisdictions where you're registered for tax collection in order to calculate taxes for that jurisdictions. 

This API will allow you to manage your tax IDs and validate any tax IDs.

### Create a tax ID

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_ids \
  -u YOUR_API_KEY:x \
  -d jurisdiction_id=6 \
  -d value="IE6388047V"
```

> The above command returns JSON structured like this:

```json
{
  "id": 456987213,
  "jurisdiction": {
    "id": 6,
    "name": "Ireland"
  },
  "state": "verified",
  "valid_from": "2015-01-01",
  "valid_until": null,
  "value": "IE6388047V",
  "created_at": 1611322639
}
```

#### HTTP Request

`POST /tax_ids`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`jurisdiction_id`       | integer   | The [tax jurisdiction](#tax-jurisdictions). **Required**
`value`                 | decimal   | The tax ID's value for the selected. **Required**
`valid_from`            | date      | The tax ID's validity date. Defaults to the beginning of the year.





### Retrieve a tax ID

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_ids/TAX_ID \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
{
  "id": 456987213,
  "jurisdiction": {
    "id": 6,
    "name": "Ireland"
  },
  "state": "verified",
  "valid_from": "2015-01-01",
  "valid_until": null,
  "value": "IE6388047V",
  "created_at": 1611322639
}
```

#### HTTP Request

`GET /tax_ids/TAX_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`TAX_ID`                | integer   | The ID of the tax ID to retrieve. **Required**





### Update a tax ID

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_ids/TAX_ID º
  -u YOUR_API_KEY:x \
  -X PUT \
  -d jurisdiction_id=73 \
  -d value="LU21416127"
```

> The above command returns JSON structured like this:

```json
{
  "id": 456987213,
  "jurisdiction": {
    "id": 73,
    "name": "Luxembourg"
  },
  "state": "verified",
  "valid_from": "2015-01-01",
  "valid_until": null,
  "value": "LU21416127",
  "created_at": 1611322639
}
```
#### HTTP Request

`PUT /tax_ids/TAX_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`TAX_ID`                | integer   | The ID of the tax ID to update. **Required**
`value`                 | decimal   | The tax ID's value for the selected. **Required**
`valid_from`            | date      | The tax ID's validity date.
`valid_until`           | date      | The tax ID's expiration date. Use null if the tax ID has no expiration date.





### Delete a tax ID

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_ids/TAX_ID \
  -u YOUR_API_KEY:x \
  -X DELETE
```

#### HTTP Request

`DELETE /tax_ids/TAX_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`TAX_ID`                | integer   | The ID of the tax ID to delete. **Required**





### List all tax IDs

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_ids \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 456987213,
    "jurisdiction": {
      "id": 6,
      "name": "Ireland"
    },
    "state": "verified",
    "valid_from": "2015-01-01",
    "valid_until": null,
    "value": "IE6388047V",
    "created_at": 1611322639
  },
  {
    "id": 456982365,
    "jurisdiction": {
      "id": 73,
      "name": "Luxembourg"
    },
    "state": "verified",
    "valid_from": "2020-01-01",
    "valid_until": null,
    "value": "LU21416127",
    "created_at": 1611322639
  }
]
```

#### HTTP Request

`GET /tax_ids`






### Validate a tax ID

<aside class="notice">
Quaderno can validate tax IDs from the EU, United Kingdom, Switzerland, Québec (Canada), Australia, and New Zealand.
</aside>

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_ids/validate.json?country=IE&tax_id=IE6388047V \
  -u YOUR_API_KEY:x \
```

> The above command returns JSON structured like this:

```json
{
  "valid":true
}
```

#### HTTP Request

`GET /tax_ids/validate`

#### Parameters

Parameter    | Mandatory | Description
-------------|-----------|------------------------------------------------------------------------------------------------
`country`    | **Yes**   | The customer's country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`tax_id`     | **Yes**   | The customer's tax ID. 


The result can be true (valid tax ID), false (invalid tax ID) or null (the external service is temporarily unavailable).

Please keep in mind that tax IDs are not actually validated while using the [Sandbox](https://developers.quaderno.io/#introduction-the-sandbox). For testing purposes you can use the following IDs: 

- **DE111111111**: invalid number, returns `false`
- **IE222222222**: node down, returns `null` 
