# Evidence

Location evidence are proofs of the customer's location that should be stored in order to be EU VAT MOSS compliant.

## The evidence object
```json
{
  "id":3649491,
  "document_id":"5059bdbf2f412e0901000024",
  "state":"confirmed",
  "billing_country":"ES",
  "ip_address":"192.168.1.1",
  "ip_country":"FR",
  "bank_country":"FR",
  "vat_number":null,
  "vies_reference":null,
  "additional_evidence":null,
  "additional_evidence_country":null
  "notes":null
}
```


Attribute          | Description
-------------------|-----------------------------------------------------------------------------------------------------------
`id`               | Evidence ID
`document_id`      | Invoice or Receipt's ID
`state`            | Customer's location evidence state (confirmed or unconfirmed)
`billing_country`  | Customer's billing country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`ip_address`       | Customer's IP address
`ip_country`       | Customer's country geolocated by IP (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`bank_country`     | Customer's bank country
`vat_number`       | Customer's intra-community VAT number (if present)
`notes`            | Readable information related to the evidence state


## Create an evidence

> `POST /evidence.json`

```shell
# body.json
{
  "document_id":"5059bdbf2f412e0901000024",
  "billing_country":"FR",
  "ip_address":"192.168.1.1",
  "bank_country":"FR"
}

curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     --data-binary @body.json \     'https://ACCOUNT_NAME.quadernoapp.com/api/evidence.json'
```

```php?start_inline=1
$evidence = new QuadernoEvidence(array(
                                        'document_id' => '5059bdbf2f412e0901000024',
                                        'billing_country' => 'FR',
                                        'ip_address' => '192.168.1.1',
                                        'bank_country' => 'FR'));

$evidence->save(); // Returns true (success) or false (error)
```

```ruby
contact = Quaderno::Evidence.create(
                                      document_id: '5059bdbf2f412e0901000024',
                                      billing_country: 'FR',
                                      ip_address: '192.168.1.1',
                                      bank_country: 'FR')) #=> Quaderno::Evidence

```

```swift?start_inline=1
// Coming soon!
```

`POST`ing to `/evidence.json` will create a new evidence from the parameters passed.

This will return 201 Created and the current JSON representation of the evidence if the creation was a success.

## Retrieve: Get and filter all evidence

> `GET /evidence/EVIDENCE_ID.json`


```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/evidence.json'
```

```php?start_inline=1
// Coming soon!
```

```ruby
# Coming soon!

```

```swift?start_inline=1
// Coming soon!
```

```json
[
  {
    "id":487,
    "document_id":9756,
    "state":"unsettled",
    "billing_country":"US",
    "ip_address":null,
    "ip_country":null,
    "bank_country":null,
    "vat_number":null,
    "vies_reference":null,
    "additional_evidence":null,
    "additional_evidence_country":null,
    "notes":null
  },
  {
    "id":486,
    "document_id":9755,
    "state":"confirmed",
    "billing_country":"ES",
    "ip_address":"92.186.16.30",
    "ip_country":"ES",
    "bank_country":null,
    "vat_number":null
    "vies_reference":null,
    "additional_evidence":null,
    "additional_evidence_country":null,
    "notes":null
  },
  {
    "id":485,"document_id":9752,
    "state":"confirmed",
    "billing_country":"ES",
    "ip_address":"92.186.16.30",
    "ip_country":"ES",
    "bank_country":null,
    "vat_number":null,
    "vies_reference":null,
    "additional_evidence":null,
    "additional_evidence_country":null,
    "notes":null
  },
  {
    "id":484,
    "document_id":9749,
    "state":"confirmed",
    "billing_country":"ES",
    "ip_address":"80.29.119.132",
    "ip_country":"ES",
    "bank_country":null,
    "vat_number":null,
    "vies_reference":null
    ,"additional_evidence":null,
    "additional_evidence_country":null,
    "notes":null
  },
  {
    "id":483,
    "document_id":9748,
    "state":"conflicting",
    "billing_country":"ES",
    "ip_address":null,
    "ip_country":null,
    "bank_country":"US",
    "vat_number":null,
    "vies_reference":null,
    "additional_evidence":null,
    "additional_evidence_country":null,
    "notes":null
  }
]
```

## Retrieve: Get a single evidence

> `GET /estimates/ESTIMATE_ID.json`


```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/evidence/EVIDENCE_ID.json'
```

```php?start_inline=1
// Coming soon!
```

```ruby
# Coming soon!

```

```swift?start_inline=1
// Coming soon!
```

```json
  {
    "id":483,
    "document_id":9748,
    "state":"conflicting",
    "billing_country":"ES",
    "ip_address":null,
    "ip_country":null,
    "bank_country":"US",
    "vat_number":null,
    "vies_reference":null,
    "additional_evidence":null,
    "additional_evidence_country":null,
    "notes":null
  }
```

### Attributes
Parameter          | Mandatory | Description
-------------------|-----------|------------------------------------------------------------------------------------------------
`document_id`      | **Yes**   | Invoice or Receipt's ID
`billing_country`  | No        | Customer's billing country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`ip_address`       | No        | Customer's IP address
`bank_country`     | No        | Customer's bank country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
