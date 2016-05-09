# Receipts

A receipt is a detailed list of goods shipped or services rendered, with an account of all costs. It differs from invoices in a few key ways (mostly in that it does not require customer information in a legal sense). Read more [here](http://support.quaderno.io/article/37-what-is-a-sales-receipt).

## Create a receipt

> `POST /receipts.json`

```shell
# body.json
{
  "payment_method":"credit_card",
  "contact_id":"5059bdbf2f412e0901000024",
  "contact_name":"STARK",
  "po_number":"",
  "currency":"USD",
  "tag_list":"playboy, businessman",
  "items_attributes":[
    {
      "description":"Whiskey",
      "quantity":"1.0",
      "unit_price":"20.0",
      "discount_rate":"0.0",
      "reference":"item_code_X"
    }
  ],
}

curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     --data-binary @body.json \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/receipts.json'
```

```php?start_inline=1
$receipt = new QuadernoReceipt(array(
                                 'contact_id' => '5059bdbf2f412e0901000024',
                                 'payment_method' => 'credit_card',
                                 'po_number' => '',
                                 'currency' => 'USD',
                                 'tag_list' => 'playboy, businessman'));
$item = new QuadernoDocumentItem(array(
                               'description' => 'Pizza bagels',
                               'unit_price' => 9.99,
                               'quantity' => 20));
$contact = QuadernoContact::find('5059bdbf2f412e0901000024');

$receipt->addItem($item);
$receipt->addContact($contact);

$receipt->save(); // Returns true (success) or false (error)
```

```ruby
params = {
 contact_id: '5059bdbf2f412e0901000024',
 contact_name: 'STARK',
 payment_method: 'credit_card',
 po_number: '',
 currency: 'USD',
 tag_list: 'playboy, businessman',
 items_attributes: [
   {
     description: 'Whiskey',
     quantity: '1.0',
     unit_price: '20.0',
     discount_rate: '0.0',
     reference: 'ITEM_ID'
   }
 ]
}
Quaderno::Receipt.create(params) #=> Quaderno::Receipt
```

```swift?start_inline=1
TODO!
```

`POST`ing to `/receipts.json` will create a new contact from the parameters passed.

This will return `201 Created` and the current JSON representation of the receipt if the creation was a success, along with the location of the new receipt in the `url` field.

### Mandatory Fields

Key          | Description
-------------|------------------------------------------------------------------------------------------
`payment_method`       | Method of payment (`credit_card`, `cash`, `wire_transfer`, `direct_debit`, `check`, `promissory_note`, `iou`, `paypal` or `other`).
`contact_id` / `contact`       | Either the ID of an existing contact or the JSON object for an existing/new contact.
`items_attributes` | An array of hashes which contains the `description`, `quantity`, `unit_price` and `discount_rate` of each item. If you want to have stock tracking, also pass the item code as the `reference` attribute. You may also add items to a receipt by passing the `reference` of a pre-existing item.

<aside class="notice">
If you pass a `contact` JSON object instead of a `contact_id`, and the first and last name combination does not match any of your existing contacts, a new one will be created, otherwise a new receipt will be created for the existing contact.<br /><br />

<p>Please note that you can pass <strong>either</strong> contact_id or contact, but if you pass both then results may not be what you expect.</p>

<p>Allowed contact fields are the same as when creating a contact via the dedicated <a href="#contacts">Contacts API</a>.</p>
</aside>

<aside class="warning">
You can't create a receipt for a contact with a tax ID. If the contact has a tax ID a regular (full) invoice should be created.
</aside>

### Receipt States

Possible receipt states are:

- `paid` (default state)
- `invoiced` (final state reached when the receipt is invoiced and *cannot be undone*)

You can set the receipt state by passing the `state` attribute.

## Retrieve: Get and filter all receipts

> `GET /receipts.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/receipts.json'
```

```ruby
Quaderno::Receipt.all() #=> Array
```

```php?start_inline=1
$receipts = QuadernoReceipt::find(); // Returns an array of QuadernoReceipt
```

```swift
let client = Quaderno.Client(/* ... */)

let readReceipt = Receipt.list(pageNum)
client.request(readReceipt) { response in
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
        "amount":"$93.75",
      },
    ],
    "notes":"",
    "state":"paid",
    "tag_list":["lasagna", "cat"],
    "secure_id":"7hef1rs7p3rm4l1nk",
    "permalink":"https://quadernoapp.com/receipt/7hef1rs7p3rm4l1nk",
    "pdf":"https://quadernoapp.com/receipt/7hef1rs7p3rm4l1nk.pdf",
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/v1/receipts/507693322f412e0e2e00000f"
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
    "currency":"USD",
    "exchange_rate":"0.680309",
    "items":[
      {
        "id":"481516234291",
        "description":"pizza",
        "quantity":"1.0",
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
    "payments":[
      {
        "id":"50aca7d92f412eda5200002ssdc",
        "date":"2012-11-21",
        "payment_method":"credit_card",
        "amount":"$60.00",
      },
    ],
    "notes":"",
    "state":"draft",
    "tag_list":["pizza", "turtles"],
    "secure_id":"7hes3c0ndp3rm4l1nk",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/receipt/7hes3c0ndp3rm4l1nk",
    "pdf":"https://ACCOUNT_NAME.quadernoapp.com/receipt/7hes3c0ndp3rm4l1nk.pdf",
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/v1/receipts/507693322f412e0e2e0000da"
  },
]
```

`GET`ting from `/receipts.json` will return all the user's receipts.

You can filter the results in a few ways:

- By `number`, `contact_name` or `po_number` by passing the `q` parameter in the url like `?q=KEYWORD`.
- By date range, passing the `date` parameter in the url like `?date=DATE1,DATE2`.
- By state, passing the `state` parameter like `?state=STATE`.
- By contact, passing the contact ID in the `contact` parameter, like `?contact=3231`.
- By `exchange_rate`, if the receipt currency differs from your account currency.

## Retrieve: Get a single receipt

> `GET /receipts/RECEIPT_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/receipts/RECEIPT_ID.json'
```

```ruby
Quaderno::Receipt.find(RECEIPT_ID) #=> Quaderno::Receipt
```

```php?start_inline=1
$receipt = QuadernoReceipt::find('RECEIPT_ID'); // Returns a QuadernoReceipt
```

```swift
let client = Quaderno.Client(/* ... */)

let readReceipt = Receipt.list(pageNum)
client.request(readReceipt) { response in
  // response will contain the result of the request.
}
```

```json
{
  "id":13638223434,
  "number":"0000001",
  "issue_date":"2016-05-05",
  "created_at":1462453039,
  "contact":{
    "id":424230933,
    "full_name":"Toxic Crusader"
  },
  "street_line_1":null,
  "street_line_2":null,
  "city":null,
  "region":null,
  "postal_code":null,
  "po_number":null,
  "currency":"EUR",
  "items":[{
    "id":2621550,
    "description":"Mop",
    "quantity":"1.0",
    "unit_price":"30.0",
    "discount_rate":"0.0",
    "tax_1_name":"",
    "tax_1_rate":null,
    "tax_2_name":"",
    "tax_2_rate":null,
    "reference":null,
    "subtotal":"30.00 €",
    "discount":"0.00 €",
    "gross_amount":"30.00 €"
  }],
  "subtotal":"30.00 €",
  "discount":"0.00 €",
  "taxes":[],
  "total":"30.00 €",
  "payments":[{
    "id":972444,
    "date":"2016-05-05",
    "payment_method":"credit_card",
    "amount":"30.00 €"
  }],
  "notes":null,
  "state":"paid",
  "tag_list":[],
  "secure_id":"f6a78e399aa6369e3b0329da78cc24534bc1934d",
  "permalink":
  "http://ACCOUNT_NAME.quadernoapp.com/receipt/f6a78e399aa6369e3b0329da78cc24534bc1934dzzRot",
  "pdf":"https://ACCOUNT_NAME.quadernoapp.com/receipt/f6a78e399aa6369e3b0329da78cc24534bc1934dzzRot.pdf",
  "url":"https://ACCOUNT_NAME.quadernoapp.com/api/v1/receipts/13638223434.json",
}
```

`GET`ting from `/receipts/RECEIPT_ID.json` will return that specific receipt.

If you have connected Quaderno and Stripe, you can also `GET /stripe/charges/STRIPE_CHARGE_ID.json` to get the Quaderno receipt for a Stripe charge.

## Update a receipt

> `PUT /receipts/RECEIPT_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X PUT \
     -d '{"notes":"You better pay this time, Tony."}' \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/receipts/RECEIPT_ID.json'
```

```ruby
params = {
    notes: 'You better pay this time, Tony.'
}
Quaderno::Receipt.update(RECEIPT_ID, params) #=> Quaderno::Receipt
```

```php?start_inline=1
$receipt->notes = 'You better pay this time, Tony.';
$receipt->save();
```

```swift?start_inline=1
// TODO
```

`PUT`ting to `/receipts/RECEIPT_ID.json` will update the receipt with the passed parameters.

This will return `200 OK` along with the current JSON representation of the receipt if successful.

## Delete a receipt

> `DELETE /receipts/RECEIPT_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X DELETE 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/receipts/RECEIPT_ID.json'
```

```ruby
Quaderno::Receipt.delete(RECEIPT_ID) #=> Boolean
```

```php?start_inline=1
$receipt->delete();
```

```swift?start_inline=1
// TODO
```

`DELETE`ing to `/receipt/RECEIPT_ID.json` will delete the specified receipt and returns `204 No Content` if successful.

## Deliver (Send) a receipt

`GET`ting `/receipts/RECEIPT_ID/deliver.json` will send the receipt to the assigned contact email. This will return `200 OK` if successful, along with a JSON representation of the receipt.

If the destination contact does not have an email address you will receive a `422 Unprocessable Entity` error.
