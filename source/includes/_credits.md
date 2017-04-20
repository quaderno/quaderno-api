# Credits

A credit note is a reverse invoice; something to cancel out - partially or completely - an invoice you have issued in the past. It may be for the full amount of an invoice or it may be for less in the case of a partial refund.

## Create a credit

> `POST /credits.json`

```shell
# body.json
{
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
  "custom_metadata":{
    "a_custom_key":"a custom value"
  }
}

curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     --data-binary @body.json \
     'https://ACCOUNT_NAME.quadernoapp.com/api/credits.json'
```

```php?start_inline=1
$credit = new QuadernoCredit(array(
                                 'po_number' => '',
                                 'currency' => 'USD',
                                 'tag_list' => array('playboy', 'businessman'),
                                 'custom_metadata' => array('a_custom_key' => 'a custom value')));
$item = new QuadernoDocumentItem(array(
                               'description' => 'Whiskey',
                               'unit_price' => 20.0,
                               'reference' => 'ITEM_ID',
                               'quantity' => 1.0));
$contact = QuadernoContact::find('5059bdbf2f412e0901000024');

$credit->addItem($item);
$credit->addContact($contact);

$credit->save(); // Returns true (success) or false (error)
```

```ruby
contact = Quaderno::Contact.find('50603e722f412e0435000024') #=> Quaderno::Contact

params = {
  contact_id: contact.id,
  contact_name: 'STARK',
  po_number: '',
  currency: 'USD',
  tag_list: ['playboy', 'businessman'],
  items_attributes: [
    {
      description: 'Whiskey',
      quantity: '1.0',
      unit_price: '20.0',
      discount_rate: '0.0',
    }
  ],
  custom_metadata: {
    a_custom_key: 'a custom value'
  }
}

Quaderno::Credit.create(params) #=> Quaderno::Credit
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let params : [String: Any] = [
 "contact_id": "5059bdbf2f412e0901000024",
 "contact_name": "STARK",
 "po_number": "",
 "currency": "USD",
 "tag_list": ["playboy", "businessman"]
]

let createCredit = Credit.create(params)
client.request(createCredit) { response in
    // response will contain the result of the request.
}
```

`POST`ing to `/credits.json` will create a new credit from the parameters passed.

This will return `201 Created` and the current JSON representation of the credit if the creation was a success, along with the location of the new credit in the `url` field.

### Attributes

Attribute       | Mandatory                                  | Type/Description
----------------|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
number          | no                                         | String(255 chars) Available for invoices, credit_notes, receipts and estimates. Automatic numbering is used by default. Validates uniqueness
issue_date      | no                                         | String(255 chars) Available for all except for `recurring`. Defaults to the current date. Format YYYY-MM-DD
po_number       | no                                         | String(255 chars)
due_date        | no                                         | String(255 chars) Available for invoices, expenses and credit notes. Format `YYYY-MM-DD`
currency        | no                                         | String(3 chars) Defaults to the account currency. En formato ISO 4217
tag_list        | no                                         | String(255 chars). Multiple tags should be separated by commas
payment_details | no                                         | Text
notes           | no                                         | Text
contact_id      | (Mandatory if `contact` is not present)    | ID
contact         | (Mandatory if `contact_id` is not present) | Hash with a contact data for creation
country         | no                                         | String(2 chars) `ISO 3166-1 alpha-2`. Available for updates
street_line_1   | no                                         | String(255 chars). Available for updates
street_line_2   | no                                         | String(255 chars)
city            | no                                         | String(255 chars). Available for updates
region          | no                                         | String(255 chars). Available for updates
postal_code     | no                                         | String(255 chars). Available for updates
items_attributes| **yes**                                    | Array of document items (check available attributes for document items below). No more than 200 items are allowed in a request. To add more use subsequent update requests. Maximum items per document are limited up to 1000 items.
payment_method  | no                                         | Create a paid document in a single request. One of the following: `credit_card`, `cash`, `wire_transfer`, `direct_debit`, `check`, `promissory_note`, `iou`, `paypal` or `other`
custom_metadata | no                                         | Key-value data. You can have up to 20 keys, with key names up to 40 characters long and values up to 500 characters long.

<aside class="notice">
If you pass a `contact` JSON object instead of a `contact_id`, and the first and last name combination does not match any of your existing credits, a new one will be created, otherwise a new credit will be created for the existing credit.<br /><br />

