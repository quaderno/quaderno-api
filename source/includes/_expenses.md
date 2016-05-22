# Expenses

Expenses are all the invoices that you receive from your vendors.

## Create an expense

> `POST /expenses.json`

```shell
# body.json
{     
  "contact_id":"5059bdbf2f412e0901000024",
  "contact_name":"ACME",
  "currency":"USD",
  "items_attributes":[
    {
    "description":"Rocket launcher",
    "quantity":"1.0",
    "unit_price":"0.0",
    "discount_rate":"0.0",
    "reference":"item_code_X"
    }
  ],
}

curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     --data-binary @body.json \     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/expenses.json'
```

```php?start_inline=1
$expense = new QuadernoExpense(array(
                                 'po_number' => '',
                                 'currency' => 'USD',
                                 'tag_list' => array('playboy', 'businessman')));
$item = new QuadernoDocumentItem(array(
                               'description' => 'Rocket launcher',
                               'unit_price' => 0.0,
                               'discount_rate' => 0.0,
                               'reference' => "ITEM_ID",
                               'quantity' => 1.0));
$contact = QuadernoContact::find('5059bdbf2f412e0901000024');

$expense->addItem($item);
$expense->addContact($contact);

$expense->save(); // Returns true (success) or false (error)
```

```ruby
params = {
    currency: 'USD',
    tag_list: ['playboy', 'businessman']
}
invoice = Quaderno::Expense.create(params) #=> Quaderno::Expense
item_params = {
    description: 'Rocket launcher',
    quantity: '1.0',
    unit_price: '0.0',
    discount_rate: '0.0',
    reference: 'ITEM_ID'
}
item = Quaderno::Item.create(item_params) #=> Quaderno::Item
contact = Quaderno::Contact.find('5059bdbf2f412e0901000024') #=> Quaderno::Contact
expense.add_item(item)
expense.add_contact(contact)
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let params : [String: Any] = [
    "contact_id":"5059bdbf2f412e0901000024",
    "contact_name":"ACME",
    "currency":"USD"
]

// TODO: Items!

let createExpense = Expense.create(params)
client.request(createExpense) { response in
    // response will contain the result of the request.
}
```

`POST`ing to `/expenses.json` will create a new contact from the parameters passed.

This will return `201 Created` and the current JSON representation of the expense if the creation was a success, along with the location of the new expense in the `url` field.

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

<aside class="notice">
If you pass a `contact` JSON object instead of a `contact_id`, and the first and last name combination does not match any of your existing contacts, a new one will be created, otherwise a new expense will be created for the existing contact.<br /><br />

<p>Please note that you can pass <strong>either</strong> contact_id or contact, but if you pass both then results may not be what you expect.</p>

<p>Allowed contact fields are the same as when creating a contact via the dedicated <a href="#contacts">Contacts API</a>.</p>
</aside>

### Expense States

Possible expense states are:

- `outstanding`
- `paid`
- `late`
- `archived`

### Create an attachment during expense creation

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

Valid file extensions are `pdf`, `txt`, `jpeg`, `jpg`, `png`, `xml`, `xls`, `doc`, `rtf` and `html`. Any other format will stop expense creation and return a `422 Unprocessable Entity` error.

## Retrieve: Get and filter all expenses

> `GET /expenses.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/expenses.json'
```

```ruby
Quaderno::Expense.all() #=> Array
```

```php?start_inline=1
$expenses = QuadernoExpense::find(); // Returns an array of QuadernoExpense
```

```swift
let client = Quaderno.Client(/* ... */)

let listExpenses = Expense.list(pageNum)
client.request(listExpenses) { response in
  // response will contain the result of the request.
}
```

```json
[
  {
    "id":"5076a6b92f412e0e2e00006c",
    "issue_date":"2012-10-11",
    "contact":{
      "id":"5059bdbf2f412e0901000024",
      "full_name":"ACME"
    },
    "street_line_1":"Verizon Center",
    "street_line_2":"",
    "city":"Washington DC",
    "region":"Washington DC",
    "postal_code":"",
    "po_number":"",
    "currency":"USD",
    "exchange_rate":"0.680309",
    "items":[
      {
        "id":"48151623423",
        "description":"Rockets",
        "quantity":"50.0",
        "unit_price":"125.0",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "subtotal":"$6,250.00",
        "discount":"$0.00",
        "gross_amount":"$6,250.00"
      }
    ],
    "subtotal":"$6,250.00",
    "discount":"$0.00",
    "taxes":[],
    "total":"$6,250.00",
    "payments":[],
    "tag_list":["rockets", "acme"],
    "notes":"",
    "state":"outstanding",
    "url":"https://my-account.quadernoapp.com/api/v1/expenses/5076a6b92f412e0e2e00006c"
  },
  {
    "id":"5076a6b92f412e0e2e00016d",
    "issue_date":"2012-10-11",
    "contact":{
    "id":"5059bdbf2f412e0901000024",
    "full_name":"ACME"
    },
    "street_line_1":"Verizon Center",
    "street_line_2":"",
    "city":"Washington DC",
    "region":"Washington DC",
    "postal_code":"",
    "po_number":"",
    "currency":"USD",
    "exchange_rate":"0.680309",
    "items":[
      {
      "id":" 48151623424",
      "description":"TNT",
      "quantity":"100.0",
      "unit_price":"25.0",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "subtotal":"$2,500.00",
      "discount":"$0.00",
      "gross_amount":"$2,500.00"
    }
    ],
    "subtotal":"$2,500.00",
    "discount":"$0.00",
    "taxes":[],
    "total":"$2,500.00",
    "payments":[],
    "tag_list":["tnt", "acme"],
    "notes":"",
    "state":"outstanding",
    "url":"https://my-account.quadernoapp.com/api/v1/expenses/5076a6b92f412e0e2e00016d"
  },
]
```

