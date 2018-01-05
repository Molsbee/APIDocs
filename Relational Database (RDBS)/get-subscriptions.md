{{{
  "title": "Get Subscriptions",
  "date": "04-05-2015",
  "author": "Relational Database Services",
  "attachments": []
}}}

Returns a list of all subscriptions and details. Calls to this operation must include a token acquired from the authentication endpoint. See the [Login API](../Authentication/login.md) for information on acquiring this token.

### When to Use It

Use this API operation when you want to see all subscriptions and their details. You can narrow focus to a single datacenter and/or subscription status.

## URL

### Structure

    GET https://api.rdbs.ctl.io/v1/{accountAlias}/subscriptions?dataCenter={dataCenterId}&status={status}

### Example

    GET https://api.rdbs.ctl.io/v1/ALIAS/subscriptions
    GET https://api.rdbs.ctl.io/v1/ALIAS/subscriptions?dataCenter=UC1
    GET https://api.rdbs.ctl.io/v1/ALIAS/subscriptions?staus=ACTIVE
    GET https://api.rdbs.ctl.io/v1/ALIAS/subscriptions?dataCenter=UC1&staus=PENDING

## Request

### URI Parameters

|     Name     |  Type  |              Description             | Req. |
| ------------ | ------ | ------------------------------------ | ---- |
| AccountAlias | string | Short code for a particular account. | Yes  |

### Query Parameters

|    Name    |  Type  |                                                    Description                                                                                                  |
| ---------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dataCenter | string | Short string representing the data center. Datacenters are listed in the `dataCenter` attribute in the RDBS [Get Datacenters](get-datacenters.md) API operation |
| status     | string | One of: ACTIVE, BACKING_UP, CONFIGURING, RESTORING, SUCCESS, TERMINATED                                                                                         |

## Response

### Entity Definition

| Name                 |  Type   |                    Description                                                         |
| -------------------- | ------- | -------------------------------------------------------------------------------------- |
| id                   | number  | Subscription id                                                                        |
| accountAlias         | string  | Customer account alias associated with subscription                                    |
| location             | string  | Datacenter id                                                                          |
| instanceType         | string  | Type of relational database.                                                           |
| engine               | string  | Database engine type MSSQL, MySQL                                                      |
| edition              | string  | MSSQL Edition Web, Standard, Enterprise                                                |
| version              | string  | Database Engine Version                                                                |
| externalId           | string  | External id used in connection strings, etc.                                           |
| restartRequired      | boolean | Notification of if subscription needs restarting to apply changes                      |
| status               | string  | One of: Active, Backing Up, Configuring, Restoring, Terminated                         |
| replicationHealth    | string  | Health of replication - UP, Down, Unknown (Only used by MSSQL at this time)            |
| backupTime           | string  | Scheduled backup time in "hour:minute"                                                 |
| backupRetentionDays  | string  | Number of days to retain backups.                                                      |
| servers              | array   | Servers implementing the subscription. Replicated subscriptions will have two servers. |
| host                 | string  | Host DNS name used in connection strings.                                              |
| port                 | string  | Host port used in connection strings.                                                  |
| certificate          | string  | TLS certificate used to secure subscription connections.                               |
| backups              | array   | List of all backup for subscription within retention period.                           |
| configurationProfile | object  | Configuration Profile object representing current database configuration information   |
| pendingJobCount      | number  | Current count of pending jobs being performed on subscription                          | 
| replicated           | boolean | Boolean representing if subscription is replicated.                                    |

### Server Entity Definition

|     Name    |  Type  |                                Description                                          |
| ----------- | ------ | ----------------------------------------------------------------------------------- |
| id          | number | Server id                                                                           |
| alias       | string | Server alias (hostname)                                                             |
| location    | string | Datacenter id where server is located                                               |
| cpu         | number | Number of CPUs allocated to the server                                              |
| memory      | number | Amount of RAM allocated to the server (GB)                                          |
| storage     | number | Amount of disk storage allocated to the server (GB)                                 |
| attributes  | object | Misc attributes for the server                                                      |
| connections | number | Estimated number of concurrent connections possible given the server's RAM and CPU. |


### Backup Entity Definition

|    Name    |  Type  |           Description            |
| ---------- | ------ | -------------------------------- |
| id         | number | Backup ID                        |
| fileName   | string | File Name                        |
| backupTime | number | Time of Backup                   |
| backupType | string | Type of Backup AUTOMATED, MANUAL |
| status     | string | Status SUCCESS, FAILED           |
| size       | number | Total size in bytes of back file |

### Configuration Profile Entity Definition

|     Name     |   Type  |                    Description                          |
| ------------ | ------- | ------------------------------------------------------- |
| id           | number  | Configuration Profile ID                                |
| name         | string  | Name of profile                                         |
| description  | string  | Brief description of profile                            |
| lastEditedBy | string  | Who last edited profile                                 |
| lastEdited   | number  | Time of last edit                                       |
| parameters   | object  | Map of key value paris of configuration profile changes |
| isDefault    | boolean | Default profile for database engine                     |

## Examples

```json
[
  {
    "id": 894,
    "location": "VA1",
    "instanceType": "MySQL",
    "externalId": "st-api-test",
    "status": "Configuring",
    "backupTime": "9:15",
    "backupRetentionDays": 6,
    "servers": [
      {
        "id": 1025,
        "alias": "VA1DBDVMYSQL2130",
        "location": "va1",
        "cpu": 1,
        "memory": 1,
        "storage": 1,
        "attributes": {
          "INTERNAL_IP": "10.126.63.89"
        },
        "connections": 89
      }
    ],
    "host": "10.126.63.31",
    "port": 49562,
    "certificate": "-----BEGIN CERTIFICATE-----....-----END CERTIFICATE-----",
    "backups": [
      {
        "id": 1,
        "fileName": "1_2017-12-30T00:00:17Z",
        "backupTime": 1514592017000,
        "backupType": "Automated",
        "status": "SUCCESS",
        "size": 4853942
      }
    ],
    "configurationProfile": {
      "id": 1,
      "name": "MySQL Default",
      "description": "RDBS default MySQL Configuration",
      "lastEditedBy": null,
      "lastEdited": 1464189378000,
      "parameters": {}
    },
    "pendingJobCount": 0,
    "replicated": false
  },
  ...
]
```
