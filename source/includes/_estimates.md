# Estimates

An estimate is an offer that you give a client in order to get a specific job. With time, estimates are usually turned into issued invoices.

## Create an estimate

> `POST /estimates.json`

```shell
# body.json
{
  "number":"0000006",
  "contact_id":"50603e722f412e0435000024",
  "contact_name":"Wild E. Coyote",
  "po_number":"",
  "currency":"EUR",
  "items_attributes":[
    {
        "description":"ACME Catapult",
        "quantity":"1.0",
        "unit_price":"0.0",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "reference":"item_code_X"
      }
  ],
  "tag_list":"tnt",
  "payment_details":"",
  "notes":""
}

curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     --data-binary @- body.json \
     'https://ACCOUNT_NAME.quadernoapp.com/api/estimates.json'
```

```php?start_inline=1
$estimate = new QuadernoEstimate(array(
                                 'po_number' => '',
                                 'currency' => 'USD',
                                 'tag_list' => array('playboy', 'businessman')));
$item = new QuadernoDocumentItem(array(
                               'description' => 'ACME Catapult',
                               'unit_price' => 0.0,
                               'reference' => 'ITEM_ID',
                               'quantity' => 1.0));
$contact = QuadernoContact::find('5059bdbf2f412e0901000024');

$estimate->addItem($item);
$estimate->addContact($contact);

$estimate->save(); // Returns true (success) or false (error)
```

```ruby
contact = Quaderno::Contact.find('50603e722f412e0435000024') #=> Quaderno::Contact

params = {
  contact_id: contact.id,
  number: '0000006',
  po_number: '',
  currency: 'EUR',
  tag_list: 'tnt',
  payment_details: '',
  notes: '',
  items_attributes: [
    {
      description: 'Whiskey',
      quantity: '1.0',
      unit_price: '20.0',
      discount_rate: '0.0'
    }

  ]
}
estimate = Quaderno::Estimate.create(params) #=> Quaderno::Estimate
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let params : [String: Any] = [
number":"0000006",
    "contact_id":"50603e722f412e0435000024",
    "contact_name":"Wild E. Coyote",
    "po_number":"",
    "currency":"EUR",
    "tag_list": ["playboy", "businessman"]
]

let createEstimate = Estimate.create(params)
client.request(createEstimate) { response in
    // response will contain the result of the request.
}
```

`POST`ing to `/estimates.json` will create a new estimate from the parameters passed.

This will return `201 Created` and the current JSON representation of the estimate if the creation was a success, along with the location of the new estimate in the `url` field.

### Attributes

Attribute       | Mandatory                                  | Type/Description
----------------|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------
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
street_line_1   | no                                         | String(255 chars). Available for updates
street_line_2   | no                                         | String(255 chars)
city            | no                                         | String(255 chars). Available for updates
region          | no                                         | String(255 chars). Available for updates
postal_code     | no                                         | String(255 chars). Available for updates
items_attributes| **yes**                                    | Array of document items (check available attributes for document items below). No more than 200 items are allowed in a request. To add more use subsequent update requests

<aside class="notice">
If you pass a `contact` JSON object instead of a `contact_id`, and the first and last name combination does not match any of your existing estimates, a new one will be created, otherwise a new estimate will be created for the existing estimate.<br /><br />

<p>Please note that you can pass <strong>either</strong> estimate_id or estimate, but if you pass both then results may not be what you expect.</p>

<p>Allowed contact fields are the same as when creating a contact via the dedicated <a href="#estimates">Contacts API</a>.</p>
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

### Estimate States

Possible estimate states are:

- `outstanding`
- `accepted`
- `declined`
- `invoiced`
- `late`

### Create an attachment during estimate creation

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

Valid file extensions are `pdf`, `txt`, `jpeg`, `jpg`, `png`, `xml`, `xls`, `doc`, `rtf` and `html`. Any other format will stop estimate creation and return a `422 Unprocessable Entity` error.

