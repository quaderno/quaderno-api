# Recurring

A recurring is a special document that periodically renews itself and generates a recurring invoice or expense.

## Create a recurring

> `POST /recurring.json`

```shell
# body.json
{ "recurring_document":"expense",
  "frequency":"monthly",
  "start_date":"2015-08-01",
  "contact_id":"5059bdbf2f412e0901000024",
  "delivery":"send",
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
     'https://ACCOUNT_NAME.quadernoapp.com/api/recurring.json'
```

```php?start_inline=1
$recurring = new QuadernoRecurring(array(
                                 'po_number' => '',
                                 'frequency' => 'monthly',
                                 'start_date' => new DateTime('2015-08-01'),
                                 'currency' => 'USD',
                                 'tag_list' => array('playboy', 'businessman'),
                                 'custom_metadata' => array('a_custom_key' => 'a custom value')));
$item = new QuadernoDocumentItem(array(
                               'description' => 'Pizza bagels',
                               'unit_price' => 9.99,
                               'quantity' => 20));
$contact = QuadernoContact::find('5059bdbf2f412e0901000024');

$recurring->custom_metadata = array('a_custom_key' => 'a custom value');
$recurring->addItem($item);
$recurring->addContact($contact);

$recurring->save(); // Returns true (success) or false (error)
```

```ruby
contact = Quaderno::Contact.find('50603e722f412e0435000024') #=> Quaderno::Contact

params = {
  contact_id: contact.id,
  po_number: '',
  currency: 'USD',
  tag_list: ['playboy', 'businessman'],
  frequency: 'monthly',
  start_date: Date.parse('2015-08-01'),
  items_attributes: [
    {
      description: 'Whiskey',
      quantity: '1.0',
      unit_price: '20.0',
      discount_rate: '0.0'
    }
  ],
  custom_metadata: {
    a_custom_key: 'a custom value'
  }
}
recurring = Quaderno::Recurring.create(params) #=> Quaderno::Recurring
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let params : [String: Any] = [
 "frequency": "monthly",
 "start_date": "2015-08-01",
 "contact_id": "5059bdbf2f412e0901000024",
 "contact_name": "STARK",
 "po_number": "",
 "currency": "USD",
 "tag_list": ["playboy", "businessman"]
]

let createRecurring = Recurring.create(params)
client.request(createRecurring) { response in
    // response will contain the result of the request.
}
```

`POST`ing to `/recurring.json` will create a new recurring from the parameters passed.

This will return `201 Created` and the current JSON representation of the recurring if the creation was a success, along with the location of the new recurring in the `url` field.

### Attributes

Attribute       | Mandatory                                  | Type/Description
----------------|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
number          | no                                         | String(255 chars) Available for recurrings, credit_notes, receipts and estimates. Automatic numbering is used by default. Validates uniqueness
issue_date      | no                                         | String(255 chars) Available for all except for `recurring`. Defaults to the current date. Format YYYY-MM-DD
po_number       | no                                         | String(255 chars)
due_date        | no                                         | String(255 chars) Available for recurrings, expenses and credit notes. Format `YYYY-MM-DD`
currency        | no                                         | String(3 chars) Defaults to the account currency. En formato ISO 4217
tag_list        | no                                         | String(255 chars). Multiple tags should be separated by commas
payment_details | no                                         | Text
notes           | no                                         | Text
contact_id      | (Mandatory if `contact` is not present)    | ID
contact         | (Mandatory if `contact_id` is not present) | Hash with a contact data for creation
street_line_1   | no                                         | String(255 chars). Available for updates
street_line_2   | no                                         | String(255 chars)
city            | no                                         | String(255 chars). Available for updates
region          | no                                         | String(255 chars). Available for updates
postal_code     | no                                         | String(255 chars). Available for updates
items_attributes| **yes**                                    | Array of document items (check available attributes for document items below). No more than 200 items are allowed in a request. To add more use subsequent update requests. Maximum items per document are limited up to 1000 items.
payment_method  | no                                         | Create a paid document in a single request. One of the following: `credit_card`, `cash`, `wire_transfer`, `direct_debit`, `check`, `promissory_note`, `iou`, `paypal` or `other`
recurring_document  | no                                     |  `invoice` or `expense`. Defaults to `invoice`
start_date      |  **yes**                                   | Format `YYYY-MM-DD`.
end_date        | no                                         | Format `YYYY-MM-DD`.
frequency       | no                                         |  One of these values: `daily`, `weekly`, `biweekly`, `monthly`, `bimonthly`, `quarterly`, `semiyearly`, `yearly`, `biyearly`. Defaults to `monthly`.
delivery        | no                                         |  One of these values: `send`, `create`, `nothing`. Defaults to `send`.
due_days        | no                                         |  Positvive integer
custom_metadata | no                                         | Key-value data. You can have up to 20 keys, with key names up to 40 characters long and values up to 500 characters long.

