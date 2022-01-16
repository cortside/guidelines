# Health Check

- [Rfc health check response draft](https://inadarei.github.io/rfc-healthcheck)

## Cache

Because it could be expensive to perform health checks frequently, endpoint should be able to return internally cached statuses. Furthermore, it needs to support cache control headers to help consumers cache the response.

## Route

Path should be at `/api/health`.

## Response

Response should have the following shape:

```json
{
  "service":"document-api",
  "build":{
    "timestamp":"2021-12-16T01:58:10+00:00",
    "version":"1.27.939",
    "tag":"1.27.939",
    "suffix":null
  },
  "checks":{
    "document-db":{
      "healthy":true,
      "status":"ok",
      "statusDetail":"Successful",
      "timestamp":"2022-01-16T23:43:05Z",
      "required":true,
      "availability":{
        "count":595,
        "success":595,
        "failure":0,
        "uptime":100,
        "totalDuration":2296,
        "averageDuration":3.8588235294117648,
        "lastSuccess":"2022-01-16T23:43:05Z",
        "lastFailure":null
      }
    },
    "policyserver":{
      "healthy":true,
      "status":"ok",
      "statusDetail":"Successful",
      "timestamp":"2022-01-16T23:43:05Z",
      "required":false,
      "availability":{
        "count":595,
        "success":595,
        "failure":0,
        "uptime":100,
        "totalDuration":6606,
        "averageDuration":11.102521008403361,
        "lastSuccess":"2022-01-16T23:43:05Z",
        "lastFailure":null
      }
    },
    "identityserver":{
      "healthy":true,
      "status":"ok",
      "statusDetail":"Successful",
      "timestamp":"2022-01-16T23:43:11Z",
      "required":false,
      "availability":{
        "count":595,
        "success":595,
        "failure":0,
        "uptime":100,
        "totalDuration":185620,
        "averageDuration":311.96638655462186,
        "lastSuccess":"2022-01-16T23:43:11Z",
        "lastFailure":null
      }
    },
    "user-api":{
      "healthy":true,
      "status":"ok",
      "statusDetail":"Successful",
      "timestamp":"2022-01-16T23:43:11Z",
      "required":false,
      "availability":{
        "count":595,
        "success":595,
        "failure":0,
        "uptime":100,
        "totalDuration":70180,
        "averageDuration":117.94957983193277,
        "lastSuccess":"2022-01-16T23:43:11Z",
        "lastFailure":null
      }
    },
  },
  "uptime":"P0Y0M0DT5H18M33S",
  "healthy":true,
  "status":"ok",
  "statusDetail":null,
  "timestamp":"2022-01-16T23:43:11Z",
  "required":false,
  "availability":{
    "count":3509,
    "success":3509,
    "failure":0,
    "uptime":100,
    "totalDuration":42,
    "averageDuration":0.011969222000569962,
    "lastSuccess":"2022-01-16T23:43:11Z",
    "lastFailure":null
  }
}
```

| Field        | Type     | Description                                                                                                           |
| ------------ | -------- | --------------------------------------------------------------------------------------------------------------------- |
| service      | string   | Name of current service                                                                                               |
| build        | object   | Contains build information of current instance                                                                        |
| checks       | object   | Provides health status summaries of downstream dependencies. Key of each object should match dependency name          |
| status       | string   | Status of service. Value can be `Ok`, `Degraded`, or `Failure`. `Degraded` indicates non-critical dependency failures |
| statusDetail | string   | Optional. Detail relevant to current state of health                                                                  |
| healthy      | boolean  | Indicates whether service is currently healthy                                                                        |
| timestamp    | date     | Current UTC time of response                                                                                          |
| uptime       | duration | How long current instance has been running                                                                            |

### Status codes

Endpoint should return 200 if service is healthy, otherwise it should return 503.