## Retrieve: Get and filter all estimates

> `GET /estimates.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/estimates.json'
```

```ruby
Quaderno::Estimate.all() #=> Array
```

```php?start_inline=1
$estimates = QuadernoEstimate::find(); // Returns an array of QuadernoEstimate
```

```swift
let client = Quaderno.Client(/* ... */)

let listEstimates = Estimate.list(pageNum)
client.request(listEstimates) { response in
  // response will contain the result of the request.
}
```

```json
[
  {
    "id":"50603e722f412e0435000024",
    "number":"0000003",
    "issue_date":"2012-09-24",
    "contact":{
      "id":"5059bdbf2f412e0901000024",
      "full_name":"Wild E. Coyote"
    },
    "street_line_1":"Desert of New Mexico",
    "street_line_2":"",
    "city":"New Mexico",
    "region":"New Mexico",
    "postal_code":"",
    "po_number":"",
    "currency":"EUR",
    "items":[
      {
        "id":"4815162342",
        "description":"ACME TNT",
        "quantity":"1.0",
        "unit_price_cents":"10000",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "subtotal_cents":"10000",
        "discount_cents":"0",
        "gross_amount_cents":"10000"
       }
    ],
    "subtotal_cents":"10000",
    "discount_cents":"0",
    "taxes":[],
    "total_cents":"10000",
    "payment_details":"",
    "notes":"",
    "state":"outstanding",
    "tag_list":[],
    "permalink":"https://my-account.quadernoapp.com/estimate/7hef1rs7p3rm4l1nk",
    "url":"https://my-account.quadernoapp.com/api/estimates/50603e722f412e0435000024.json"
  },
  {
    "id":"50603e722f412e0435000144",
    "number":"0000005",
    "issue_date":"2012-09-24",
    "contact":{
      "id":"5059bdbf2f412e0901000044",
      "full_name":"Cookie Monster"
    },
    "street_line_1":"Sesame Street",
    "street_line_2":"",
    "city":"New York",
    "region":"New York",
    "postal_code":"",
    "po_number":"",
    "currency":"EUR",
    "items":[
      {
        "id":"48151623421",
        "description":"Cookies",
        "quantity":"5.0",
        "unit_price_cents":"195",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "subtotal_cents":"975",
        "discount_cents":"0",
        "gross_amount_cents":"975"
       }
    ],
    "subtotal_cents":"975",
    "discount_cents":"0",
    "taxes":[],
    "total_cents":"975",
    "payment_details":"",
    "notes":"",
    "state":"outstanding",
    "tag_list":[],
    "permalink":"https://my-account.quadernoapp.com/estimate/7hes3c0ndp3rm4l1nk",
    "url":"https://my-account.quadernoapp.com/api/estimates/50603e722f412e0435000144.json"
  }
]
```

`GET`ting from `/estimates.json` will return all the user's estimates.

You can filter the results in a few ways:

- By `number`, `contact_name` or `po_number` by passing the `q` parameter in the url like `?q=KEYWORD`.
- By date range, passing the `date` parameter in the url like `?date=DATE1,DATE2`.
- By state, passing the `state` parameter like `?state=STATE`.
- By contact, passing the contact ID in the `contact` parameter, like `?contact=3231`.

## Retrieve: Get a single estimate

> `GET /estimates/ESTIMATE_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/estimates/ESTIMATE_ID.json'
```

```ruby
Quaderno::Estimate.find(ESTIMATE_ID) #=> Quaderno::Estimate
```

```php?start_inline=1
$estimate = QuadernoEstimate::find('ESTIMATE_ID'); // Returns a QuadernoEstimate
```

```swift
let client = Quaderno.Client(/* ... */)

let readEstimate = Estimate.read(ESTIMATE_ID)
client.request(readEstimate) { response in
  // response will contain the result of the request.
}
```