<aside class="notice">
If you pass a `contact` JSON object instead of a `contact_id`, and the email or first and last name combination does not match any of your existing contacts, a new one will be created, otherwise a new recurring will be created for the existing contact. Only a `contact` object OR a `contact_id` property should be passed in the same call.<br /><br />

<p>Please note that you can pass <strong>either</strong> contact_id or contact, but if you pass both then results may not be what you expect.</p>

<p>Allowed contact fields are the same as when creating a contact via the dedicated <a href="#contacts">Contacts API</a>.</p>
</aside>

### Document Item Attributes

Attribute     | Mandatory                                | Type/Description
--------------|------------------------------------------|----------------------------------------------------------------------------------------------------------------------
id            | no                                       | ID. Available only for updates
description   | **yes**                                  | String(255 chars)
quantity      | no                                       | Decimal. Defaults to 1.0
unit_price    | **yes**                                  | Decimal
total_amount  | Mandatory if `unit_price` is not present | Decimal
discount_rate | no                                       | Decimal
tax_1_name    | Mandatory if `tax_1_rate` is present     | String(255 chars)
tax_1_rate    | Mandatory if `tax_1_name` is present     | Decimal between -100.00 and 100.00 (not included)
tax_1_country | no                                       | String(2 chars). Defaults to the contact's country
tax_1_region  | no                                       | String(255 chars). Recommendable to set for Canadian or United States taxes.
tax_1_county  | no                                       | String(255 chars). Recommendable to set for US sales taxes.
tax_1_city    | no                                       | String(255 chars). Recommendable to set for US sales taxes.
tax_1_county_code    | no                                       | String(255 chars). Recommendable to set for US sales taxes.
tax_1_city_code      | no                                       | String(255 chars). Recommendable to set for US sales taxes.
tax_1_transaction_type | no                              | String. Accepts `eservice`, `ebook` or `standard`. Defaults to your account "default tax type".
tax_2_name    | Mandatory if `tax_2_rate` is present     | String(255 chars)
tax_2_rate    | Mandatory if `tax_2_name` is present     | Decimal between -100.00 and 100.00 (not included)
tax_2_country | no                                       | String(2 chars). Defaults to the contact's country
tax_2_region  | no                                       | String(255 chars). Recommendable to set for Canadian or United States taxes.
tax_2_county  | no                                       | String(255 chars). Recommendable to set for US sales taxes.
tax_2_city    | no                                       | String(255 chars). Recommendable to set for US sales taxes.
tax_2_county_code    | no                                       | String(255 chars). Recommendable to set for US sales taxes.
tax_2_city_code      | no                                       | String(255 chars). Recommendable to set for US sales taxes.
tax_2_transaction_type | no                              | String. Accepts `eservice`, `ebook` or `standard`. Defaults to your account "default tax type".
reference     | no                                       | String(255 chars) Code (`code`) of an existing item. **If present none of the mandatory attributes are mandatory (as you are referencing an item that already exists)**
_destroy      | no                                       | Set it to 1 if you want to remove the document item selected by ID. Available only for updates

### Recurring States

Possible recurring states are:

- `active`
- `archived`

### Create an attachment during recurring creation

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

Valid file extensions are `pdf`, `txt`, `jpeg`, `jpg`, `png`, `xml`, `xls`, `doc`, `rtf` and `html`. Any other format will stop recurring creation and return a `422 Unprocessable Entity` error.

## Retrieve: Get and filter all recurrings

> `GET /recurring.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/recurring.json'
```

```ruby
Quaderno::Recurring.all() #=> Array
```

```php?start_inline=1
$recurrings = QuadernoRecurring::find(); // Returns an array of QuadernoRecurring
```

```swift
let client = Quaderno.Client(/* ... */)

let listRecurring = Recurring.list(pageNum)
client.request(listRecurring) { response in
  // response will contain the result of the request.
}
```

```json
[
  {
    "id":642069232,
    "recurring_document":"expense",
    "start_date":"2015-02-21",
    "end_date":"2015-06-21",
    "frequency":"monthly",
    "recurring_period":"months",
    "recurring_frequency":"1",
    "contact":{
      "id":176938,
      "full_name":"Chuck Testa"
    },
    "po_number":"",
    "due_days":null,
    "currency":"EUR",
    "items":[
      {
        "id":1589108,
        "description":"Something corporate",
        "quantity":"1.0",
        "unit_price_cents":"2300",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":null,
        "tax_2_name":"",
        "tax_2_rate":null,
        "reference":"",
        "subtotal_cents":"2300",
        "discount_cents":"0",
        "gross_amount_cents":"2300"
      }
    ],
    "subtotal_cents":"2300",
    "discount_cents":"0",
    "taxes":[],
    "total_cents":"2300",
    "payment_details":"",
    "notes":"",
    "state":"archived",
    "tag_list":[],
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/recurring/642069232.json",
    "custom_metadata":{}
  },
  {
    "id":6420739232,
    "recurring_document":"recurring",
    "start_date":"2015-01-02",
    "end_date":null,
    "frequency":"weekly",
    "recurring_period":"weeks",
    "recurring_frequency":"1",
    "contact":{
      "id":176939,
      "full_name":"Veronica Black"
    },
    "po_number":"",
    "due_days":null,
    "currency":"EUR",
    "items":[
      {
        "id":1589114,
        "description":"Friday stuff",
        "quantity":"1.0",
        "unit_price_cents":"3400",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":null,
        "tax_2_name":"",
        "tax_2_rate":null,
        "reference":"",
        "subtotal_cents":"3400",
        "discount_cents":"0",
        "gross_amount_cents":"3400"
      }
    ],
    "subtotal_cents":"3400",
    "discount_cents":"0",
    "taxes":[],
    "total_cents":"3400",
    "payment_details":"",
    "notes":"",
    "state":"active",
    "tag_list":[],
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/recurring/6420739232.json",
    "custom_metadata":{}
  }
]
```

