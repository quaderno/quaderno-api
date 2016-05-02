# Credits

An credit is an offer that you give a client in order to get a specific job. With time, credits are usually turned into issued invoices.

## Create a credit

> `POST /credits.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     -d '{
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
        }' \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/credits.json'
```

```php?start_inline=1
$credit = new QuadernoCredit(array(
                                 'po_number' => '',
                                 'currency' => 'USD',
                                 'tag_list' => 'playboy, businessman'));
$item = new QuadernoDocumentItem(array(
                               'description' => 'Whiskey',
                               'unit_price' => 20.0,
                               'reference' => 'item_code_X',
                               'quantity' => 1.0));
$contact = QuadernoContact::find('5059bdbf2f412e0901000024');

$credit->addItem($item);
$credit->addContact($contact);

$credit->save(); // Returns true (success) or false (error)
```

```ruby
params = {
  contact_id: '5059bdbf2f412e0901000024',
  contact_name: 'STARK',
  po_number: '',
  currency: 'USD',
  tag_list: 'playboy, businessman',
  items_attributes: [
    {
      description: 'Whiskey',
      quantity: '1.0',
      unit_price: '20.0',
      discount_rate: '0.0',
      reference: 'item_code_X'
    }
  ],
}
Quaderno::Credit.create(params) #=> Quaderno::Credit
```

```swift?start_inline=1
TODO!
```

`POST`ing to `/credits.json` will create a new credit from the parameters passed.

This will return `201 Created` and the current JSON representation of the credit if the creation was a success, along with the location of the new credit in the `url` field.

### Mandatory Fields

Key          | Description
-------------|------------------------------------------------------------------------------------------
`credit_id` / `credit`       | Either the ID of an existing credit or the JSON object for an existing/new credit.
`items_attributes` | An array of hashes which contains the `description`, `quantity`, `unit_price` and `discount_rate` of each item. If you want to have stock tracking, also pass the item code as the `reference` attribute. You may also add items to a credit by passing the `reference` of a pre-existing item.

<aside class="notice">
If you pass a `credit` JSON object instead of a `credit_id`, and the first and last name combination does not match any of your existing credits, a new one will be created, otherwise a new credit will be created for the existing credit.<br /><br />

<p>Please note that you can pass <strong>either</strong> credit_id or credit, but if you pass both then results may not be what you expect.</p>

<p>Allowed credit fields are the same as when creating a credit via the dedicated <a href="#credits">Contacts API</a>.</p>
</aside>

### Credit States

Possible credit states are:

- `draft`
- `sent`
- `partial`
- `paid`
- `late`
- `archived`

You can set the credit state by passing the `state` attribute, but bear the following considerations in mind:

- The `paid` state is only reachable by adding a `payment` to the invoice and is also final, so **cannot be overwritten unless the payment is removed from the credit**.
- The `sent` state can only overwrite the state `draft`.

### Create an attachment during credit creation

```json
{
  "attachment":{
    "data":"aBaSe64EnCoDeDFiLe",
    "filename":"the_filename.png"
  }
}
```

Optionally, you can pass an attachment hash along with the rest of the parameters.

Fields:

Field      | Description
-----------|------------------------------------------------------------
`data`     | Contains a Base64 encoded string which represents the file.
`filename` | The attachment file name.

Valid file extensions are `pdf`, `txt`, `jpeg`, `jpg`, `png`, `xml`, `xls`, `doc`, `rtf` and `html`. Any other format will stop credit creation and return a `422 Unprocessable Entity` error.

## Retrieve: Get and filter all credits

> `GET /credits.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/credits.json'
```

```ruby
Quaderno::Credit.all() #=> Array
```

```php?start_inline=1
$credits = QuadernoCredit::find(); // Returns an array of QuadernoCredit
```

