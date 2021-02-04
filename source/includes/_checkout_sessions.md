# CHECKOUT 

## Sessions

A Checkout Session represents your customer's session as they pay for one-time purchases or subscriptions through Quaderno Checkout.

### Create a session

Sessions are created via checkout links, but you can also create them via API if you need to create sessions on the fly.

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/checkout/sessions \
  -u YOUR_API_KEY:x \
  -d billing_details_collection="auto" \
  -d cancel_url="https://domain.com" \
  -d coupon_collection=true \
  ...
```
> The above command returns JSON structured like this:

```json
{
      "id":1,
      "status":"completed",
      "billing_details_collection":"required",
      "cancel_url":"http://go.back.com",
      "coupon_collection":true,
      "locale":"auto",
      "payment_methods":["card", "paypal"],
      "success_url":"http://success.com?prod=1",
      "custom":{},
      "items":[
         {
            "product":"prod_61ffa845b4a0b8",
            "amount":999.0,
            "name":"A test item",
            "description":"This a test item.",
            "currency":"EUR",
            "quantity":1
         }
      ],
      "customer":{
         "billing_city":"John",
         "billing_country":"GB",
         "billing_postal_code":"asd",
         "billing_street_line_1":"asd",
         "billing_street_line_2":"asd",
         "company":"",
         "email":"john@doe.com",
         "first_name":"John",
         "last_name":"Doe",
         "tax_id":null
      },
      "metadata": {},
      "permalink":"https://demo.quadernoapp.com/checkout/session/8ccf3fdc42b85800188b113b81d3e4212ef094b3"
   }
