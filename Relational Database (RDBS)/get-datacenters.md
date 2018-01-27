{{{
  "title": "Get Datacenters",
  "date": "04-01-2015",
  "author": "Relational Database Services",
  "attachments": []
}}}

Get a list of datacenters providing RDBS subscriptions. Calls to this operation must include a token acquired from the authentication endpoint. See the [Login API](../Authentication/login.md) for information on acquiring this token.

### When to Use It

Use this API operation to identify datacenters that you may create subscriptions in. You will use the `dataCenter` response attribute in create subscription requests.

## URL

### Example

    GET https://api.rdbs.ctl.io/v1/datacenters

## Response

### Entity Definition

|      Name      |   Type  |                        Description                             |
| -------------- | ------- | -------------------------------------------------------------- |
| dataCenter     | string  | Short string representing the data center.                     |
| friendlyName   | string  | Name of the datacenter.                                        |
| engines        | array   | List of database engines currently supported in each location. |
| backupLocation | array   | List of available locations that backups can be stored.        |

#### JSON

```json
[
  {
    "dataCenter": "CA3",
    "friendlyName": "CA3 - Canada (Toronto)",
    "engines": [
      "MySQL",
      "MSSQL"
    ],
    "backupLocations": [
      {
        "id": 1,
        "provider": "AWS",
        "endpoint": "s3-us-west-2.amazonaws.com",
        "regionName": "us-west-2",
        "location": "OREGON"
      },
      {
        "id": 5,
        "provider": "CLC",
        "endpoint": "canada.os.ctl.io",
        "regionName": "Canada",
        "location": "Canada"
      },
      {
        "id": 10,
        "provider": "AWS",
        "endpoint": "s3.ca-central-1.amazonaws.com",
        "regionName": "ca-central-1",
        "location": "Canada"
      }
    ]
  },
  {
    "dataCenter": "UC1",
    "friendlyName": "UC1 - US West (Santa Clara)",
    "engines": [
      "MySQL",
      "MSSQL"
    ],
    "backupLocations": [
      {
        "id": 1,
        "provider": "AWS",
        "endpoint": "s3-us-west-2.amazonaws.com",
        "regionName": "us-west-2",
        "location": "OREGON"
      },
      {
        "id": 6,
        "provider": "CLC",
        "endpoint": "uswest.os.ctl.io",
        "regionName": "US-West",
        "location": "US-WEST"
      }
    ]
  },
  {
    "dataCenter": "VA1",
    "friendlyName": "VA1 - US East (Sterling)",
    "engines": [
      "MySQL",
      "MSSQL"
    ],
    "backupLocations": [
      {
        "id": 1,
        "provider": "AWS",
        "endpoint": "s3-us-west-2.amazonaws.com",
        "regionName": "us-west-2",
        "location": "OREGON"
      },
      {
        "id": 7,
        "provider": "CLC",
        "endpoint": "useast.os.ctl.io",
        "regionName": "US-East",
        "location": "US-EAST"
      }
    ]
  }
]
```
