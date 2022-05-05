To upgrade to net6.0

run `update-nugetpackages.ps1`

dotnet build src

resolve any breaking changes causing compile errors

dotnet test src

commit changes -- repo is updated to latest for current version

run update-targetversion -version "net6.0"

dotnet build src

should not have any compile errors

dotnet test src

update nuget packages without locking current major version
if project uses cortside.common.restsharpclient
	dotnet outdated ./src --pre-release Never --upgrade --exclude restsharp
otherwise
	dotnet outdated ./src --pre-release Never --upgrade

validate app insights config:

    var appInsightsOptions = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions {
        EnableAdaptiveSampling = bool.Parse(Configuration["ApplicationInsights:EnableAdaptiveSampling"]),
        InstrumentationKey = Configuration["ApplicationInsights:InstrumentationKey"],
        EnableActiveTelemetryConfigurationSetup = true
    };
    services.AddApplicationInsightsTelemetry(appInsightsOptions);

dotnet build src

resolve any breaking changes causing compile errors

dotnet test src

commit changes -- updated to net6.0

Microsoft.Extensions.Configuration.Binder -- for missing Bind method
