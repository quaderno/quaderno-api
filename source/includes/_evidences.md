# Evidences

Location evidences are proofs of the location of your customer that should be stored in order to be VAT compliant.

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

Parameter          | Mandatory | Description
-------------------|-----------|------------------------------------------------------------------------------------------------
`document_id`      | **Yes**   | Invoice or Receipt's ID
`billing_country`  | No        | Customer's billing country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`ip_address`       | No        | Customer's IP address
`bank_country`     | No        | Customer's bank country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
