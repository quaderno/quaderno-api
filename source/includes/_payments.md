# Payments

Payments in Quaderno-lingo represent the recording of a successful payment.

Like [items](#items), payments are a sub-object and can be attached to multiple types of other records:

- Invoices and sales receipts
- Credits (a.k.a credit notes)
- Expenses

<aside class="notice">
Note that unlike items, payments cannot exist in Quaderno separately to one of these objects, and the same payment cannot be referenced by more than one instance of any of them.
</aside>

## Create an item

> `POST /invoices/INVOICE_ID/payments.json` or `POST /expenses/EXPENSE_ID/payments.json` or `POST /credits/CREDIT_ID/payments.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     -d '{ \
          "amount":"56.60", \
          "payment_method":"credit_card" \
        }' \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/credits/1/payments.json'
```

```php?start_inline=1
$payment = new QuadernoPayment(array(
                                 'name' => 'Jelly pizza',
                                 'code' => 'Yummy',
                                 'unit_cost' => '15.00',
                                 'tax_1_name' => 'JUNKTAX',
                                 'tax_1_rate' => '99.99'));
$payment->save(); // Returns true (success) or false (error)
```

```ruby
params = {
    name: 'Jelly pizza',
    code: 'Yummy',
    unit_cost: '15.00',
    tax_1_name: 'JUNKTAX',
    tax_1_rate: '99.99'
}
Quaderno::Item.create(params) #=> Quaderno::Item
```

```swift?start_inline=1
TODO!
```

`POST`ing to `/payments.json` will create a new item from the parameters passed.

This will return `201 Created` and the current JSON representation of the item if the creation was a success, along with the location of the new item in the `url` field.

### Mandatory Fields

Key          | Description
-------------|------------------------------------------------------------------------------------------
`name`       | Name of the item.
`unit_cost` | Cost of the item.
`code` | Mandatory *only* if you want to have stock tracking by defining the value `stock`.

<aside class="notice">
Note that this kind of item is like a template for items, a shortcut that you can reference in order to speed up document creation. You can also have items that exist only for the relevant document and not as independent entities, and these do not require "unit_cost" (also not being stock-trackable and so on).
</aside>

## Retrieve: Get and filter all items

> `GET /payments.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/payments.json'
```

```ruby
Quaderno::Item.all() #=> Array
```

```php?start_inline=1
$payments = QuadernoPayment::find(); // Returns an array of QuadernoPayment
```

```swift
let client = Quaderno.Client(/* ... */)

let readItem = Item.read()
client.request(readItem) { response in
  // response will contain the result of the request.
}
```

```json
[
  {
    "id":1,
    "code":"BluRay 0000",
    "name":"Titanic",
    "unit_cost":"15.0",
    "stock":"100",
    "url":"https://my-account.quadernoapp.com/api/v1/items/1"
  },
  {
    "id":2,
    "code":"BluRay 0001",
    "name":"Titanic II: The revenge",
    "unit_cost":"15.0",
    "tax_1_name":"AWESOME_TAX",
    "tax_1_rate":"7.00",
    "url":"https://my-account.quadernoapp.com/api/v1/items/2"
  },
  {
    "id":3,
    "code":"BluRay 0002",
    "name":"Titanic III: The origin",
    "unit_cost":"15.0",
    "stock":"33",
    "url":"https://my-account.quadernoapp.com/api/v1/items/3"
  }
]
```

`GET`ting from `/payments.json` will return all the user's payments.

You can filter the results in a few ways:

- By `number`, `contact_name` or `po_number` by passing the `q` parameter in the url like `?q=KEYWORD`.
- By date range, passing the `date` parameter in the url like `?date=DATE1,DATE2`.
- By state, passing the `state` parameter like `?state=STATE`.
- By contact, passing the contact ID in the `contact` parameter, like `?contact=3231`.
- By `exchange_rate`, if the item currency differs from your account currency.

## Retrieve: Get a single item

> `GET /items/ITEM_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/items/ITEM_ID.json'
```

```ruby
Quaderno::Item.find(ITEM_ID) #=> Quaderno::Item
```

```php?start_inline=1
$payment = QuadernoPayment::find('ITEM_ID'); // Returns a QuadernoPayment
```

```swift
let client = Quaderno.Client(/* ... */)

let readItem = Item.read()
client.request(readItem) { response in
  // response will contain the result of the request.
}
```

```json
{
  "id":2,
  "code":"BluRay 0001",
  "name":"Titanic II: The revenge",
  "unit_cost":"15.0",
  "tax_1_name":"AWESOME_TAX",
  "tax_1_rate":"7.00",
  "url":"https://my-account.quadernoapp.com/api/v1/items/2"
}
```

`GET`ting from `/items/ITEM_ID.json` will return that specific item.

## Update an item

> `PUT /items/ITEM_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X PUT \
     -d '{ "unit_cost":"10.0" }' \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/items/ITEM_ID.json'
```

```ruby
params = {
    unit_cost: '10.0',
}
Quaderno::Item.update(ITEM_ID, params) #=> Quaderno::Item
```

```php?start_inline=1
$payment->unit_cost = '10.0';
$payment->save();
```

```swift?start_inline=1
// TODO
```

`PUT`ting to `/items/ITEM_ID.json` will update the item with the passed parameters.

This will return `200 OK` along with the current JSON representation of the item if successful.

## Delete an item

> `DELETE /items/ITEM_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X DELETE 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/items/ITEM_ID.json'
```

```ruby
Quaderno::Item.delete(ITEM_ID) #=> Boolean
```

```php?start_inline=1
$payment->delete();
```

```swift?start_inline=1
// TODO
```

`DELETE`ing to `/item/ITEM_ID.json` will delete the specified item and returns `204 No Content` if successful.

## Adding an item to a document by reference

> Given this item:

```json
{
  "code":"BluRay 0003",
  "name":"Titanic IV: The revenge",
  "unit_cost":"15.0",
  "tax_1_name":"AWESOME_TAX",
  "tax_1_rate":"7.00",
  "tax_2_name":"ANOTHER_AWESOME_TAX",
  "tax_2_rate":"10.00",
  "stock":"1000"
}
```

> You can generate an invoice with this item by passing this data via POST to /invoices.json:

```json
{
  "contact_id":"5059bdbf2f412e0901000024",
  "contact_name":"STARK",
  "currency":"USD",
  "tag_list":"playboy, businessman",
  "items_attributes":[
    {
      "reference":"BluRay 0003",
      "quantity":"2",
      "unit_cost":"10.95"
    }
  ],
}
```

You can use an item attributes to create or update a document (invoice, expense or estimate). In order to use this feature, the item must have a `code` that must be set as a `reference` in the `items_attributes` array.

Then you can use the `code` in the `reference` field along with per-use attributes such as `quantity` when setting the `item_attributes` of another document, such as an invoice or expense.

<aside class="notice">
You can see this happening in API examples under the relevant document sections.
</aside>
