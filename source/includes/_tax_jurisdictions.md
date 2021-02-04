## Tax Jurisdictions

A tax jurisdiction is an area subject to its own tax regulations. A tax jurisdiction can be a country, region, group of countries, province, state, city, county, district or any other area with a designated authority for tax purposes.

A businesses cannot collect taxes in a particular jurisdiction without being registered for tax collection with their tax authorities first. 

This API will allow you to get all tax jurisdictions you need to manage your tax IDs and your tax rates in your Quaderno account. 

### Retrieve a jurisdiction

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/jurisdictions/JURISDICTION_ID \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
{
  "id": 94,
  "name": "Canada – British Columbia"
}
```
#### HTTP Request

`GET /jurisdictions/JURISDICTION_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`JURISDICTION_ID`       | integer   | The ID of the jurisdiction to retrieve. **Required**






### List all jurisdictions

> `GET /jurisdictions.json?country=CA`

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/jurisdictions.json?country=CA \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 94,
    "name": "Canada – British Columbia"
  },
  {
    "id": 95,
    "name": "Canada – Quebec"
  },
  {
    "id": 96,
    "name": "Canada – Saskatchewan"
  },
  {
    "id": 100,
    "name": "Canada – Manitoba"
  },
  {
    "id": 129,
    "name": "Canada – GST/HST"
  }
]
```

#### HTTP Request

`GET /jurisdictions`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`country`               | string    | A case-sensitive filter on the list based on the jurisdiction's country.
`region`                | string    | A case-sensitive filter on the list based on the jurisdiction's region.

