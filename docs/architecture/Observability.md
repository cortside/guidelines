# Observability

There are several aspects to understanding the health, performance and usage of a service and the enterprise system it belongs to, as well as to being able to troubleshoot problems.  The following are key aspects to supporting these needs.

## Logging

All services should log to a centralized service that aggregates logs together.  This will allow for multiple instances of a given service to have logs found together, as well as for all related services within the enterprise system to be located together and searched across.  Logging should be at a sufficient level of being able to understand when an operation is operating successfully or not and when not, where it failed.

Most logging services support the concept of structured logging, in fact that should likely be a requirement for selection of a logging service.  Structured logging allow for the association of properties that may not be in the textual log contents that can be used for searching and filtering.

Care should be taken for what is logged and to be cognizant of things like personal identifying information (PII), secrets and passwords, or anything that would lead to a security leak or loss of private data.

## Application Metrics

Services should be instrument to gather statistics about the state of the running service as well as of individual operations. Aggregate metrics in centralized metrics service, typically an application performance management system (APM) that can provide reporting and alerting.

Key metrics to monitor might include:

* Error rates
* CPU, memory, disk, network and other resource usage
* Request/operation response times
* Request rates
* Uptime and availability

## Audit Logging

Each service should be responsible for keeping track of who acted on what entities when.  Ideally this is logged in the data store used for the service context.  This can be in the form of created and last modified columns on tables and/or separate table that keeps track of changes.

Another form of audit logging can be done via change tracking that is either built into the data store or via something like triggers.  This should allow for being able to see who created or changed something but what in specific was changed.

## Distributed Tracing

Services should be instrumented to generate or keep track of a unique request id, or correlationId.  This request id should be part of the logging properties in all structured logging.  The service should pass the request id to all services that are involved in the handling of the request.

The requestId can be used to identify performance issues as well in aide in troubleshooting of operations across multiple services.

## Exception Tracking

Report all exceptions to a centralized exception tracking service that aggregates and tracks exceptions.  This may be part of the same service that handles logging and/or application metrics.

## Alerts

Use the information captured by logging, application metrics, exception tracking and health monitoring to raise alerts of situations that might allow for both reactive and preemptive actions on the health and performance of the enterprise system.

Alerts can be of specific events, i.e. an exception, or of a change in a trend, i.e. the number of successful requests over a given period.

## Health Check API

Each service should expose a health check that can return the health of the service.  This response should have an overall status as well as a status for each dependency.

See [here](../rest/HealthCheck.md) for more specifics.

This health check be used by a [health monitoring service](HealthMonitoring.md) for reporting and visualizing the overall health of the enterprise system.

The health check can also be used by orchestrators and load balancers that can direct traffic to healthy instances of a service.

## Log deployments and changes

Keep track of and log service deployments and versions.  Keep continuing changelog by service of changes to service.  Keep track of changes to the running environment where any enterprise system service runs.
