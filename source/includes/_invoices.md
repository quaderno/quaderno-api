# Invoices

An invoice is a detailed list of goods shipped or services rendered, with an account of all costs.

## Create

> `POST /invoices.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     -d { \
          "contact_id":"5059bdbf2f412e0901000024", \
          "contact_name":"STARK", \
          "po_number":"", \
          "currency":"USD", \
          "tag_list":"playboy, businessman", \
          "items_attributes":[ \
            { \
              "description":"Whiskey", \
              "quantity":"1.0", \
              "unit_price":"20.0", \
              "discount_rate":"0.0", \
              "reference":"item_code_X" \
            } \
          ], \
        } \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/invoices.json'
```

```php?start_inline=1
$contact = new QuadernoInvoice(array(
                                 'first_name' => 'Tony',
                                 'kind' => 'person',
                                 'contact_name' => 'Stark'));

$contact->save(); // Returns true (success) or false (error)
```

```ruby
# Using new hash syntax
params = {
    first_name: 'Tony',
    kind: 'person',
    contact_name: 'Stark'
}
Quaderno::Invoice.create(params) #=> Quaderno::Invoice
```

```swift?start_inline=1
TODO!
```

## Read: Get and filter all invoices

> `GET /invoices.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/invoices.json'
```

```ruby
Quaderno::Invoice.all() #=> Array
```

```php?start_inline=1
$contacts = QuadernoInvoice::find(); // Returns an array of QuadernoInvoice
```

```swift
let client = Quaderno.Client(/* ... */)

let readInvoice = Invoice.read()
client.request(readInvoice) { response in
  // response will contain the result of the request.
}
```

```json
[
  {
    "id":"507693322f412e0e2e00000f",
    "number":"0000047",
    "issue_date":"2012-10-11",
    "contact":{
      "id":"5073f9c22f412e02d0000032",
      "full_name":"Garfield"
    },
    "street_line_1":"Street 23",
    "street_line_2":"",
    "city":"New York",
    "region":"New York",
    "postal_code":"10203",
    "po_number":null,
    "due_date":null,
    "currency":"USD",
    "exchange_rate":"0.680309",
    "items":[
      {
        "id":"48151623429",
        "description":"lasagna",
        "quantity":"25.0",
        "unit_price":"3.75",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "reference":"Awesome!",
        "subtotal":"$93.75",
        "discount":"$0.00",
        "gross_amount":"$93.75"
      }
    ],
    "subtotal":"$93.75",
    "discount":"$0.00",
    "taxes":[],
    "total":"$93.75",
    "payments":[
      {
        "id":"50aca7d92f412eda5200002c",
        "date":"2012-11-21",
        "payment_method":"credit_card",
        "amount":"\u20ac93.75",
        "url":"https://ACCOUNT_NAME.quadernoapp.com/api/v1/invoices/507693322f412e0e2e00000f/payments/50aca7d92f412eda5200002c.json"
      },
    ],
    "payment_details":"Ask Jon",
    "notes":"",
    "state":"paid",
    "tag_list":["lasagna", "cat"],
    "secure_id":"7hef1rs7p3rm4l1nk",
    "permalink":"https://quadernoapp.com/invoice/7hef1rs7p3rm4l1nk",
    "pdf":"https://quadernoapp.com/invoice/7hef1rs7p3rm4l1nk.pdf",
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/v1/invoices/507693322f412e0e2e00000f"
  },

  {
    "id":"507693322f412e0e2e0000da",
    "number":"0000047",
    "issue_date":"2012-10-11",
    "contact":{
      "id":"5073f9c22f412e02d00004cf",
      "full_name":"Teenage Mutant Ninja Turtles"
    },
    "street_line_1":"Melrose Ave, Sewer #3",
    "street_line_2":"",
    "city":"New York",
    "region":"New York",
    "postal_code":"",
    "po_number":null,
    "due_date":null,
    "currency":"USD",
    "exchange_rate":"0.680309",
    "items":[
      {
        "id":"481516234291",
        "description":"pizza",
        "quantity":"15.0",
        "unit_price":"60.0",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "reference":"Even the bad ones taste good!",
        "subtotal":"$60.00",
        "discount":"$0.00",
        "gross_amount":"$60.00"
      }
    ],
    "subtotal":"$60.00",
    "discount":"$0.00",
    "taxes":[],
    "total":"$60.00",
    "payments":[],
    "payment_details":"",
    "notes":"",
    "state":"draft",
    "tag_list":["pizza", "turtles"],
    "secure_id":"7hes3c0ndp3rm4l1nk",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/invoice/7hes3c0ndp3rm4l1nk",
    "pdf":"https://ACCOUNT_NAME.quadernoapp.com/invoice/7hes3c0ndp3rm4l1nk.pdf",
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/v1/invoices/507693322f412e0e2e0000da"
  },
]
```

## Read: Get a single invoice

> `GET /invoices/1.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/invoices/1.json'
```

```ruby
Quaderno::Invoice.find(id) #=> Quaderno::Invoice
```

```php?start_inline=1
$contact = QuadernoInvoice::find('IDTOFIND'); // Returns a QuadernoInvoice
```

```swift
let client = Quaderno.Client(/* ... */)

let readInvoice = Invoice.read()
client.request(readInvoice) { response in
  // response will contain the result of the request.
}
```

```json
{
  "id":"507693322f412e0e2e0000da",
  "number":"0000047",
  "issue_date":"2012-10-11",
  "contact":{
    "id":"5073f9c22f412e02d00004cf",
    "full_name":"Teenage Mutant Ninja Turtles"
  },
  "street_line_1":"Melrose Ave, Sewer #3",
  "street_line_2":"",
  "city":"New York",
  "region":"New York",
  "postal_code":"",
  "po_number":null,
  "due_date":null,
  "currency":"USD",
  "exchange_rate":"0.680309",
  "items":[
    {
      "id":"48151623429",
      "description":"pizza",
      "quantity":"15.0",
      "unit_price":"60.0",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "reference":"Even the bad ones taste good!",
      "subtotal":"$60.00",
      "discount":"$0.00",
      "gross_amount":"$60.00"
    }
  ],
  "subtotal":"$60.00",
  "discount":"$0.00",
  "taxes":[],
  "total":"$60.00",
  "payments":[],
  "payment_details":"",
  "notes":"",
  "state":"draft",
  "tag_list":["lasagna", "cat"],
  "secure_id":"7hef1rs7p3rm4l1nk",
  "permalink":"https://ACCOUNT_NAME.quadernoapp.com/invoice/7hef1rs7p3rm4l1nk",
  "pdf":"https://ACCOUNT_NAME.quadernoapp.com/invoice/7hef1rs7p3rm4l1nk.pdf",
  "url":"https://ACCOUNT_NAME.quadernoapp.com/api/v1/invoices/507693322f412e0e2e0000da"
}
```

## Update

> `PUT /invoices/1.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X PUT \
     -d {"first_name":"Anthony"} \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/invoices/1.json'
```

```ruby
Quaderno::Invoice.update(id, params) #=> Quaderno::Invoice
```

```php?start_inline=1
$contact->first_name = 'Joey';
$contact->save();
```

```swift?start_inline=1
// TODO
```

## Delete

> `DELETE /invoices/1.json` returns `204 No Content` if successful.

```shell
curl -u YOUR_API_KEY:x \
     -X DELETE 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/invoices/1.json'
```

```ruby
Quaderno::Invoice.delete(id) #=> Boolean
```

```php?start_inline=1
$contact->delete();
```

```swift?start_inline=1
// TODO
```