`GET`ting from `/recurring.json` will return all the user's recurrings.

You can filter the results in a few ways:

- By `number`, `contact_name` or `po_number` by passing the `q` parameter in the url like `?q=KEYWORD`.
- By date range, passing the `date` parameter in the url like `?date=DATE1,DATE2`. Date range allows these formats: `2019-01-01,2019-12-31` or `2019/01/01,2019/12/31`.
- By state, passing the `state` parameter like `?state=STATE`.
- By contact, passing the contact ID in the `contact` parameter, like `?contact=3231`.

## Retrieve: Get a single recurring

> `GET /recurring/RECURRING_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/recurring/RECURRING_ID.json'
```

```ruby
Quaderno::Recurring.find(RECURRING_ID) #=> Quaderno::Recurring
```

```php?start_inline=1
$recurring = QuadernoRecurring::find('RECURRING_ID'); // Returns a QuadernoRecurring
```

```swift
let client = Quaderno.Client(/* ... */)

let readRecurring = Recurring.read(RECURRING_ID)
client.request(readRecurring) { response in
  // response will contain the result of the request.
}
```

```json
{
  "id":6420739232,
  "recurring_document":"expense",
  "start_date":"2015-01-02",
  "end_date":null,
  "frequency":"weekly",
  "recurring_period":"weeks",
  "recurring_frequency":"1",
  "contact":{
    "id":176939,
    "full_name":"Veronica Black"
  },
  "po_number":"",
  "due_days":null,
  "currency":"EUR",
  "items":[
    {
      "id":1589114,
      "description":"Friday stuff",
      "quantity":"1.0",
      "unit_price_cents":"3400",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":null,
      "tax_2_name":"",
      "tax_2_rate":null,
      "reference":"",
      "subtotal_cents":"3400",
      "discount_cents":"0",
      "gross_amount_cents":"3400"
    }
  ],
  "subtotal_cents":"3400",
  "discount_cents":"0",
  "taxes":[],
  "total_cents":"3400",
  "payment_details":"",
  "notes":"",
  "state":"active",
  "tag_list":[],
  "url":"https://ACCOUNT_NAME.quadernoapp.com/api/recurring/6420739232.json",
  "custom_metadata":{}
}
```

`GET`ting from `/recurring/RECURRING_ID.json` will return that specific recurring.

If you have connected Quaderno and Stripe, you can also `GET /stripe/charges/STRIPE_CHARGE_ID.json` to get the Quaderno recurring for a Stripe charge.

## Update a recurring

> `PUT /recurring/RECURRING_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X PUT \
     -d '{"notes":"You better pay this time, Tony."}' \
     'https://ACCOUNT_NAME.quadernoapp.com/api/recurring/RECURRING_ID.json'
```

```ruby
params = {
    notes: 'You better pay this time, Tony.'
}
Quaderno::Recurring.update(RECURRING_ID, params) #=> Quaderno::Recurring
```

```php?start_inline=1
$recurring->notes = 'You better pay this time, Tony.';
$recurring->save();
```

````swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let params : [String: Any] = [
    "notes": "You better pay this time, Tony."
]

let updateRecurring = Recurring.update(RECURRING_ID, params)
client.request(updateRecurring) { response in
    // response will contain the result of the request.
}
```

`PUT`ting to `/recurring/RECURRING_ID.json` will update the recurring with the passed parameters.

This will return `200 OK` along with the current JSON representation of the recurring if successful.

## Delete a recurring

> `DELETE /recurring/RECURRING_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X DELETE 'https://ACCOUNT_NAME.quadernoapp.com/api/recurring/RECURRING_ID.json'
```

```ruby
Quaderno::Recurring.delete(RECURRING_ID) #=> Boolean
```

```php?start_inline=1
$recurring->delete();
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let deleteRecurring = Recurring.delete(RECURRING_ID)
client.request(deleteRecurring) { response in
    // response will contain the result of the request.
}
```

`DELETE`ing to `/recurring/RECURRING_ID.json` will delete the specified recurring and returns `204 No Content` if successful.