```

#### HTTP Request

`POST /checkout/sessions`

#### Parameters

**Session parameters:**

Parameter                            | Type                  | Description
-------------------------------------|-----------------------|-----------------------------------------
`billing_details_collection`         | string                | The value for whether Checkout collected the customer’s billing details. Values are `auto` and `required` (default to `required`).
`cancel_url`                         | storing               | The URL the customer will be directed to if they decide to cancel payment and return to your website. **Required**
`coupon_collection`                  | boolean               | The value for whether Checkout collected coupons.
`custom`                             | object                | Set of key-value pairs that you want to forward to the payment processor. This can be useful for setting up additional options in the payment processor. If you want to send specific data to one processor, you can create a subhash with the name of that particular processor. E.g.: stripe: { metadata: { user_id: 999 } }, paypal: { no_shipping: 0 }
`customer`                           | object                | The customer's billing information. Check the attributes [here](#customer-attributes).
`items`                              | array                 | The list of products purchased by the customer. Check the attributes [here](#items-attributes). **Required**
`locale`                             | string                | The 2-letter ISO code of the language the Checkout is displayed in. Values are `auto`, `ca`, `de`, `en`, `es`, `fi`, `fr`, `hu`, `nl`, `sv`, and `no`.
`metadata`                           | object                | Set of key-value pairs that you can attach to the session. This can be useful for storing additional information about the purchase.
`payment_methods`                    | array                 | Can be `card` and `paypal`.
`success_url`                        | string                | The URL the customer will be directed to after the payment is successful. **Required**


**Customer parameters:**

Parameter                            | Type           | Description
-------------------------------------|----------------|-----------------------------------------
`billing_city`                       | string         | City/District/Suburb/Town/Village. Use this parameter to prefill customer data if you already have her city on file.
`billing_country`                    | string         | 2-letter country code. Use this parameter to prefill customer data if you already have her country on file.
`billing_postal_code`                | string         | ZIP or postal code. Use this parameter to prefill customer data if you already have her postal code on file.
`billing_street_line_1`              | string         | Address line 1 (Street address/PO Box). Use this parameter to prefill customer data if you already have her street address on file.
`billing_street_line_2`              | string         | Address line 2 (Apartment/Suite/Unit/Building). Use this parameter to prefill customer data if you already have her street address on file.
`contact`                            | integer        | The ID of the Quaderno Contact. A new contact will be created unless an existing contact was provided in when the Checkout was created.
`company`                            | string         | Customer company. Use this parameter to prefill customer data if you already have her company name on file.
`email`                              | string         | Customer email. Use this parameter to prefill customer data if you already have an email on file.
`first_name`                         | string         | Customer first name. Use this parameter to prefill customer data if you already have her first name on file.
`last_name`                          | string         | Customer last name. Use this parameter to prefill customer data if you already have her last name on file
`tax_id`                             | string         | Customer Tax ID. Use this parameter to prefill customer data if you already have a tax id on file.

**Items parameters:**

Parameter                            | Type                  | Description
-------------------------------------|-----------------------|-----------------------------------------
`amount`                             | number(decimal)       | Unit price of the item being purchased. Default value the product's price is used if amount is not set.
`currency`                           | string                | ISO 4217 currency code. Must be a supported currency by the payment processor. Default value the product's currency is used if currency is not set.
`description`                        | string                | The description of the item being purchased. Default value the product's description is used if description is not set.
`name`                               | string                | The name of the item being purchased. Default value the product's name is used if name is not set.
`product`                            | string                | The SKU of the Quaderno Product. **Required**
`quantity`                           | integer               | Quantity of the item being purchased. Minimum value is 1.






### Retrieve a session

Retrieves the details of an existing session. You need only supply the unique session identifier that was returned upon session creation.

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/checkout/sessions/SESSION_ID \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
{
      "id":1,
      "status":"completed",
      "billing_details_collection":"required",
      "cancel_url":"http://go.back.com",
      "coupon_collection":true,
      "locale":"auto",
      "payment_methods":["card", "paypal"],
      "success_url":"http://success.com?prod=1",
      "custom":{},
      "items":[
         {
            "product":"prod_61ffa845b4a0b8",
            "amount":999.0,
            "name":"A test item",
            "description":"This a test item.",
            "currency":"EUR",
            "quantity":1
         }
      ],
      "customer":{
         "billing_city":"John",
         "billing_country":"GB",
         "billing_postal_code":"asd",
         "billing_street_line_1":"asd",
         "billing_street_line_2":"asd",
         "company":"",
         "email":"john@doe.com",
         "first_name":"John",
         "last_name":"Doe",
         "tax_id":null
      },
      "metadata": {},
      "permalink":"https://demo.quadernoapp.com/checkout/session/8ccf3fdc42b85800188b113b81d3e4212ef094b3"
   }
```
#### HTTP Request

`GET /checkout/sessions/SESSION_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`SESSION_ID`            | integer   | The ID of the session to retrieve. **Required**





### Update a session

Updates the specified session by setting the values of the parameters passed. Any parameters not provided will be left unchanged. Only pending sessions can be updated.

This request accepts mostly the same arguments as the session creation call.

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/checkout/sessions/SESSION_ID \
  -u YOUR_API_KEY:x \
  -X PUT \
  -d billing_details_collection="auto" \
  -d cancel_url="https://domain.com" \
  -d coupon_collection=true \
```
> The above command returns JSON structured like this:

```json
{
      "id":1,
      "status":"completed",
      "billing_details_collection":"required",
      "cancel_url":"http://go.back.com",
      "coupon_collection":true,
      "locale":"auto",
      "payment_methods":["card", "paypal"],
      "success_url":"http://success.com?prod=1",
      "custom":{},
      "items":[
         {
            "product":"prod_61ffa845b4a0b8",
            "amount":999.0,
            "name":"A test item",
            "description":"This a test item.",
            "currency":"EUR",
            "quantity":1
         }
      ],
      "customer":{
         "billing_city":"John",
         "billing_country":"GB",
         "billing_postal_code":"asd",
         "billing_street_line_1":"asd",
         "billing_street_line_2":"asd",
         "company":"",
         "email":"john@doe.com",
         "first_name":"John",
         "last_name":"Doe",
         "tax_id":null
      },
      "metadata": {},
      "permalink":"https://demo.quadernoapp.com/checkout/session/8ccf3fdc42b85800188b113b81d3e4212ef094b3"
   }
```
#### HTTP Request

