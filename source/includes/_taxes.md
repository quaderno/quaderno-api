# Taxes

One of the killer features Quaderno provides is efficient and easy tax management. As part of this, we offer an API for easy tax calculation.

## Calculate Taxes

> `GET /taxes/calculate.json`

```shell

curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X GET \
     'https://ACCOUNT_NAME.quadernoapp.com/api/taxes.json?country=ES&postal_code=08080&vat_number=ESA58818501'
```

```ruby
params = {
    country: 'ES',
    postal_code: '08080'
    vat_number: 'ESA58818501'
}
tax = Quaderno::Tax.calculate(params) #=> Quaderno::Tax
```

```php?start_inline=1
$data = array(
  'country' => 'ES',
  'postal_code' => '08080',
  'tax_id' => 'A58818501'
);

$tax = QuadernoTax::calculate($data); // Returns a QuadernoTax
$tax->name; // "VAT"
$tax->rate; // 21.0
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let params : [String: Any] = [
    "country": "ES",
    "postal_code": "08080"
    "vat_number": "ESA58818501"
]

let taxCalculation = Tax.calculate(params)
client.request(taxCalculation) { response in
    // response will contain the result of the request.
}
```

```json
{
    "name": "VAT",
    "rate": 21.0,
    "notes": null
}
```

`GET`ting to `/taxes/calculate.json` will calculate the applicable taxes given a customer's data.

Parameter          | Mandatory | Description
-------------------|-----------|------------------------------------------------------------------------------------------------
`country`          | **Yes**   | Customer's country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`postal_code`      | No        | Customer's postal code (ZIP)
`vat_number`       | No        | Customer's VAT number
`transaction_type` | No        | Values: `eservice`, `ebook`, `standard`. Default is `eservice`

This will return a `200 OK` if the request was a success, along with the taxes represented as a JSON string.

## Validating VAT Numbers

> GET /taxes/validate.json

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X GET \
     'https://ACCOUNT_NAME.quadernoapp.com/api/taxes.json?country=ES&vat_number=ESA58818501'
```

```ruby
# Coming soon!
```

```php?start_inline=1
// Coming soon!
```

```swift?start_inline=1
// Coming soon!
```

```json
{
    "valid":true
}
```

`GET`ting to `/taxes/validate.json` will validate the given EU VAT number.

Parameter    | Mandatory | Description
-------------|-----------|------------------------------------------------------------------------------------------------
`country`    | **Yes**   | Customer's country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`vat_number` | **Yes**   | Customer's VAT number
