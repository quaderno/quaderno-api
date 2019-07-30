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