# REPORTING 

## Report Requests

Every time you need to generate a Quaderno report, you need to create a request with specific parameters. When the report has finished running, the request will give you a reference to a file where you can retrieve your results.

### Create a request

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/reporting/requests \
  -u YOUR_API_KEY:x \
  -d report_type="tax_summary" \
  -d parameters[from_date]="2021-01-01" \
  -d parameters[from_date]="2021-01-31"
```
> The above command returns JSON structured like this:

```json
{
  "id": 456987213,
  "completed_at": 1612351337,
  "created_at": 1612351337,
  "parameters": {
    "from_date": "2020-10-01",
    "to_date": "2020-12-31"
  },
  "report_type": "tax_report",
  "report_url": null,
  "state": "succeeded"
}
```
#### HTTP Request

`POST /reporting/requests`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`report_type`           | string    | The type of report. Can be `tax_summary`, `invoices_list` or `credits_list`. **Required**
`parameters`            | string    | Parameters specifying how the report should be run. Different report types have different required and optional parameters.

**Parameters for tax summary, invoices list, and credits list:**

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`from_date`             | date      | Starting date of data to be included in the report. **Required**
`to_date`               | date      | Ending date of data to be included in the report. **Required**







### Retrieve a request

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/reporting/requests/REQUEST_ID \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
{
  "id": 456987213,
  "completed_at": 1612351337,
  "created_at": 1612351337,
  "parameters": {
    "from_date": "2020-10-01",
    "to_date": "2020-12-31"
  },
  "report_type": "tax_report",
  "report_url": null,
  "state": "succeeded"
}
```
#### HTTP Request

`GET /reporting/requests/REQUEST_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`REQUEST_ID`            | integer   | The ID of the request to retrieve. **Required**






### List all requests


```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/reporting/requests \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
[
  {
    "id": 456987213,
    "completed_at": 1612351337,
    "created_at": 1612351337,
    "parameters": {
      "from_date": "2020-10-01",
      "to_date": "2020-12-31"
    },
    "report_type": "tax_report",
    "report_url": null,
    "state": "succeeded"
  },
  {
    "id": 1,
    "completed_at": 456982365,
    "created_at": 1612351337,
    "parameters": {
      "from_date": "2021-01-01",
      "to_date": "2021-01-31"
    },
    "report_type": "tax_report",
    "report_url": null,
    "state": "succeeded"
  }
]
```
#### HTTP Request

`GET /reporting/requests`