<p>Please note that you can pass <strong>either</strong> credit_id or credit, but if you pass both then results may not be what you expect.</p>

<p>Allowed contact fields are the same as when creating a contact via the dedicated <a href="#credits">Contacts API</a>.</p>
</aside>

### Document Item Attributes

Attribute     | Mandatory                                | Type/Description
--------------|------------------------------------------|----------------------------------------------------------------------------------------------------------------------
id            | no                                       | ID. Available only for updates
description   | **yes**                                  | String(255 chars)
quantity      | no                                       | Decimal. Defaults to 1.0
unit_price    | **yes**                                  | Decimal
total_amount  | Mandatory if `unit_price` is not present | Decimal. Cents version available as `total_amount_cents`
discount_rate | no                                       | Decimal
tax_1_name    | Mandatory if `tax_1_rate` is present     | String(255 chars)
tax_1_rate    | Mandatory if `tax_1_name` is present     | Decimal between -100.00 and 100.00 (not included)
tax_1_country | no                                       | String(2 chars). Defaults to the contact's country
tax_2_name    | Mandatory if `tax_2_rate` is present     | String(255 chars)
tax_2_rate    | Mandatory if `tax_2_name` is present     | Decimal between -100.00 and 100.00 (not included)
tax_2_country | no                                       | String(2 chars). Defaults to the contact's country
reference     | no                                       | String(255 chars) Code (`code`) of an existing item. **If present none of the mandatory attributes are mandatory (as you are referencing an item that already exists)**
_destroy      | no                                       | Set it to 1 if you want to remove the document item selected by ID. Available only for updates


### Credit States

Possible credit states are:

- `outstanding`
- `paid`
- `late`
- `archived`

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
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/credits.json'
```

```ruby
Quaderno::Credit.all() #=> Array
```

```php?start_inline=1
$credits = QuadernoCredit::find(); // Returns an array of QuadernoCredit
```

```swift
let client = Quaderno.Client(/* ... */)

let listCredits = Credit.list(pageNum)
client.request(listCredits) { response in
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
    "country":"US",
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
        "unit_price_cents":"375",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "reference":"Awesome!",
        "subtotal_cents":"9375",
        "discount_cents":"0",
        "gross_amount_cents":"9375"
      }
    ],
    "subtotal_cents":"9375",
    "discount_cents":"0",
    "taxes":[],
    "total_cents":"9375",
    "payments":[
      {
        "id":"50aca7d92f412eda5200002c",
        "date":"2012-11-21",
        "payment_method":"credit_card",
        "amount_cents":"9375",
        "url":"https://my-account.quadernoapp.com/api/credits/507693322f412e0e2e00000f/payments/50aca7d92f412eda5200002c.json"
      },
    ],
    "payment_details":"Ask Jon",
    "notes":"",
    "state":"paid",
    "tag_list":["lasagna", "cat"],
    "secure_id":"7hef1rs7p3rm4l1nk",
    "permalink":"https://quadernoapp.com/credit/7hef1rs7p3rm4l1nk",
    "pdf":"https://quadernoapp.com/credit/7hef1rs7p3rm4l1nk.pdf",
    "url":"https://my-account.quadernoapp.com/api/credits/507693322f412e0e2e00000f",
    "custom_metadata":{}
  },

  {
    "id":"507693322f412e0e2e0000da",
    "number":"0000047",
    "issue_date":"2012-10-11",
    "contact":{
      "id":"5073f9c22f412e02d00004cf",
      "full_name":"Teenage Mutant Ninja Turtles"
    },
    "country":"US",
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
        "unit_price_cents":"6000",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "reference":"Even the bad ones taste good!",
        "subtotal_cents":"6000",
        "discount_cents":"0",
        "gross_amount_cents":"6000"
      }
    ],
    "subtotal_cents":"6000",
    "discount_cents":"0",
    "taxes":[],
    "total_cents":"6000",
    "payments":[],
    "payment_details":"",
    "notes":"",
    "state":"outstanding",
    "tag_list":["pizza", "turtles"],
    "secure_id":"7hes3c0ndp3rm4l1nk",
    "permalink":"https://my-account.quadernoapp.com/credit/7hes3c0ndp3rm4l1nk",
    "pdf":"https://my-account.quadernoapp.com/credit/7hes3c0ndp3rm4l1nk.pdf",
    "url":"https://my-account.quadernoapp.com/api/credits/507693322f412e0e2e0000da",
    "custom_metadata":{}
  }
]
```

`GET`ting from `/credits.json` will return all the user's credits.

You can filter the results in a few ways:

- By `number`, `contact_name` or `po_number` by passing the `q` parameter in the url like `?q=KEYWORD`.
- By date range, passing the `date` parameter in the url like `?date=DATE1,DATE2`.
- By state, passing the `state` parameter like `?state=STATE`.
- By contact, passing the contact ID in the `contact` parameter, like `?contact=3231`.

## Retrieve: Get a single credit

> `GET /credits/CREDIT_ID.json`

```shell
curl -u YOUR_API_KEY:x \_cents
    -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/credits/CREDIT_ID.json'
