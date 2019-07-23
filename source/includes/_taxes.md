# Taxes

One of the killer features Quaderno provides is efficient and easy tax management. As part of this, we offer an API for easy tax calculation.

## Calculate Taxes

> `GET /taxes/calculate.json`

```shell

curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X GET \
     'https://ACCOUNT_NAME.quadernoapp.com/api/taxes/calculate.json?country=US&postal_code=94010'
```

```ruby
params = {
    country: 'US',
    postal_code: '94010'
}
tax = Quaderno::Tax.calculate(params) #=> Quaderno::Tax
```

```php?start_inline=1
$data = array(
  'country' => 'US',
  'postal_code' => '94010'
);

$tax = QuadernoTax::calculate($data); // Returns a QuadernoTax
$tax->name; // "Sales Tax"
$tax->rate; // 9.5
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let params : [String: Any] = [
    "country": "US",
    "postal_code": "94010"
]

let taxCalculation = Tax.calculate(params)
client.request(taxCalculation) { response in
    // response will contain the result of the request.
}
```

```json
{
    "name": "Sales tax",
    "rate": 9.5,
    "extra_name":null,
    "extra_rate":null,
    "country":"US",
    "region":"CA",
    "county":"SAN MATEO",
    "city":"BURLINGAME",
    "county_tax_code":"41",
    "city_tax_code":"746",
    "transaction_type":"eservice",
    "notes":null
}
```

`GET`ting to `/taxes/calculate.json` will calculate the applicable taxes given a customer's data.

Parameter          | Mandatory | Description
-------------------|-----------|------------------------------------------------------------------------------------------------
`country`          | **Yes**   | Customer's country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`postal_code`      | No        | Customer's postal code / zip
`city`             | No        | Customer's city (for US sales tax)
`vat_number`       | No        | Customer's VAT number
`transaction_type` | No        | Values: `eservice`, `ebook`, `standard`, `saas`. Defaults to the "default tax type" configured in your account taxes settings.

This will return a `200 OK` if the request was a success, along with the taxes represented as a JSON string.

## Validating Business Numbers

> GET /taxes/validate.json

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X GET \
     'https://ACCOUNT_NAME.quadernoapp.com/api/taxes/validate.json?country=IE&vat_number=IE6388047V'

{
    "valid":true
}
```

```ruby
country = 'IE'
vat_number = 'IE6388047V'

Quaderno::Tax.validate_vat_number(country, vat_number) #=> true, false or nil
```

```php?start_inline=1

$country = 'IE';
$vat_number = 'IE6388047V';

QuadernoTax::validate_vat_number($country, $vat_number) // true, false or null
```

```swift?start_inline=1
// Coming soon!
```


`GET`ting to `/taxes/validate.json` will validate the given business number. Currently Quaderno validates EU VAT numbers, ABN, and NZBN.

Please keep in mind that business numbers are not actually validated while using the Sandbox.

Parameter    | Mandatory | Description
-------------|-----------|------------------------------------------------------------------------------------------------
`country`    | **Yes**   | Customer's country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`vat_number` | **Yes**   | Customer's business number

This will return a `200 OK` and the result of validating the business number against the official external validation service. The result can be true (valid business number), false (invalid business number) or null (the external service is temporarily unavailable).
