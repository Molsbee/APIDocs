{{{
  "title": "Create Subscription",
  "date": "04-07-2015",
  "author": "Relational Database Services",
  "attachments": []
}}}

Create a new subscription. Calls to this operation must include a token acquired from the authentication endpoint. See the [Login API](../Authentication/login.md) for information on acquiring this token.

### When to Use It

Use this API operation when you want to create a new subscription.

## URL

### Structure

    POST https://api.rdbs.ctl.io/v1/{accountAlias}/subscriptions

### Example

    POST https://api.rdbs.ctl.io/v1/ALIAS/subscriptions

## Request

### URI Parameters

|     Name     |  Type  |             Description             | Req. |
| ------------ | ------ | ----------------------------------- | ---- |
| accountAlias | string | Short code for a particular account | Yes  |

### Content Properties

|         Name           |   Type  |                                                       Description                                                                                         | Req. |
| ---------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- |
| instanceType           | string  | One of MYSQL, MySQL_REPLICATION, MSSQL_WEB, MSSQL_STANDARD, MSSQL_ENTERPRISE                                                                              | Yes  |
| location               | string  | Datacenter id. Available datacenters are listed in [Get Datacenters method](get-datacenters.md)                                                           | Yes  |
| networkId              | string  | ID of the Customer's Network for the location provided (used only by MSSQL at this time).  This can be retrieved from the Get Network List API operation. | No   |
| replicated             | boolean | Turns on replication for subscription (used only by MSSQL at this time).  Default: false.                                                                 | No   |
| replica                | object  | Replication information secondary location and network (used only by MSSQL at this time).                                                                 | No   |
| version                | string  | Database Engine version (MySQL - 5.6, 5.7) currently only used by MySQL.  Default: 5.7                                                                    | No   |
| externalId             | string  | The hostname part of the connection string                                                                                                                | Yes  |
| machineConfig          | object  | Server provisioning information for cpu, memory, and storage                                                                                              | Yes  |
| backupLocationId       | number  | Backup location id available selections for each data center can be retrieved at [Get Datacenters method](get-datacenters.md)                             | Yes  |
| backupRetentionDays    | number  | Number of days to retain database backups                                                                                                                 | Yes  |
| destinations           | array   | Server capacity notification destinations                                                                                                                 | No   |
| users                  | array   | Initial database user account.                                                                                                                            | Yes  |
| backupTime             | object  | Time of day to run backups (UTC)                                                                                                                          | Yes  |
| configurationProfileId | number  |  ID of the configuration profile created by customer (only used for MySQL at this time).                                                                  | No   |

### Replica Definition
|    Name   |  Type  |                                           Description                                                                   | Req. |
| location  | string | Datacenter id. Available datacenters are listed in [Get Datacenters method](get-datacenters.md)                         | Yes  |
| networkId | string | ID of the Customer's Network for the location provided.  This can be retrieved from the Get Network List API operation. | Yes  |

### MachineConfig Definition

| Name | Type | Description | Req. |
| --- | --- | --- | --- |
| cpu | number | Number of CPUs to allocate to server | Yes |
| memory | number | Amount of RAM to allocate to server (GB) | Yes |
| storage | number | Amount of disk storage to allocate to server (GB) | Yes |

### Destination Definition

| Name | Type | Description | Req. |
| --- | --- | --- | --- |
| destinationType | string | EMAIL | Yes |
| location | string | Where to send notifications. For EMAIL, this is an email address. | Yes |
| notifications | array | A list of notifications to receive. | Yes |

### User Definition

| Name | Type | Description | Req. |
| --- | --- | --- | --- |
| name | string | User name. | Yes |
| password | string | User password. The password must pass our minimum requirements: at least 9 characters and contain at least 3 of the following: uppercase letters, lowercase letters, numbers, symbols| Yes |

### BackupTime Definition

| Name | Type | Description | Req. |
| --- | --- | --- | --- |
| hour | number | Hour of day to start backup (UTC). | Yes |
| minute | number | Minute of minute to start backup (UTC). | Yes |

### Examples

```json
{
  "instanceType": "MySQL",
  "location": "VA1",
  "externalId": "st-api-test",
  "machineConfig": {
    "cpu": 1,
    "memory": 1,
    "storage": 1
  },
  "backupLocationId": 1,
  "backupRetentionDays": 7,
  "destinations": [
    {
      "destinationType": "EMAIL",
      "location": "foo@bar.com",
      "notifications": [
        "CPU_UTILIZATION",
        "MEMORY_UTILIZATION"
      ]
    }
  ],
  "users": [
   {
     "name": "admin",
     "password": "AstrongPW!"
   }
  ],
  "backupTime": {
    "hour": 9,
    "minute": 15
  }
}
```

```json
{
  "instanceType": "MSSQL_WEB",
  "location": "VA1",
  "networkId": "a933432bd8894e84b6c4fb123e48cb8b",
  "replicated": true,
  "replica": {
    "location": "NY1",
    "networkId": "b977472bd8894e84b6c4fb123e48cb8a"
  },
  "externalId": "st-api-test",
  "machineConfig": {
      "cpu": 1,
      "memory": 1,
      "storage": 1
   },
   "backupLocationId": 1,
   "backupRetentionDays": 7,
   "destinations": [
      {
        "destinationType": "EMAIL",
        "location": "foo@bar.com",
        "notifications": [
          "CPU_UTILIZATION",
          "MEMORY_UTILIZATION"
        ]
      }
    ],
    "users": [
     {
       "name": "admin",
       "password": "AstrongPW!"
     }
    ],
    "backupTime": {
      "hour": 9,
      "minute": 15
    }
}
```

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
| backupLocation       | string  | String represent the provider and region associated with selected backup location.     |

### Server Entity Definition

|     Name    |  Type   |                                Description                                          |
| ----------- | ------- | ----------------------------------------------------------------------------------- |
| id          | number  | Server id                                                                           |
| alias       | string  | Server alias (hostname)                                                             |
| location    | string  | Datacenter id where server is located                                               |
| cpu         | number  | Number of CPUs allocated to the server                                              |
| memory      | number  | Amount of RAM allocated to the server (GB)                                          |
| storage     | number  | Amount of disk storage allocated to the server (GB)                                 |
| status      | string  | Internal field used during configuration process to track changes.                  |
| active      | boolean | Internal field used during configuration process to track changes.                  |
| role        | string  | Used for denoting server role in replication PRIMARY or REPLICANT.                  |
| health      | string  | Health of server UP, DOWN, UNKNOWN (Only used by MSSQL at this time)                |
| connections | number  | Estimated number of concurrent connections possible given the server's RAM and CPU. |

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
{
  "id": 894,
  "location": "VA1",
  "instanceType": "MySQL",
  "externalId": "st-api-test",
  "status": "Configuring",
  "backupTime": "9:15",
  "backupLocation": "AWS - OREGON",
  "backupRetentionDays": 6,
  "servers": [
    {
      "id": 1025,
      "alias": "VA1DBDVMYSQL2130",
      "location": "va1",
      "cpu": 1,
      "memory": 1,
      "storage": 1,
      "ipAddress": "10.126.63.89",
      "status": "ACTIVE",
      "active": true,
      "role": "PRIMARY",
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
}
```