```swift
let client = Quaderno.Client(/* ... */)

let readCredit = Credit.read()
client.request(readCredit) { response in
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
    "postal_code":"10203"
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
        "url":"https://my-account.quadernoapp.com/api/v1/credits/507693322f412e0e2e00000f/payments/50aca7d92f412eda5200002c.json"
      },
    ],
    "payment_details":"Ask Jon",
    "notes":"",
    "state":"paid",
    "tag_list":["lasagna", "cat"],
    "secure_id":"7hef1rs7p3rm4l1nk",
    "permalink":"https://quadernoapp.com/credit/7hef1rs7p3rm4l1nk",
    "pdf":"https://quadernoapp.com/credit/7hef1rs7p3rm4l1nk.pdf",
    "url":"https://my-account.quadernoapp.com/api/v1/credits/507693322f412e0e2e00000f"
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
    "permalink":"https://my-account.quadernoapp.com/credit/7hes3c0ndp3rm4l1nk",
    "pdf":"https://my-account.quadernoapp.com/credit/7hes3c0ndp3rm4l1nk.pdf",
    "url":"https://my-account.quadernoapp.com/api/v1/credits/507693322f412e0e2e0000da"
  },
]
```

`GET`ting from `/credits.json` will return all the user's credits.

You can filter the results in a few ways:

- By `number`, `contact_name` or `po_number` by passing the `q` parameter in the url like `?q=KEYWORD`.
- By date range, passing the `date` parameter in the url like `?date=DATE1,DATE2`.
- By state, passing the `state` parameter like `?state=STATE`.
- By contact, passing the contact ID in the `contact` parameter, like `?contact=3231`.
- By `exchange_rate`, if the credit currency differs from your account currency.

## Retrieve: Get a single credit

> `GET /credits/CREDIT_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/credits/CREDIT_ID.json'
```

```ruby
Quaderno::Credit.find(CREDIT_ID) #=> Quaderno::Credit
```

```php?start_inline=1
$credit = QuadernoCredit::find('CREDIT_ID'); // Returns a QuadernoCredit
```

```swift
let client = Quaderno.Client(/* ... */)

let readCredit = Credit.read()
client.request(readCredit) { response in
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
  "permalink":"https://my-account.quadernoapp.com/credit/7hef1rs7p3rm4l1nk",
  "pdf":"https://my-account.quadernoapp.com/credit/7hef1rs7p3rm4l1nk.pdf",
  "url":"https://my-account.quadernoapp.com/api/v1/credits/507693322f412e0e2e0000da"
}
```

`GET`ting from `/credits/CREDIT_ID.json` will return that specific credit.

## Update a credit

> `PUT /credits/CREDIT_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X PUT \
     -d '{ \
            "contact_id":"505c3b402f412e0248000044",
            "contact_name":"Dick Dastardly" \
         }' \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/credits/CREDIT_ID.json'
```

```ruby
params = {
    contact_id: '505c3b402f412e0248000044',
    contact_name: 'Dick Dastardly'
}
Quaderno::Credit.update(CREDIT_ID, params) #=> Quaderno::Credit
```

```php?start_inline=1
$credit->contact_id = '505c3b402f412e0248000044';
$credit->contact_name = 'Dick Dastardly';
$credit->save();
```

```swift?start_inline=1
// TODO
```

`PUT`ting to `/credits/CREDIT_ID.json` will update the credit with the passed parameters.

This will return `200 OK` along with the current JSON representation of the credit if successful.

## Delete a credit

> `DELETE /credits/CREDIT_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X DELETE 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/credits/CREDIT_ID.json'
```

```ruby
Quaderno::Credit.delete(CREDIT_ID) #=> Boolean
```

```php?start_inline=1
$credit->delete();
```

```swift?start_inline=1
// TODO
```

`DELETE`ing to `/credit/CREDIT_ID.json` will delete the specified credit and returns `204 No Content` if successful.

## Deliver (Send) a credit

`GET`ting `/credits/CREDIT_ID/deliver.json` will send the invoice to the assigned contact email. This will return `200 OK` if successful, along with a JSON representation of the invoice.

If the destination contact does not have an email address you will receive a `422 Unprocessable Entity` error.
