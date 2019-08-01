# Contacts

A contact is any client, customer or vendor who appears on your invoices or expenses.

## Create a contact

> `POST /contacts.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     -d '{"first_name":"Tony", "kind":"person", "contact_name":"Stark"}' \
     'https://ACCOUNT_NAME.quadernoapp.com/api/contacts.json'
```

```php?start_inline=1
$contact = new QuadernoContact(array(
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
Quaderno::Contact.create(params) #=> Quaderno::Contact
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let params : [String: Any] = [
    "first_name": "Tony",
    "kind": "person",
    "contact_name": "Stark"
]

let createContact = Contact.create(params)
client.request(createContact) { response in
    // response will contain the result of the request.
}
```

`POST`ing to `/contacts.json` will create a new contact from the parameters passed.

This will return `201 Created` and the current JSON representation of the contact if the creation was a success, along with the location of the new contact in the `url` field.

### Mandatory Fields

Key          | Description
-------------|------------------------------------------------------------------------------------------
`first_name` | The first name of the contact.
`bic`        | *If sending a bank_account (in electronic IBAN format)*. Must be 11 characters in length.

### Attributes

Attribute               | Mandatory | Type/Description
------------------------|-----------|----------------------------------------------------------------------------
kind                    | no        | `company` or `person`. Defaults to `company`.
first_name              | **yes**       | String(255 chars)
last_name               | no        | String(255 chars)
contact_name            | no        | String(255 chars)
street_line_1           | no        | String(255 chars)
street_line_2           | no        | String(255 chars)
city                    | no        | String(255 chars)
postal_code             | no        | String(255 chars)
region                  | no        | String(255 chars)
country                 | no        | String(2 chars) `ISO 3166-1 alpha-2`
secondary_street_line_1 | no        | String(255 chars)
secondary_street_line_2 | no        | String(255 chars)
secondary_postal_code   | no        | String(255 chars)
secondary_city          | no        | String(255 chars)
secondary_region        | no        | String(255 chars)
secondary_country       | no        | String(255 chars)
phone_1                 | no        | String(255 chars)
phone_2                 | no        | String(255 chars)
fax                     | no        | String(255 chars)
email                   | no        | String(255 chars) Multiple emails should be separated by commas. Maximum number of addresses allowed is 3.
web                     | no        | String(255 chars). Validates format
discount                | no        | Decimal
language                | no        | String(2 chars) Should be included in the translations list
tax_id                  | no        | String(255 chars)
vat_number              | no        | String(255 chars) If present, it is validated against VIES
bank_account            | no        | String(255 chars) format is validated
bic                     | **yes**/no        | String(255 chars) **Mandatory if bank_account is present.** Format is validated
notes                   | no        | Text

## Retrieve: Get and filter all contacts

> `GET /contacts.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/contacts.json'
```

```ruby
Quaderno::Contact.all() #=> Array
```

```php?start_inline=1
$contacts = QuadernoContact::find(); // Returns an array of QuadernoContact
```

```swift
let client = Quaderno.Client(/* ... */)

let listContacts = Contact.list(pageNum)
client.request(listContacts) { response in
  // response will contain the result of the request.
}
```

```json
[
  {
    "id":"456987213",
    "kind":"person",
    "first_name":"Sheldon",
    "last_name":"Cooper",
    "full_name":"Sheldon Cooper",
    "street_line_1":"2311 N. Los Robles Avenue",
    "street_line_2":"",
    "postal_code":"91104",
    "city":"Pasadena",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "phone_2":"",
    "fax":"",
    "email":"s.cooperphd@yahoo.com",
    "web":"",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "secure_id":"th3p3rm4l1nk",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/billing/th3p3rm4l1nk",
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/contacts/456987213"
  },
  {
    "id":"456982365",
    "kind":"company",
    "full_name":"Apple Inc.",
    "street_line_1":"1 Infinite Loop",
    "street_line_2":"",
    "postal_code":"95014",
    "city":"Cupertino",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "phone_2":"",
    "fax":"",
    "email":"info@apple.com",
    "web":"http://apple.com",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "secure_id":"4n0th3rp3rm4l1nk",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/billing/4n0th3rp3rm4l1nk",
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/contacts/456982365"
  }
]
```

`GET`ting from `/contacts.json` will return all the user's contacts.

You can filter the results by full name, email or tax ID by passing the `q` parameter in the URL as a query string, like `?q=KEYWORD`.

