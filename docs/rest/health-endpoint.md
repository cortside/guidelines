# Health Endpoint

- [Rfc health check response draft](https://inadarei.github.io/rfc-healthcheck)

## Cache

Because it could be expensive to perform health checks frequently, endpoint should be able to return internally cached statuses. Furthermore, it needs to support cache control headers to help consumers cache the response.

## Route

Path should be at `/api/health`.

## Response

Response should have the following shape:

```
{
  "service": "document-api",
  "build": {
    "timestamp": "2020-01-04T03:21:02+00:00",
    "version": "0.1.417",
    "tag": "0.1-develop",
    "suffix": "develop"
  },
  "checks": {
    "database": {
      "healthy": true,
      "status": "Ok",
      "statusDetail": "kilroy was here",
      "timestamp": "2020-01-08T01:27:23Z"
    },
    "communicationService": {
      "healthy": true,
      "status": "Ok",
      "statusDetail": "kilroy was here",
      "timestamp": "2020-01-08T01:27:23Z"
    },
    "policyServer": {
      "healthy": true,
      "status": "Ok",
      "statusDetail": "kilroy was here",
      "timestamp": "2020-01-08T01:27:23Z"
    }
  },
  "status": "Ok",
  "statusDetail": "kilroy was here",
  "healthy": true,
  "timestamp": "2020-01-08T01:27:23Z",
  "uptime": "3.22:00:44.1522033",
  "cached": true
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
| cached       | boolean  | Indicates whether current response is an internal cache                                                               |

### Status codes

Endpoint should return 200 if service is healthy, otherwise it should return 503.
