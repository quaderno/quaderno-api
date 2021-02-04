## Products

Products are all those goods or services that you sell to your customers.

Products are their own object in Quaderno, but they can be referenced, in an array, by invoices, expenses, and estimates.

### Create a product

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/items \
  -u YOUR_API_KEY:x \
  -d code="9999" \
  -d name="Titanic II: The revenge" \
  -d unit_cost=99.0
  -d tax_class="eservice"
```

> The above command returns JSON structured like this:

```json
{
  "id": 186698,
  "code": "9999",
  "description": null,
  "name": "Titanic II: The revenge",
  "product_type": "service",
  "unit_cost": "99.0",
  "tax_class": "eservice",
  "kind": "one_off",
  "tax_type": "excluded",
  "tax_based_on": "customer_country",
}
```

#### HTTP Request

`POST /items`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`code`                  | string    | The product’s SKU (Stock Keeping Unit) describe specific product variations, taking into account any combination of: attributes, currency, and cost. **Required**
`name`                  | string    | The product’s name, meant to be displayable to the customer. **Required**
`unit_cost`             | string    | The unit amount in paise to be charged
`description`           | string    | The product’s description, meant to be displayable to the customer. Use this field to optionally store a long form explanation of the product being sold for your own rendering purposes.
`curency`               | string    | Three-letter [ISO currency code](https://en.wikipedia.org/wiki/ISO_4217), in uppercase. Must be a supported currency in your payment processors.
`tax_class`             | string    | The tax class that applies to the product.
`tax_type`              | string    | Specify if taxes are included or excluded in the `unit_cost`. Can be `included`, `excluded`. Defaults to `excluded`. 
`kind`                  | string    | The type of transaction. Can be `one_off` or `subscription`. Defaults to `one_off`
`stripe_id`        | string    | Only for Stripe `subscriptions`. ID of the Stripe price related to the subscription.
`interval_unit`  | string    | Only for PayPal `subscriptions`. Specifies billing frequency. Can be `daily`, `weekly`, `monthly`, `yearly`.
`interval_frequency` | integer    | Only for PayPal `subscriptions`. The number of intervals between subscription billings.
`interval_duration`  | integer    | Only for `subscriptions`. Number of times the charge should recur.

### Retrieve a product

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/items/PRODUCT_ID \ 
  -u YOUR_API_KEY:x 
```

> The above command returns JSON structured like this:

```json
{
  "id": 186698,
  "code": "9999",
  "description": null,
  "name": "Titanic II: The revenge",
  "product_type": "service",
  "unit_cost": "99.0",
  "tax_class": "eservice",
  "kind": "one_off",
  "tax_type": "excluded",
  "tax_based_on": "customer_country",
}
```

#### HTTP Request

`GET /items/PRODUCT_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`PRODUCT_ID`            | integer   | The ID of the product to retrieve. **Required**

### Update a product

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/items/PRODUCT_ID \
  -u YOUR_API_KEY:x \
  -X PUT \
  -d unit_cost=10.0
```

> The above command returns JSON structured like this:

```json
{
  "id": 186698,
  "code": "9999",
  "description": null,
  "name": "Titanic II: The revenge",
  "product_type": "service",
  "unit_cost": "99.0",
  "tax_class": "eservice",
  "kind": "one_off",
  "tax_type": "excluded",
  "tax_based_on": "customer_country",
}
```

#### HTTP Request

`PUT /items/PRODUCT_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`PRODUCT_ID`            | integer   | The ID of the product to update. **Required**
`code`                  | string    | The product’s SKU (Stock Keeping Unit) describe specific product variations, taking into account any combination of: attributes, currency, and cost. **Required**
`name`                  | string    | The product’s name, meant to be displayable to the customer. **Required**
`unit_cost`             | string    | The unit amount in paise to be charged
`description`           | string    | The product’s description, meant to be displayable to the customer. Use this field to optionally store a long form explanation of the product being sold for your own rendering purposes.
`curency`               | string    | Three-letter [ISO currency code](https://en.wikipedia.org/wiki/ISO_4217), in uppercase. Must be a supported currency in your payment processors.
`tax_class`             | string    | The tax class that applies to the product.
`tax_type`              | string    | Specify if taxes are included or excluded in the `unit_cost`. Can be `included`, `excluded`. Defaults to `excluded`. 
`kind`                  | string    | The type of transaction. Can be `one_off` or `subscription`. Defaults to `one_off`
`stripe_id`        | string    | Only for Stripe `subscriptions`. ID of the Stripe price related to the subscription.
`interval_unit`  | string    | Only for PayPal `subscriptions`. Specifies billing frequency. Can be `daily`, `weekly`, `monthly`, `yearly`.
`interval_frequency` | integer    | Only for PayPal `subscriptions`. The number of intervals between subscription billings.
`interval_duration`  | integer    | Only for `subscriptions`. Number of times the charge should recur.

### Delete a product

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/items/PRODUCT_ID \
  -u YOUR_API_KEY:x \
  -X DELETE
```

#### HTTP Request

`DELETE /items/PRODUCT_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`PRODUCT_ID`            | integer   | The ID of the product to delete. **Required**


### List all products

> `GET /items.json`

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/items \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 186695,
    "code": "8888",
    "description": null,
    "name": "Titanic I",
    "product_type": "service",
    "unit_cost": "79.0",
    "tax_class": "eservice",
    "kind": "one_off",
    "tax_type": "excluded",
    "tax_based_on": "customer_country",
  },
  {
    "id": 186698,
    "code": "9999",
    "description": null,
    "name": "Titanic II: The revenge",
    "product_type": "service",
    "unit_cost": "99.0",
    "tax_class": "eservice",
    "kind": "one_off",
    "tax_type": "excluded",
    "tax_based_on": "customer_country",
  }
 ]
```

#### HTTP Request

`GET /items`

#### Parameters

Parameter      | Type      | Description
---------------|-----------|----------------------------------------------------------------------------
`q`            | string    | A case-sensitive filter on the list based on the product's code or name.

