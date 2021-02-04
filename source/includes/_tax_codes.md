## Tax Codes

Tax codes classify goods and services for tax purposes.

### Retrieve a tax code

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_codes/TAX_CODE_ID \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
  {
    "id": "eservice",
    "description": "Service that is delivered over the internet. It is essentially automated, involves minimal human intervention and in the absence of information technology does not have viability.",
    "name": "Electronically supplied services"
  }
 ```

#### HTTP Request

`GET /tax_codes/TAX_CODE_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`TAX_CODE_ID`            | integer   | The ID of the tax code to retrieve. **Required**




### List all tax codes

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/tax_codes \ 
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
[
  {
    "id": "ebook",
    "description": "An electronic book that is sold with unlimited use.",
    "name": "eBook"
  },
  {
    "id": "eservice",
    "description": "Service that is delivered over the internet. It is essentially automated, involves minimal human intervention and in the absence of information technology does not have viability.",
    "name": "Electronically supplied services"
  }
]
```

#### HTTP Request

`GET /tax_codes`