`GET`ting from `/expenses.json` will return all the user's expenses.

You can filter the results in a few ways:

- By `contact_name` or `po_number` by passing the `q` parameter in the url like `?q=KEYWORD`.
- By date range, passing the `date` parameter in the url like `?date=DATE1,DATE2`.
- By state, passing the `state` parameter like `?state=STATE`.
- By specific vendor, passing the vendor ID in the `contact` parameter, like `?contact=3231`.

## Retrieve: Get a single expense

> `GET /expenses/EXPENSE_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/expenses/EXPENSE_ID.json'
```

```ruby
expense = Quaderno::Expense.find(EXPENSE_ID) #=> Quaderno::Expense
```

```php?start_inline=1
$expense = QuadernoExpense::find('EXPENSE_ID'); // Returns a QuadernoExpense
```

```swift
let client = Quaderno.Client(/* ... */)

let readExpense = Expense.read(EXPENSE_ID)
client.request(readExpense) { response in
  // response will contain the result of the request.
}
```

```json
{
  "id":"5076a6b92f412e0e2e00006c",
  "issue_date":"2012-10-11",
  "contact":{
    "id":"5059bdbf2f412e0901000024",
    "full_name":"ACME"
  },
  "street_line_1":"Verizon Center",
  "street_line_2":"",
  "city":"Washington DC",
  "region":"Washington DC",
  "postal_code":"",
  "po_number":"",
  "currency":"USD",
  "items":[
    {
      "id":"48151623423",
      "description":"Rockets",
      "quantity":"50.0",
      "unit_price":"125.0",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "subtotal":"$6,250.00",
      "discount":"$0.00",
      "gross_amount":"$6,250.00"
    }
  ],
  "subtotal":"$6,250.00",
  "discount":"$0.00",
  "taxes":[],
  "total":"$6,250.00",
  "payments":[],
  "tag_list":["rockets", "acme"],
  "notes":"",
  "state":"outstanding",
  "url":"https://my-account.quadernoapp.com/api/v1/expenses/5076a6b92f412e0e2e00006c"
}
```

`GET`ting from `/expenses/EXPENSE_ID.json` will return that specific expense.

## Update an expense

> `PUT /expenses/EXPENSE_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X PUT \
     -d '{ "payment_details":"Money in da bank" }'  \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/expenses/EXPENSE_ID.json'
```

```ruby
params = {
    payment_details: 'Money in da bank'
}
Quaderno::Expense.update(EXPENSE_ID, params) #=> Quaderno::Expense
```

```php?start_inline=1
$expense->payment_details = 'Money in da bank';
$expense->save();
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let params : [String: Any] = [
    "payment_details": "Money in da bank"
]

let updateExpense = Expense.update(EXPENSE_ID, params)
client.request(updateExpense) { response in
    // response will contain the result of the request.
}
```

`PUT`ting to `/expenses/EXPENSE_ID.json` will update the expense with the passed parameters.

This will return `200 OK` along with the current JSON representation of the expense if successful.

### Save an invoice as an expense

```json
{
  "permalink":"c2d480801d0e577c6c3177ce0795c4bdaa480e22"
}
```

`GET`ting `/expenses/save.json` will save another account invoice as an expense using the permalink passed as a parameter.

This will return `201 Created`, with the current JSON representation of the expense if the creation was a success, along the location of the new expense in the `url` field.

If the user does not have access to create new expenses, you'll see `401 Unauthorized` or a `422` if the permalink is invalid or the invoice is not suitable to be saved.

## Delete an expense

> `DELETE /expenses/EXPENSE_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X DELETE 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/expenses/EXPENSE_ID.json'
```

```ruby
Quaderno::Expense.delete(EXPENSE_ID) #=> Boolean
```

```php?start_inline=1
$expense->delete();
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let deleteExpense = Expense.delete(EXPENSE_ID)
client.request(deleteExpense) { response in
    // response will contain the result of the request.
}
```

`DELETE`ing to `/expense/EXPENSE_ID.json` will delete the specified contact and returns `204 No Content` if successful.
