
# observability-gap-analysis-checklist

Checklist for fast assessment of Application/Service/Infra's observability maturity and production readiness from SRE/Ops point of view

![image](https://github.com/telecomprofi/observability-gap-analysis-checklist/assets/17558124/9cd67611-e9ea-46a5-bb4c-79dd98e63ffd)


### Business KPIs/metrics 
- SRE teaches us that since every service is for businees we should prioritize business metrics over technical
- [ ] Number of active users connected
- [ ] Number of successful transactions (sales, purchases, etc)
- etc

### Infra monitoring

Metrics are exposed by host/vm/orchestrator and ingested into standard Observability Platform:

- [ ] CPU usage data is collected
- [ ]	Memory usage data is collected
- [ ] Disk/Storage usage data is collected
- [ ] Inode fs usage data is collected (*nix only)
- [ ] Network latency (RTT) data is collected
- [ ] DNS resolution/Certificate Expiration (FE, API Endpoint) is collected

Examples of infra VM/Host monitoring alerts (warn/alert thresholds are provided in parenthesis):
*	Inode usage (80% / 90%) - *nix only
* 	CPU load (75% / 85% average over 2 minutes)
* 	Disk usage (80% / 90%)
* 	Memory usage (60% / 70% average over 2 minutes)
* 	Host stopped reporting
* 	NTP sync (host time is out of sync)
 
### App Monitoring 
(4 golden signals):
- [ ] Error Rate is collected
- [ ] Saturation data is collected
- [ ] Response Latency data is collected
- [ ] Traffic (QPS, Transactions per second) data is collected

Optional (containerized/distributed app components):
- [ ] Liveness/Healthy endpoint exists
- [ ] Readiness endpoint exists

### Log collection
- [ ] app logs are collected
- [ ] Tagged logging configured to include:
*   Timestamp in UTC timezone, nanosecond granurality is advised for conteinerized/distributed web-apps
* 	(Sub)domain of request
* 	Application environment (production, staging/UAT or testing/SIT)
* 	UUID of request
* 	Session ID
* 	ClusterID (distributed/containerized apps)
* 	ContainerID (distributed/containerized apps)

- [ ] 	log levels are configured according to env:
* 	production set to INFO
* 	staging/UAT set to DEBUG
* 	testing/SIT set to DEBUG

- [ ] 	HTTP requests in logs are condensed to a single line log output including:
* 	IP address of originating request
* 	HTTP method
* 	HTTP parameters

- [ ] 	Sensitive parameters are filtered from logs
- [ ] 	logs are shipped/ingested to centralized storage backend
- [ ] 	Logs are in structured format (JSON) and are indexed for fast query/triggering alerts
- [ ] 	log-based alerts are configured
- [ ] 	alert is configured for 'no logs' in last xx minutes/hours

### Metrics collection
- [ ] 	business metrics are defined and generated by an app component/infra 
- [ ] 	metrics are exposed by app with proper description/tags, 
- [ ] 	metrics are Pushed/Pulled (scraped/pushed/collected/filtered) and are shipped to metrics storage backend
- [ ] 	alert is configured for metrics not available in last xx minutes/hours

### Tracing (distributed & containerized app)

optional for monolith, mandatory for containerized/distributed web apps
- [ ] 	application is instrumented with OpenTelemetry/Trace library to facilitate tracing
- [ ] 	traceID is copied over from HTTP header and reflected in Log/Tracing entries on incoming/outgoing requests
- [ ] 	Traces are shipped (are collected) and ingested to storage backend

### APM - Application Performance monitoring

APM service is configured to monitor
- [ ] 	App performance
- [ ] 	Request time

### RUM
- [ ] client browser session/client app is instrumented with RUM 
- [ ] Real user stats/metrics are being collected/analysed/acted upon

 
### Dashboards
- [ ] 	Business KPI dashboards exist and URL to access them is documented
- [ ] 	Canary (Business KPI Before/After Release Dashboard(s) exists
- [ ] 	Golden signals dashboard(s) exists
 
### Alerts
- [ ] 	metrics-based alerts are configured
- [ ] 	log-based alerts are configured
- [ ] 	Alerts are sent to appropriate channel (Instant Messager group, e-mail, SMS)
- [ ] 	Only Alerts from Prod env are sent to Support team

examples of alert use cases:
* 	Traffic exceeds threshold (is more than X TPS/QPS for last 10min)
* 	Error rate higher than x over last 5min (4xx, 5xx)
* 	Disk/Memory (Quota) exceeded 90%
* 	Inode usage exceed 90% (*nix fs only)
* 	CPU usage over 90% for last 5 min
* 	Latency is > XXX msec over 5 min
* 	Certificate expires in less than 10 days
* 	logs are not being shipped for last 5min
* 	metrics are not available for the last 5 min
* 	heartbeat missing for over 5 min
 	 
 
### Synthetic checks & Status
- [ ] 	Response is not OK/200 for 3 consecutive retries (every 5 min check, fast retry on failure after 300 msec)
- [ ] 	latency is over 3500msec for 2 consecutive checks from 2 Geo regions
- [ ] 	Does the service have an entry on the status page?

### Costs control - SaaS/PaaS/IaaS
- [ ] 	IaaC code cost is estimate before change is applied to Production
- [ ] 	Cloud cost estimate thresholds are set and responsible FinOps team/TeamLead are alerted
- [ ] 	Logs/traces/metrics ingest & retention cost thresholds are set and FinOps team/TeamLead are alerted
- [ ] 	Sampling is apllied to traces to decrease costs
- [ ] 	Filtering is applied to metrics and logs to decrease ingest/storage costs
- [ ] 	Reasonable retention periods are set for logs/metrics/traces
- [ ] 	Old monitoringn data is automatically moved to cheaper storage options over set periods (hot -> warm -> cold storage)

### On-call rotation

Alert escalation path - per technology/tier - e.g. FE/BE/DB:
- [ ] 	Documented
- [ ] 	Configured in Alert Management/Incident managemnt system

on-call Rotation roster:
- [ ] 	Documented, team members know when their turn is
- [ ] 	Configured in on-call scheduling/alerting system


