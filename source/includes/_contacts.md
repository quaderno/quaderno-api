# Contacts

A contact is any client, customer or vendor who appears on your invoices or expenses.

## Create

> `POST /contacts.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     -d {"first_name":"Tony", "kind":"person", "contact_name":"Stark"} \
     'https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts.json'
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
TODO!
```

## Read

### Get and filter all contacts

> `GET /contacts.json`

```shell
# :x stops cURL from prompting for a password
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts.json'
```

```ruby
Quaderno::Contact.all() #=> Array
```

```php?start_inline=1
$contacts = QuadernoContact::find(); // Returns an array of QuadernoContact
```

```swift
let client = Quaderno.Client(/* ... */)

let readContact = Contact.read()
client.request(readContact) { response in
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
    "permalink":"https:///my-account.quadernoapp.com/billing/th3p3rm4l1nk",
    "url":"https://my-account.quadernoapp.com/api/v1/contacts/456987213"
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
    "permalink":"https:///my-account.quadernoapp.com/billing/4n0th3rp3rm4l1nk",
    "url":"https://my-account.quadernoapp.com/api/v1/contacts/456982365"
  }
]
```

### Get a single contact

> `GET /contacts/1.json`

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts/1.json'
```

```ruby
Quaderno::Contact.find(id) #=> Quaderno::Contact
```

```php?start_inline=1
$contact = QuadernoContact::find('IDTOFIND'); // Returns a QuadernoContact
```

```swift
let client = Quaderno.Client(/* ... */)

let readContact = Contact.read()
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
    "permalink":"https:///my-account.quadernoapp.com/billing/th3p3rm4l1nk",
    "url":"https://my-account.quadernoapp.com/api/v1/contacts/456987213"
}
```

## Update

> `PUT /contacts/1.json`

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X PUT \
     -d {"first_name":"Anthony"} \
     'https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts/1.json'
```

```ruby
Quaderno::Contact.update(id, params) #=> Quaderno::Contact
```

```php?start_inline=1
$contact->first_name = 'Joey';
$contact->save();
```

```swift?start_inline=1
// TODO
```

## Delete

> `DELETE /contacts/1.json` returns `204 No Content` if successful.

```shell
curl -u YOUR_API_KEY:x \
     -X DELETE 'https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts/1.json'
```

```ruby
Quaderno::Contact.delete(id) #=> Boolean
```

```php?start_inline=1
$contact->delete();
```

```swift?start_inline=1
// TODO
```