`PUT /checkout/sessions/SESSION_ID`

#### Parameters

**Session attributes:**

Parameter                            | Type                  | Description
-------------------------------------|-----------------------|-----------------------------------------
`SESSION_ID`                         | integer               | The ID of the session to update. **Required**
`billing_details_collection`         | string                | The value for whether Checkout collected the customer’s billing details. Values are `auto` and `required` (default to `required`).
`cancel_url`                         | storing               | The URL the customer will be directed to if they decide to cancel payment and return to your website. **Required**
`coupon_collection`                  | boolean               | The value for whether Checkout collected coupons.
`custom`                             | object                | Set of key-value pairs that you want to forward to the payment processor. This can be useful for setting up additional options in the payment processor. If you want to send specific data to one processor, you can create a subhash with the name of that particular processor. E.g.: stripe: { metadata: { user_id: 999 } }, paypal: { no_shipping: 0 }
`customer`                           | object                | The customer's billing information. Check the attributes [here](#customer-attributes).
`items`                              | array                 | The list of products purchased by the customer. Check the attributes [here](#items-attributes). **Required**
`locale`                             | string                | The 2-letter ISO code of the language the Checkout is displayed in. Values are `auto`, `ca`, `de`, `en`, `es`, `fi`, `fr`, `hu`, `nl`, `sv`, and `no`.
`metadata`                           | object                | Set of key-value pairs that you can attach to the session. This can be useful for storing additional information about the purchase.
`payment_methods`                    | array                 | Can be `card` and `paypal`.
`success_url`                        | string                | The URL the customer will be directed to after the payment is successful. **Required**


**Customer parameters:**

Parameter                            | Type           | Description
-------------------------------------|----------------|-----------------------------------------
`billing_city`                       | string         | City/District/Suburb/Town/Village. Use this parameter to prefill customer data if you already have her city on file.
`billing_country`                    | string         | 2-letter country code. Use this parameter to prefill customer data if you already have her country on file.
`billing_postal_code`                | string         | ZIP or postal code. Use this parameter to prefill customer data if you already have her postal code on file.
`billing_street_line_1`              | string         | Address line 1 (Street address/PO Box). Use this parameter to prefill customer data if you already have her street address on file.
`billing_street_line_2`              | string         | Address line 2 (Apartment/Suite/Unit/Building). Use this parameter to prefill customer data if you already have her street address on file.
`contact`                            | integer        | The ID of the Quaderno Contact. A new contact will be created unless an existing contact was provided in when the Checkout was created.
`company`                            | string         | Customer company. Use this parameter to prefill customer data if you already have her company name on file.
`email`                              | string         | Customer email. Use this parameter to prefill customer data if you already have an email on file.
`first_name`                         | string         | Customer first name. Use this parameter to prefill customer data if you already have her first name on file.
`last_name`                          | string         | Customer last name. Use this parameter to prefill customer data if you already have her last name on file
`tax_id`                             | string         | Customer Tax ID. Use this parameter to prefill customer data if you already have a tax id on file.

**Items parameters:**

Parameter                            | Type                  | Description
-------------------------------------|-----------------------|-----------------------------------------
`amount`                             | number(decimal)       | Unit price of the item being purchased. Default value the product's price is used if amount is not set.
`currency`                           | string                | ISO 4217 currency code. Must be a supported currency by the payment processor. Default value the product's currency is used if currency is not set.
`description`                        | string                | The description of the item being purchased. Default value the product's description is used if description is not set.
`name`                               | string                | The name of the item being purchased. Default value the product's name is used if name is not set.
`product`                            | string                | The SKU of the Quaderno Product. **Required**
`quantity`                           | integer               | Quantity of the item being purchased. Minimum value is 1.





### Delete a session

Permanently deletes a session. It cannot be undone. Only `pending` sessions can be deleted.

```shell
$ curl  https://quadernoapp.com/api/checkout/sessions/SESSION_ID \
  -u YOUR_API_KEY:x \
  -X DELETE 
```

#### HTTP Request

`DELETE /checkout/sessions/SESSION_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`SESSION_ID`            | integer   | The ID of the session to delete. **Required**





### List all sessions

You can list all sessions, or list the sessions for a specific status. The sessions are returned sorted by creation date, with the most recently created sessions appearing first.

Returns an array of sessions. Each entry in the array is a separate session object. If no more sessions are available, the resulting array will be empty.

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/checkout/sessions \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
[
   {
      "id":1,
      "status":"completed",
      "billing_details_collection":"required",
      "cancel_url":"http://go.back.com",
      "coupon_collection":true,
      "locale":"auto",
      "payment_methods":["card", "paypal"],
      "success_url":"http://success.com?prod=1",
      "custom":{},
      "items":[
         {
            "product":"prod_61ffa845b4a0b8",
            "amount":999.0,
            "name":"A test item",
            "description":"This a test item.",
            "currency":"EUR",
            "quantity":1
         }
      ],
      "customer":{
         "billing_city":"John",
         "billing_country":"GB",
         "billing_postal_code":"asd",
         "billing_street_line_1":"asd",
         "billing_street_line_2":"asd",
         "company":"",
         "email":"john@doe.com",
         "first_name":"John",
         "last_name":"Doe",
         "tax_id":null
      },
      "metadata": {},
      "permalink":"https://demo.quadernoapp.com/checkout/session/8ccf3fdc42b85800188b113b81d3e4212ef094b3"
   },
   {
      "id":2,
      "status":"failed",
      "billing_details_collection":"auto",
      "cancel_url":"http://go.back.com",
      "coupon_collection":true,
      "locale":"es",
      "payment_methods":["paypal"],
      "success_url":"http://success.com?prod=2",
      "custom":{},
      "items":[
         {
            "product":"awesome",
            "amount":999.0,
            "name":"Awesome",
            "description":"",
            "currency":"EUR",
            "quantity":1
         }
      ],
      "customer":{
         "billing_city":null,
         "billing_country":null,
         "billing_postal_code":"asd",
         "billing_street_line_1":"asdas",
         "billing_street_line_2":"asd",
         "company":"",
         "email":null,
         "first_name":"James",
         "last_name":null,
         "tax_id":null
      },
      "metadata": {},
      "permalink":"https://demo.quadernoapp.com/checkout/session/5c24890be57274275358e7d25ea5200384fc7293"
   },
   {
      "id":3,
      "status":"abandoned",
      "billing_details_collection":"required",
      "success_url":"http://success.com?prod=1",
      "coupon_collection":false,
      "locale":"auto",
      "payment_methods":["card", "paypal"],
      "success_url":"http://success.com",
      "custom":{},
      "items":[
         {
            "product":"prod_61ffa845b4a0b8",
            "amount":999.0,
            "name":"Something",
            "description":null,
            "currency":"EUR",
            "quantity":1
         }
      ],
      "customer":{
         "billing_city":"York",
         "billing_country":"GB",
         "billing_postal_code":"Y01 6FA",
         "billing_street_line_1":"Fake Street 1",
         "billing_street_line_2":"Apt. 1",
         "company":"",
         "email":"giles@gorales.co.uk",
         "first_name":"Giles",
         "last_name":null,
         "tax_id":null
      },
      "metadata": {},
      "permalink":"https://demo.quadernoapp.com/checkout/session/0092e2afbc457a2e3acf1a2c75928743d06c1fe5"
   }
]
```

#### HTTP Request

`GET /checkout/sessions`

#### Parameters

Parameter      | Type      | Description
---------------|-----------|----------------------------------------------------------------------------
`status`       | string    | A case-sensitive filter on the list based on the session's status. Can be `processing`, `abandoned`, `failed`, and `completed`.