## Retrieve: Get a single contact

> `GET /contacts/CONTACT_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/contacts/CONTACT_ID.json'
```

```ruby
Quaderno::Contact.find(CONTACT_ID) #=> Quaderno::Contact
```

```php?start_inline=1
$contact = QuadernoContact::find(CONTACT_ID); // Returns a QuadernoContact
```

```swift
let client = Quaderno.Client(/* ... */)

let readContact = Contact.read(CONTACT_ID)
client.request(readContact) { response in
  // response will contain the result of the request.
}
```

```json
{
    "id":"456987213",
    "kind":"person",
    "first_name":"Sheldon",
    "last_name":"Cooper",
    "full_name":"Sheldon Cooper",
    "street_line_1":"2311 N. Los Robles Avenue",
    "street_line_2":"",
    "postal_code":"91104",
    "city":"Pasadena",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "phone_2":"",
    "fax":"",
    "email":"s.cooperphd@yahoo.com",
    "web":"",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "secure_id":"th3p3rm4l1nk",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/billing/th3p3rm4l1nk",
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/contacts/456987213"
}
```

`GET`ting from `/contacts/CONTACT_ID.json` will get you that specific contact.

<aside class="notice">
If you've connected Quaderno and Stripe, you can also `GET /stripe/customers/STRIPE_CUSTOMER_ID.json` to get the Quaderno contact for a Stripe customer.
</aside>

## Retrieve: Get a single contact by payment gateway ID

> `GET /PAYMENT_GATEWAY/customers/PAYMENT_GATEWAY_CUSTOMER_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/PAYMENT_GATEWAY/customers/PAYMENT_GATEWAY_CUSTOMER_ID.json'
```

```ruby
Quaderno::Contact.retrieve_customer(PAYMENT_GATEWAY_CUSTOMER_ID, PAYMENT_GATEWAY) #=> Quaderno::Contact
```

```php?start_inline=1
```

```swift
```

```json
{
    "id":"456987213",
    "kind":"person",
    "first_name":"Sheldon",
    "last_name":"Cooper",
    "full_name":"Sheldon Cooper",
    "street_line_1":"2311 N. Los Robles Avenue",
    "street_line_2":"",
    "postal_code":"91104",
    "city":"Pasadena",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "phone_2":"",
    "fax":"",
    "email":"s.cooperphd@yahoo.com",
    "web":"",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "secure_id":"th3p3rm4l1nk",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/billing/th3p3rm4l1nk",
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/contacts/456987213"
}
```

`GET`ting from `/PAYMENT_GATEWAY/customers/PAYMENT_GATEWAY_CUSTOMER_ID.json` will get you that specific contact by using their ID with the payment gateway they were created with.

Supported gateways:

- [Stripe](https://stripe.com/)
- [GoCardless](https://gocardless.com/)
- [PayPal](https://www.paypal.com)
- [Braintree](https://www.braintreepayments.com/)

More are added frequently, so check back!

## Update a contact

> `PUT /contacts/CONTACT_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X PUT \
     -d '{"first_name":"Anthony"}' \
     'https://ACCOUNT_NAME.quadernoapp.com/api/contacts/CONTACT_ID.json'
```

```ruby
Quaderno::Contact.update(CONTACT_ID, params) #=> Quaderno::Contact
```

```php?start_inline=1
$contact->first_name = 'Anthony';
$contact->save();
```

````swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let params : [String: Any] = [
    "first_name": "Anthony"
]

let updateContact = Contact.update(CONTACT_ID, params)
client.request(updateContact) { response in
    // response will contain the result of the request.
}
```

`PUT`ing to `/contacts/CONTACT_ID.json` will update the contact from the passed parameters.

This will return `200 OK` and a JSON representation of the contact if successful.

## Delete a contact

> `DELETE /contacts/CONTACT_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X DELETE
     'https://ACCOUNT_NAME.quadernoapp.com/api/contacts/CONTACT_ID.json'
```

```ruby
Quaderno::Contact.delete(CONTACT_ID) #=> Boolean
```

```php?start_inline=1
$contact->delete();
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let deleteContact = Contact.delete(CONTACT_ID)
client.request(deleteContact) { response in
    // response will contain the result of the request.
}
```

`DELETE`ing to `/contacts/CONTACT_ID.json` will delete the specified contact and returns `204 No Content` if successful.
