## Coupons

A coupon contains information about a percent-off or amount-off discount you might want to apply to a customer during a checkout.

### Create a coupon

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/checkout/coupons \
  -u YOUR_API_KEY:x \
  -d code="awesome" \
  -d name="Awesome discount" \
  -d percent_off=50 \
  -d processor_id="awesome01"
```
> The above command returns JSON structured like this:

```json
{
  "id": 8888,
  "code": "awesome",
  "name": "Awesome discount",
  "percent_off": 50,
  "times_redeemed": null,
  "processor_id": "awesome01",
  "amount_off_cents": null,
  "currency": null,
  "max_redemptions": null,
  "redeem_by": null
}
```

#### HTTP Request

`POST /checkout/coupons`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`code`                  | string    | The coupon's code. **Required**
`name`                  | string    | Name of the coupon displayed to customers on invoices.
`percent_off`           | decimal   | Percent that will be taken off the subtotal of the purchase. For example, a coupon with `percent_off` of 50 will make a $100 purchase $50 instead. Required if `amount_off` is not passed.
`amount_off`            | decimal   | Amount (in the `currency` specified) that will be taken off the subtotal of the purchase. Required if `percent_off` is not passed.
`currency`              | string    | If `amount_off` has been set, the three-letter [ISO currency code](https://en.wikipedia.org/wiki/ISO_4217) of the amount to take off, in uppercase. Must be a supported currency in your payment processors.
`redeem_by`             | date      | Date after which the coupon can no longer be redeemed.
`max_redemptions`       | integer   | Maximum number of times this coupon can be redeemed, in total, across all customers, before it is no longer valid.








### Retrieve a coupon

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/checkout/coupons/COUPON_ID \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
{
  "id": 8888,
  "code": "awesome",
  "name": "Awesome discount",
  "percent_off": 50,
  "times_redeemed": null,
  "processor_id": "awesome01",
  "amount_off_cents": null,
  "currency": null,
  "max_redemptions": null,
  "redeem_by": null
}
```

#### HTTP Request

`GET /checkout/coupons/COUPON_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`COUPON_ID`             | integer   | The ID of the coupon to retrieve. **Required**




### Update a coupon

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/checkout/coupons/COUPON_ID \
  -u YOUR_API_KEY:x \
  -X PUT \
  -d percent_off=25
```
> The above command returns JSON structured like this:

```json
{
  "id": 8888,
  "code": "awesome",
  "name": "Awesome discount",
  "percent_off": 25,
  "times_redeemed": null,
  "processor_id": "awesome01",
  "amount_off_cents": null,
  "currency": null,
  "max_redemptions": null,
  "redeem_by": null
}
```

#### HTTP Request

`PUT /checkout/coupons/COUPON_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`COUPON_ID`             | integer   | The ID of the coupon to update. **Required**
`code`                  | string    | The coupon's code. **Required**
`name`                  | string    | Name of the coupon displayed to customers on invoices.
`percent_off`           | decimal   | Percent that will be taken off the subtotal of the purchase. For example, a coupon with `percent_off` of 50 will make a $100 purchase $50 instead. Required if `amount_off` is not passed.
`amount_off`            | decimal   | Amount (in the `currency` specified) that will be taken off the subtotal of the purchase. Required if `percent_off` is not passed.
`currency`              | string    | If `amount_off` has been set, the three-letter [ISO currency code](https://en.wikipedia.org/wiki/ISO_4217) of the amount to take off, in uppercase. Must be a supported currency in your payment processors.
`redeem_by`             | date      | Date after which the coupon can no longer be redeemed.
`max_redemptions`       | integer   | Maximum number of times this coupon can be redeemed, in total, across all customers, before it is no longer valid.




### Delete a coupon

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/checkout/coupons/COUPON_ID \
  -u YOUR_API_KEY:x \
  -X DELETE
```

#### HTTP Request

`DELETE /checkout/coupons/COUPON_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`COUPON_ID`             | integer   | The ID of the coupon to delete. **Required**





### List all coupons

> `GET /checkout/coupons.json`

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/checkout/coupons \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 8888,
    "code": "awesome",
    "name": "Awesome discount",
    "percent_off": 50,
    "times_redeemed": null,
    "processor_id": "awesome01",
    "amount_off_cents": null,
    "currency": null,
    "max_redemptions": null,
    "redeem_by": null
  },
  {
    "id": 9999,
    "code": "20off",
    "name": "20% off for 3 months",
    "percent_off": 20,
    "times_redeemed": null,
    "processor_id": "20off",
    "amount_off_cents": null,
    "currency": null,
    "max_redemptions": null,
    "redeem_by": null
  }]
```

`GET`ting from `/coupons.json` will return all the account's coupons.

