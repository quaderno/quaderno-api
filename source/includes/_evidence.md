## Evidence

Location evidence are proofs of the customer's location that should be stored in order to be compliant with tax rules in some tax jurisdictions (e.g. EU VAT MOSS).

### Create an evidence

> `POST /evidence.json`

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/evidence \
  -u YOUR_API_KEY:x \
  -d document_id=99999 \
  -d billing_country="FR" \
  -d ip_address="92.186.6.100" \
  -d bank_country="FR"
```
> The above command returns JSON structured like this:

```json
  {
    "id":483,
    "document_id":9748,
    "state":"confirmed",
    "billing_country":"FR",
    "ip_address":"92.186.6.100",
    "ip_country":"ES",
    "bank_country":"FR",
    "additional_evidence":null,
    "additional_evidence_country":null,
    "notes":null
  }
```

#### HTTP Request

`POST /evidence`

#### Parameters

Parameter                      | Type      | Description
-------------------------------|-----------|-----------------------------------------------------------------------------------
`document_id`                  | integer   | The ID of the related document. **Required**
`billing_country`              | string    | Customer's billing country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`ip_address`                   | string    | Customer's IP address
`bank_country`                 | string    | Customer's bank country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`additional_evidence`          | string    | An explanatory note about the additional evidence. Up to 255 chars. Required if `additional_evidence_country` is passed
`additional_evidence_country`  | string    | Additional evidence for the customer location (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes)). Required if  `additional_evidence` is passed)
`notes`                        | string    | Additional notes about the evidence




### Retrieve an evidence

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/evidence/EVIDENCE_ID \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

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

#### HTTP Request

`GET /evidences/EVIDENCE_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`EVIDENCE_ID`            | integer  | The ID of the evidence to retrieve. **Required**





### Update an evidence

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/evidence/EVIDENCE_ID \
  -u YOUR_API_KEY:x \
  -X PUT \
  -d billing_country="FR" \
  -d ip_address="92.186.6.100" \
  -d bank_country="FR"
```
> The above command returns JSON structured like this:

```json
  {
    "id":483,
    "document_id":9748,
    "state":"confirmed",
    "billing_country":"FR",
    "ip_address":"92.186.6.100",
    "ip_country":"ES",
    "bank_country":"FR",
    "additional_evidence":null,
    "additional_evidence_country":null,
    "notes":null
  }
```

#### HTTP Request

`PUT /evidences/EVIDENCE_ID`

#### Parameters

Parameter                      | Type      | Description
-------------------------------|-----------|-----------------------------------------------------------------------------------
`EVIDENCE_ID`                  | integer   | The ID of the evidence to update. **Required**
`document_id`                  | integer   | The ID of the related document. **Required**
`billing_country`              | string    | Customer's billing country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`ip_address`                   | string    | Customer's IP address
`bank_country`                 | string    | Customer's bank country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`additional_evidence`          | string    | An explanatory note about the additional evidence. Up to 255 chars. Required if `additional_evidence_country` is passed
`additional_evidence_country`  | string    | Additional evidence for the customer location (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes)). Required if  `additional_evidence` is passed)
`notes`                        | string    | Additional notes about the evidence






### List all evidence

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/evidence \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

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
  }
]
```

#### HTTP Request

`GET /contacts`

#### Parameters

Parameter      | Type      | Description
---------------|-----------|----------------------------------------------------------------------------
`state`        | string    | A case-sensitive filter on the list based on the evidence's state. You can combine multiple states separated by commas like `?state=confirmed,unsettled`. Valid states are `confirmed`, `unsettled` and `conflicting`.
`document_id`  | string    | A case-sensitive filter on the list based on the document related to the evidence.

