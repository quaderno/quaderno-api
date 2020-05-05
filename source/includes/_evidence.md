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
  "additional_evidence":null,
  "additional_evidence_country":null,
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
`additional_evidence`           | Additional evidence to locate the customer
`additional_evidence_country`   | Country of the aditional evidence (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`notes`            | Additional notes about the evidence


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

`POST`ing to `/evidence.json` will create a new evidence from the parameters passed.

This will return 201 Created and the current JSON representation of the evidence if the creation was a success.

### Attributes
Parameter          | Mandatory | Description
-------------------|-----------|------------------------------------------------------------------------------------------------
`document_id`      | **yes**   | Invoice or Receipt's ID
`billing_country`  | no        | Customer's billing country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`ip_address`       | no        | Customer's IP address
`bank_country`     | no        | Customer's bank country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`additional_evidence`          | no (yes if `additional_evidence_country` is passed) | An explanatory note about the additional evidence. Up to 255 chars.
`additional_evidence_country`  | no (yes if `additional_evidence` is passed)         | Additional evidence for the customer location (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`notes`             | no       | Additional notes about the evidence

## Update an evidence

> `PUT /evidence/EVIDENCE_ID.json`

```shell
# body.json
{
  "billing_country":"FR",
  "ip_address":"192.168.1.1",
  "bank_country":"FR"
}

curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     --data-binary @body.json \     'https://ACCOUNT_NAME.quadernoapp.com/api/evidence/EVIDENCE_ID.json'
```

```php?start_inline=1
$evidence = QuadernoEvidence::find(EVIDENCE_ID);
$evidence->ip_address = "192.168.23.23";

$evidence->save(); // Returns true (success) or false (error)
```

```ruby
params = {
    ip_address: '192.168.23.23'
}
Quaderno::Evidence.update(EVIDENCE_ID, params) #=> Quaderno::Evidence
```

`PUT`ting to `/evidence/EVIDENCE_ID.json` will update the evidence with the passed parameters.

This will return `200 OK` along with the current JSON representation of the evidence if successful.

## Retrieve: Get and filter all evidence

> `GET /evidence/EVIDENCE_ID.json`


```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/evidence.json'
```

```php?start_inline=1
$evidences = QuadernoEvidence::find(array('page'=>1))
```

```ruby
Quaderno::Evidence.all() #=> Array
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
    "additional_evidence":null,
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
    "additional_evidence":null,
    "additional_evidence_country":null,
    "notes":null
  }
]
```

`GET`ting from `/evidence.json` will return all the user's evidence objects.

You can filter the results in a few ways:

- By state, passing the `state` parameter like `?state=STATE`. You can combine multiple states separated by commas like `?state=confirmed,unsettled`. Valid states are `confirmed`, `unsettled` and `conflicting`.
- By document_id, passing the document ID in the `document_id` parameter, like `?document_id=3231`.

## Retrieve: Get a single evidence

> `GET /evidence/EVIDENCE_ID.json`


```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/evidence/EVIDENCE_ID.json'
```

```php?start_inline=1
$evidence = QuadernoEvidence::find(EVIDENCE_ID);
```

```ruby
evidence = Quaderno::Evidence.find(EVIDENCE_ID) #=> Quaderno::Evidence
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
    "additional_evidence":null,
    "additional_evidence_country":null,
    "notes":null
  }
```

`GET`ting from `/evidence/EVIDENCE_ID.json` will return that specific evidence.
