# Contacts

A contact is any client, customer or vendor who appears on your invoices or expenses.

## Create a contact

> `POST /contacts.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     -d '{"first_name":"Tony", "kind":"person", "contact_name":"Stark"}' \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/contacts.json'
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
`kind`       | Indicates if the contact is a `person` or a `company`.
`first_name` | The first name of the contact.
`bic`        | *If sending a bank_account (in electronic IBAN format)*. Must be 11 characters in length.

## Retrieve: Get and filter all contacts

> `GET /contacts.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/contacts.json'
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
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/v1/contacts/456987213"
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
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/v1/contacts/456982365"
  }
]
```

`GET`ting from `/contacts.json` will return all the user's contacts.

You can filter the results by full name, email or tax ID by passing the `q` parameter in the URL as a query string, like `?q=KEYWORD`.

## Retrieve: Get a single contact

> `GET /contacts/CONTACT_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/contacts/CONTACT_ID.json'
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
    "url":"https://ACCOUNT_NAME.quadernoapp.com/api/v1/contacts/456987213"
}
```

`GET`ting from `/contacts/CONTACT_ID.json` will get you that specific contact.

<aside class="notice">
If you've connected Quaderno and Stripe, you can also `GET /stripe/customers/STRIPE_CUSTOMER_ID.json` to get the Quaderno contact for a Stripe customer.
</aside>

## Update a contact

> `PUT /contacts/CONTACT_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X PUT \
     -d '{"first_name":"Anthony"}' \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/contacts/CONTACT_ID.json'
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
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/contacts/CONTACT_ID.json'
```

```ruby
Quaderno::Contact.delete(CONTACT_ID) #=> Boolean
```

```php?start_inline=1
$contact->delete();
```

````swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let deleteContact = Contact.delete(CONTACT_ID)
client.request(deleteContact) { response in
    // response will contain the result of the request.
}
```

`DELETE`ing to `/contacts/CONTACT_ID.json` will delete the specified contact and returns `204 No Content` if successful.