```json
{
  "id":"50603e722f412e0435000024",
  "number":"0000003",
  "issue_date":"2012-09-24",
  "contact":{
    "id":"5059bdbf2f412e0901000024",
    "full_name":"Wild E. Coyote"
  },
  "street_line_1":"Desert of New Mexico",
  "street_line_2":"",
  "city":"New Mexico",
  "region":"New Mexico",
  "postal_code":"",
  "po_number":"",
  "currency":"EUR",
  "items":[
    {
      "id":"4815162342"
      "description":"ACME TNT",
      "quantity":"1.0",
      "unit_price_cents":"10000",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "subtotal_cents":"10000",
      "discount_cents":"0",
      "gross_amount_cents":"10000"
    }
  ],
  "subtotal_cents":"10000",
  "discount_cents":"0",
  "taxes":[],
  "total":"10000",
  "payment_details":"",
  "notes":"",
  "state":"outstanding",
  "tag_list":[],
  "permalink":"https://my-account.quadernoapp.com/estimate/7hef1rs7p3rm4l1nk",
  "url":"https://my-account.quadernoapp.com/api/estimates/50603e722f412e0435000024.json"
}
```

`GET`ting from `/estimates/ESTIMATE_ID.json` will return that specific estimate.

## Update an estimate

> `PUT /estimates/ESTIMATE_ID.json`

```shell
# body.json
{
  "tag_list":"Wacky, racer"
  "contact_id":"505c3b402f412e0248000044",
  "contact_name":"Dick Dastardly",
}

curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X PUT \
     --data-binary @body.json \
     'https://ACCOUNT_NAME.quadernoapp.com/api/estimates/ESTIMATE_ID.json'
```

```ruby
params = {
    contact_id: '505c3b402f412e0248000044',
    contact_name: 'Dick Dastardly'
}
Quaderno::Estimate.update(ESTIMATE_ID, params) #=> Quaderno::Estimate
```

```php?start_inline=1
$estimate->contact_id = '505c3b402f412e0248000044';
$estimate->contact_name = 'Dick Dastardly';
$estimate->save();
```

````swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let params : [String: Any] = [
    "contact_name": "Dick Dastardly",
    "contact_id": "505c3b402f412e0248000044"
]

let updateEstimate = Estimate.update(ESTIMATE_ID, params)
client.request(updateEstimate) { response in
    // response will contain the result of the request.
}
```

`PUT`ting to `/estimates/ESTIMATE_ID.json` will update the estimate with the passed parameters.

This will return `200 OK` along with the current JSON representation of the estimate if successful.

## Delete an estimate

> `DELETE /estimates/ESTIMATE_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X DELETE 'https://ACCOUNT_NAME.quadernoapp.com/api/estimates/ESTIMATE_ID.json'
```

```ruby
Quaderno::Estimate.delete(ESTIMATE_ID) #=> Boolean
```

```php?start_inline=1
$estimate->delete();
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let deleteEstimate = Estimate.delete(ESTIMATE_ID)
client.request(deleteEstimate) { response in
    // response will contain the result of the request.
}
```

`DELETE`ing to `/estimate/ESTIMATE_ID.json` will delete the specified estimate and returns `204 No Content` if successful.

## Deliver (Send) an estimate

```shell
curl -u YOUR_API_KEY:
     -X GET
     'https://ACCOUNT_NAME.quadernoapp.com/api/estimates/ESTIMATE_ID/deliver.json'
```

```ruby
estimate = Quaderno::Estimate.find(ESTIMATE_ID)
estimate.deliver
```

```php?start_inline=1
$estimate->deliver(); // Return true (success) or false (error)
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let deliverEstimate = Estimate.deliver(ESTIMATE_ID)
client.request(deliverEstimate) { response in
  // response will contain the result of the request.
}
```

`GET`ting `/estimates/ESTIMATE_ID/deliver.json` will send the estimate to the assigned contact email. This will return `200 OK` if successful, along with a JSON representation of the estimate.

If the destination contact does not have an email address you will receive a `422 Unprocessable Entity` error.
