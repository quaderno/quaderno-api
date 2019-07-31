# Checkout

## Sessions

### List all sessions

You can list all sessions, or list the sessions for a specific status. The sessions are returned sorted by creation date, with the most recently created sessions appearing first.

> `GET /checkout/sessions.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/checkout/sessions.json'
```

```php?start_inline=1
// Coming soon!
```

```ruby
# Coming soon!
```

```swift?start_inline=1
// Coming soon!
```

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
         "tax_id":null,
         "business_number":null
      },
      "permalink":"http://quaderno.lvh.me:3000/checkout/session/8ccf3fdc42b85800188b113b81d3e4212ef094b3"
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
      "permalink":"http://quaderno.lvh.me:3000/checkout/session/5c24890be57274275358e7d25ea5200384fc7293"
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
      "permalink":"http://quaderno.lvh.me:3000/checkout/session/0092e2afbc457a2e3acf1a2c75928743d06c1fe5"
   }
]

```


key                                  | type           | description
-------------------------------------|----------------|-----------------------------------------
`id`                                 | integer        | Unique identifier for the object.
`billing_details_collection`         | string         | The value for whether Checkout collected the customer’s billing details. Values are `auto` and `required` (default to `required`).
`cancel_url`                         | string         | The URL the customer will be directed to if they decide to cancel payment and return to your website.
`coupon_collection`                  | boolean        | The value for whether Checkout collected coupons.
`custom`                             | object         |
`customer`                           | object         |
`items`                              | array          | The list of products purchased by the customer.
`locale`                             | string         |The 2-letter ISO code of the language the Checkout is displayed in. Values are `auto`, `ca`, `de`, `en`, `es`, `fi`, `fr`, `hu`, `nl`, `sv`, and `no`.
`payment_methods`                    | array          | Values are card and paypal.
`permalink`                          | string         | The URL of this Checkout Link.
`success_url`                        | string         | The URL the customer will be directed to after the payment is successful.
`processor`                          | string         | Values are `stripe` and `paypal`.
`processor_id`                       | string         |
`status`                             | string         | `pending` sessions are unpaid and awaiting payment. A session is marked as failed when the payment failed or was declined. Note that this status many not show inmediately and instead show as `pending` until verified. completed sessions requires no further action and cannot be edited. A session is marked as abandoned after being cancelled by the customer or 30 minutes without activitiy. Values are `pending`, `processing`, `failed`, `completed`, and `abandoned`.

## Create a session

Sessions are created via links, but you can also create them via API if you need to create sessions on the fly.

> `POST /checkout/sessions.json`

This will return `201 Created` and the current JSON representation of the session if the creation was a success.

key                                  | type           | description
-------------------------------------|----------------|-----------------------------------------
`billing_details_collection`         | string         | The value for whether Checkout collected the customer’s billing details. Values are `auto` and `required` (default to `required`).
`cancel_url`                         | string         | The URL the customer will be directed to if they decide to cancel payment and return to your website.
`coupon_collection`                  | boolean        | The value for whether Checkout collected coupons.
`custom`                             | object         |
`customer`                           | object         |
`items`                              | array          | The list of products purchased by the customer.
`locale`                             | string         |The 2-letter ISO code of the language the Checkout is displayed in. Values are `auto`, `ca`, `de`, `en`, `es`, `fi`, `fr`, `hu`, `nl`, `sv`, and `no`.
`payment_methods`                    | array          | Values are card and paypal.
`success_url`                        | string         | The URL the customer will be directed to after the payment is successful.


## Retrieve a session

Retrieves the details of an existing session. You need only supply the unique session identifier that was returned upon session creation.

> `GET /checkout/session/SESSION_ID.json`


```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/checkout/sessions/SESSION_ID.json'
```

```ruby
# Coming soon!
```

```php?start_inline=1
// Coming soon!

```

```swift
// Coming soon!
```

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
         "tax_id":null,
         "business_number":null
      },
      "permalink":"http://quaderno.lvh.me:3000/checkout/session/8ccf3fdc42b85800188b113b81d3e4212ef094b3"
   }
```

## Update a session

Updates the specified session by setting the values of the parameters passed. Any parameters not provided will be left unchanged. Only pending sessions can be updated.

This request accepts mostly the same arguments as the session creation call.

> `PUT /checkout/session/SESSION_ID.json`


key                                  | type           | description
-------------------------------------|----------------|-----------------------------------------
`id`                                 | integer        | Unique identifier for the object.
`billing_details_collection`         | string         | The value for whether Checkout collected the customer’s billing details. Values are `auto` and `required` (default to `required`).
`cancel_url`                         | string         | The URL the customer will be directed to if they decide to cancel payment and return to your website.
`coupon_collection`                  | boolean        | The value for whether Checkout collected coupons.
`custom`                             | object         |
`customer`                           | object         |
`items`                              | array          | The list of products purchased by the customer.
`locale`                             | string         | The 2-letter ISO code of the language the Checkout is displayed in. Values are `auto`, `ca`, `de`, `en`, `es`, `fi`, `fr`, `hu`, `nl`, `sv`, and `no`.
`payment_methods`                    | array          | Values are card and paypal.
`success_url`                        | string         | The URL the customer will be directed to after the payment is successful.