```

```ruby
Quaderno::Credit.find(CREDIT_ID) #=> Quaderno::Credit
```

```php?start_inline=1
$credit = QuadernoCredit::find('CREDIT_ID'); // Returns a QuadernoCredit
```

```swift
let client = Quaderno.Client(/* ... */)

let readCredit = Credit.read(CREDIT_ID)
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
  "country":"US",
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
      "unit_price_cents":"6000",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "reference":"Even the bad ones taste good!",
      "subtotal_cents":"6000",
      "discount_cents":"0",
      "gross_amount_cents":"6000"
    }
  ],
  "subtotal_cents":"6000",
  "discount_cents":"0",
  "taxes":[],
  "total_cents":"6000",
  "payments":[],
  "payment_details":"",
  "notes":"",
  "state":"outstanding",
  "tag_list":["lasagna", "cat"],
  "secure_id":"7hef1rs7p3rm4l1nk",
  "permalink":"https://my-account.quadernoapp.com/credit/7hef1rs7p3rm4l1nk",
  "pdf":"https://my-account.quadernoapp.com/credit/7hef1rs7p3rm4l1nk.pdf",
  "url":"https://my-account.quadernoapp.com/api/credits/507693322f412e0e2e0000da",
  "custom_metadata":{}
}
```

`GET`ting from `/credits/CREDIT_ID.json` will return that specific credit.

## Update a credit

> `PUT /credits/CREDIT_ID.json`

```shell
# body.json
{
  "tag_list":"whiskey, alcohol",
  "notes":"You better pay this time, Tony",
  "region":"Genosha",
  "custom_metadata":{"memo":"I think he is not paying again."}
}

curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X PUT \
     --data-binary @body.json \
     'https://ACCOUNT_NAME.quadernoapp.com/api/credits/CREDIT_ID.json'
```

```ruby
params = {
  tag_list: ['whiskey', 'alcohol'],
  notes: 'You better pay this time, Tony',
  region: 'Genosha',
  custom_metadata: {
    memo: 'I think he is not paying again.'
  }
}
Quaderno::Credit.update(CREDIT_ID, params) #=> Quaderno::Credit
```

```php?start_inline=1
$credit->tag_list = array("whiskey", "alcohol");
$credit->notes = "You better pay this time, Tony";
$credit->region = "Genosha";
$credit->custom_metadata = array("memo" => "I think he is not paying again.");

$credit->save();
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let params : [String: Any] = [
    "tag_list": ["whiskey", "alcohol"]
]

let updateCredit = Credit.update(CREDIT_ID, params)
client.request(updateCredit) { response in
    // response will contain the result of the request.
}
```

`PUT`ting to `/credits/CREDIT_ID.json` will update the credit with the passed parameters.

This will return `200 OK` along with the current JSON representation of the credit if successful.

## Delete a credit

> `DELETE /credits/CREDIT_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X DELETE 'https://ACCOUNT_NAME.quadernoapp.com/api/credits/CREDIT_ID.json'
```

```ruby
Quaderno::Credit.delete(CREDIT_ID) #=> Boolean
```

```php?start_inline=1
$credit->delete();
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let deleteCredit = Credit.delete(CREDIT_ID)
client.request(deleteCredit) { response in
    // response will contain the result of the request.
}
```

`DELETE`ing to `/credit/CREDIT_ID.json` will delete the specified credit and returns `204 No Content` if successful.

## Deliver (Send) a credit

`GET`ting `/credits/CREDIT_ID/deliver.json` will send the invoice to the assigned contact email. This will return `200 OK` if successful, along with a JSON representation of the invoice.

If the destination contact does not have an email address you will receive a `422 Unprocessable Entity` error.
