# Evidences

Location evidences are proofs of the customer's location that should be stored in order to be VAT compliant.

## The evidence object
```json
{
  "id":3649491,
  "document_id":"5059bdbf2f412e0901000024",
  "billing_country":"ES",
  "ip_address":"192.168.1.1",
  "ip_country":"FR",
  "vat_number":null,
  "notes":null
}
```


Attribute          | Description
-------------------|-----------------------------------------------------------------------------------------------------------
`id`               | Evidence ID
`document_id`      | Invoice or Receipt's ID
`billing_country`  | Customer's billing country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`ip_address`       | Customer's IP address
`ip_country`       | Customer's country geolocated by IP (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`vat_number`       | Customer's intra-community VAT number (if present)
`notes`            | Readable information related to the evidence state


## Create an evidence

> `POST /evidences.json`

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
     --data-binary @body.json \     'https://ACCOUNT_NAME.quadernoapp.com/api/evidences.json'
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

`POST`ing to `/evidences.json` will create a new evidence from the parameters passed.

This will return 201 Created and the current JSON representation of the evidence if the creation was a success.


### Attributes
Parameter          | Mandatory | Description
-------------------|-----------|------------------------------------------------------------------------------------------------
`document_id`      | **Yes**   | Invoice or Receipt's ID
`billing_country`  | No        | Customer's billing country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`ip_address`       | No        | Customer's IP address
`bank_country`     | No        | Customer's bank country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
