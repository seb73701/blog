---
title: Creating Monitoring Dashboards
description: 
published: true
date: 2023-07-16T12:47:27.349Z
tags: 
editor: markdown
dateCreated: 2023-07-16T12:02:43.486Z
---

# Creating Monitoring Dashboards

> Guidelines for developers

![1_rwqltnq4im7chdv9bozybq.webp](/assets/img/supervision/1_rwqltnq4im7chdv9bozybq.webp)

*Figure 1: Example of a Single Pane of Glass Monitoring Dashboard.*

Recently our teams at Hotels.com™, part of Expedia Group™, started moving from Graphite to an internal metrics platform that is based on Prometheus. We saw this as an opportunity to improve our observability and, among others, we provided a set of simple guidelines to help with the migration.

We believe these guidelines would be useful to the community and hence we share them in this blog post. Some of the examples apply to our tech stack (i.e. Spring Boot, Micrometer, Kubernetes) but the idea is the same for other technologies and libraries.

![0_jzuifkvjxs-s3q1n.webp](/assets/img/supervision/0_jzuifkvjxs-s3q1n.webp)

## Purpose of this Guide
Having meaningful and carefully crafted monitoring dashboards for your services is of utmost importance. The purpose of this guide is to:

- Provide you with a set of handful resources around monitoring
- Promote best practices on monitoring metrics and dashboards
- Help you create [Grafana](https://grafana.com/) dashboards based on [Prometheus](https://prometheus.io/) metrics

> If you want to learn more about monitoring and best practices, we suggest you to read the following resources by Google:
>
> Site Reliability Engineering, How Google runs production systems ([Chapter 6 — Monitoring Distributed Systems](https://landing.google.com/sre/sre-book/chapters/monitoring-distributed-systems/))
> 
> The Site Reliability Workbook, Practical ways to implement SRE ([Chapter 4 — Monitoring](https://landing.google.com/sre/workbook/chapters/monitoring/))

## Principles
Below is a non-exhaustive list of principles to have in mind in the context of observability which also apply to dashboards:

- Keep it **simple**, avoid creating complex dashboards that you will never use or alerts that can trigger false-positive notifications.
- Keep it **consistent**, use consistent and meaningful names in your dashboards and alerts.
- Use **logs**, **metrics**, and **traces** wisely and in conjunction with each other.
- Avoid high-cardinality metrics.
- **Avoid complex** and **slow** queries in your dashboards.

## What to Monitor
### Core Metrics
As a first set of metrics, you should be looking into monitoring the [4 golden signals](https://landing.google.com/sre/sre-book/chapters/monitoring-distributed-systems/#xref_monitoring_golden-signals) as these are defined by Google, or follow the [RED method](https://www.weave.works/blog/the-red-method-key-metrics-for-microservices-architecture/) which is more relevant to micro-services.

### Latency (Duration)
This could take the form of percentiles (e.g. p90, p99). Be aware of failed requests which would result in misleading calculations.

### Traffic (Rate)
An example of this would be the number of requests per second (RPS).

### Errors
This will depend on what you consider as an error for your service or system. A typical metric could be the rate of non-2XX status code responses.

### Saturation
Saturation shows how overloaded your service or system is. This could be monitoring the number of elements in a queue. You may also want to look into utilisation which reflects how busy the service is. An example of that is monitoring the busy threads.

### Business Metrics
Ideally, you need to discuss and decide on this set of metrics with your product owner as they are based on business needs. Business metrics could be custom metrics reported by one or more services.

Indicative examples are listed below:

- A team responsible for sign-ins would need to report metrics for sign-in attempts, failed attempts due to invalid passwords, or even sign-ins coming from different channels but still hitting the same endpoint.
- A team owning the autocomplete functionality across multiple brands would need to monitor the number of requests and error rates per brand.

### Dependencies Metrics
In a micro-services architecture, there could be many external calls from your service to other services. These calls are usually wrapped with Hystrix or other Circuit Breaker libraries. Monitoring core metrics (traffic, latencies, errors) for these calls is very important.

### Connection Pools & Thread Pools Metrics
Having a dashboard that displays metrics for Tomcat threads, Circuit Breaker thread pools and HTTP client connection pools for 3rd party calls is useful.

### JVM Metrics
Useful metrics for JVM applications include memory and CPU, GC, or even memory pools. We suggest re-using the [JVM (Micrometer) Grafana dashboard](https://grafana.com/grafana/dashboards/4701).

### Infrastructure Metrics
Many services rely on infrastructures such as a cache, a database, or a queue. Even if your team does not own these components, monitoring them can help you identify the root cause of an issue. Although the [4 golden signals](https://landing.google.com/sre/sre-book/chapters/monitoring-distributed-systems/#xref_monitoring_golden-signals) apply to most infrastructure systems, these systems can also have extra characteristics you need to monitor (e.g. the size of the queue or cache hits/misses).

### Platform Metrics
In addition to infrastructure metrics you may need to monitor Platform metrics, such as ones provided by Kubernetes or by the Service Mesh (e.g. Istio). Usually incident response and SRE teams look into such dashboards to have the big picture and to achieve faster Mean Time to Detect (MTTD) and Mean Time To Recover (MTTR).

![1_ghn3ms7by0s8-hgy8s5skw.webp](/assets/img/supervision/1_ghn3ms7by0s8-hgy8s5skw.webp)

## Prometheus Best Practices
The open-source community has come up with a set of best practices on [metric names](https://prometheus.io/docs/practices/naming/#metric-names) and [labels](https://prometheus.io/docs/practices/naming/#labels) which we encourage you to follow.

Be super careful with high-cardinality metrics. As stated in the docs:

> Every unique combination of key-value label pairs represents a new time series, which can dramatically increase the amount of data stored. Do not use labels to store dimensions with high cardinality (many different label values), such as user IDs, email addresses, or other unbounded sets of values.

Popular metrics libraries may have mechanisms in place to prevent this issue. For example, Micrometer provides the `maximumAllowableTags` method through its [Meter Filters](https://micrometer.io/docs/concepts#_meter_filters). Recent versions of Spring Boot Actuator use this by default for URI tags; they expose the `management.metrics.web.client.max-uri-tags` property with a default value of 100 (you may need to decrease that value though). If your library doesn't provide this out-of-the-box you will need to implement this logic.

Let’s now look at practical examples you can re-use. Before we dive deep into queries, understanding the Prometheus format is crucial.

## Understanding the Prometheus Format
If you hit the `/prometheus` endpoint under which your application exposes Prometheus metrics, you will see a set of metrics:

```
# HELP resilience4j_circuitbreaker_calls_seconds Total number of successful calls
# TYPE resilience4j_circuitbreaker_calls_seconds summary
resilience4j_circuitbreaker_calls_seconds{app="test-app",kind="successful",name="greetings",quantile="0.95",} 0.738197504
resilience4j_circuitbreaker_calls_seconds{app="test-app",kind="successful",name="greetings",quantile="0.99",} 0.738197504
[...]
# HELP http_server_requests_seconds  
# TYPE http_server_requests_seconds summary
http_server_requests_seconds{app="test-app",client="client1",exception="None",method="GET",status="200",uri="/api/v1/hello-world",quantile="0.95",} 0.771751936
http_server_requests_seconds_count{app="test-app",client="client1",exception="None",method="GET",status="200",uri="/api/v1/hello-world",} 1.0
http_server_requests_seconds_count{app="test-app",client="client2",exception="None",method="GET",status="200",uri="/api/v1/hello-world",} 1.0
[...]
```
source : https://gist.github.com/nikos912000/6604b1813cbc8bc924bd256b502423bb#file-prometheus_endpoint_metrics


*Figure 2: Prometheus metrics exposed by Spring Boot Actuator.*

Taking the last two lines as an example, the name of the metric is `http_server_requests_seconds_count` and they both contain a set of labels such as the application name `app`, the endpoint `uri`, etc. In this case, the only difference is the client.

This is a representation of a single metric across multiple dimensions, by using labels. Having these multiple dimensions allows us to run powerful queries that could span across multiple URLs, AWS regions, and even across different applications.

## Queries
Now that we have a basic understanding of the metrics format we can look into useful queries. This section includes very basic examples but you can use them as a starting point.

### Rate
#### RPS — Overall

The following query shows the Requests Per Second (RPS) across all endpoints:

```
sum(rate(http_server_requests_seconds_count{app="test-app"}[1m]))
```
source : https://gist.github.com/nikos912000/855e0ed1ff2f477e7bd778b1e5640495#file-rps-overall

*Figure 3: Query for Requests Per Second across all endpoints.*

- `http_server_requests_seconds_count` stores the count of HTTP requests.
- `app` is a label that reflects the name of the application. You can use a regex and the ['=~' operator](https://prometheus.io/docs/prometheus/latest/querying/basics/#instant-vector-selectors) for a set of applications.
- We append the time selector `[1m]` which translates the instant vector into a range vector (over the last minute).
- Up to this point, we have a range vector which we need to transform into an instant vector in order for it to be displayed. We do this by applying the [rate function](https://prometheus.io/docs/prometheus/latest/querying/functions/#rate) which shows per second increase.
- Finally, we aggregate the results using the `sum` [aggregation operator](https://prometheus.io/docs/prometheus/latest/querying/operators/#aggregation-operators).

![1_yuat0sermlx_gfhs_0x3vq.webp](/assets/img/supervision/1_yuat0sermlx_gfhs_0x3vq.webp)
*Figure 4: Visualising the overall RPS.*

If you want to display a single number, you can use the [Singlestat visualisation](https://grafana.com/grafana/plugins/singlestat) (or the [Stat panel](https://grafana.com/docs/grafana/latest/panels/visualizations/stat-panel/) in recent versions of Grafana).

![1_xjpxd0gypyhe8u4qty0khw.webp](/assets/img/supervision/1_xjpxd0gypyhe8u4qty0khw.webp)
*Figure 5: Singlestat visualisation of the overall RPS.*

#### RPS — Aggregations

Often you need to aggregate results per label. For example, plot the RPS per Kubernetes pod, per endpoint, or even per client.

To show the RPS per pod:

```
sum by (pod_name) (rate (http_server_requests_seconds_count{app="test-app"}[1m]))
```
source : https://gist.github.com/nikos912000/1f61e452248ae78b4879ba85dba47a7e#file-rps-aggregations

*Figure 6: Query for Requests Per Second by Kubernetes pod.*

![1_a4lrjrsvw7_dexjgb9vdbg.webp](/assets/img/supervision/1_a4lrjrsvw7_dexjgb9vdbg.webp)

*Figure 7: Visualising the RPS by Kubernetes pod.*

For the RPS per endpoint and client you can use the uri and client labels respectively. In these cases, as mentioned earlier in this guide, you need to be mindful of high-cardinality issues.

### Duration
To show the latency (e.g. p99) per endpoint you can use the following query:

```
max by (uri)(http_server_requests_seconds{app="test-app", quantile="0.99"})
```
source:  https://gist.github.com/nikos912000/ac9855f974a6a945b9628e87af75a306#file-duration-p99

*Figure 8: Query for the duration/latency per uri.*

- `http_server_requests_seconds` stores the latency of HTTP requests.
- `quantile=0.99` gives the p99. You can read more about [quantiles](https://prometheus.io/docs/practices/histograms/#quantiles).
- Finally, we aggregate the results per endpoint using the `max` aggregator.

![1_6jznceuetxdvl3qqlfi1hg.webp](/assets/img/supervision/1_6jznceuetxdvl3qqlfi1hg.webp)

*Figure 9: Visualising the duration/latency per uri.*

Note that you can calculate quantiles from both [histograms and summaries](https://prometheus.io/docs/practices/histograms/).

> Quantiles may not be reported by default by your application. For example, for Spring Boot applications you need to set the `management.metrics.distribution.percentiles` for this. We recommend reporting the p50, p75, p90, p95, p99, p999 percentiles and defining a variable for these in your dashboards.
>
> If you want to include or exclude particular endpoints you can do this with the uri label. For instance `uri=~"/api/v1/.*"` will only plot endpoints under the `/api/v1/` path, while `uri!~"/swagger.*"` will exclude the Swagger endpoints.
>
> Failed requests are not representative examples of latency as they could fail fast (e.g. 500) or take a lot of time to complete if a timeout is not in place or is mis-configured. We recommend visualising latencies for successful requests and, if needed, having another panel for tracking latencies for failed requests.

### Errors
The simplest way to visualise your errors is by using a Stat panel, similar to Figure 5.

```
sum (rate(http_server_requests_seconds_count{app="test-app", status=~"5.."}[1m]))
```
source : https://gist.github.com/nikos912000/8beb6ec69d4efc789c9b784fe25e81ee#file-errors-5xx

*Figure 10: Query for error rates (5XXs).*

#### Success Rates

A more descriptive way would be to visualise success rates per endpoint:

```
100 * sum by (uri) (rate(http_server_requests_seconds_count{app="test-app",status="200"}[1h])) / sum by (uri) (rate (http_server_requests_seconds_count{app="test-app",status=~".+"}[1h]))

```
source : https://gist.github.com/nikos912000/b16ab782b2d51877b276cca0313244ac#file-success_rates_200

*Figure 11: Query for success rates (200s).*

![1_4prn5q0dpmga0fqu5qmmba.webp](/assets/img/supervision/1_4prn5q0dpmga0fqu5qmmba.webp)

*Figure 12: Visualising success rates (200s) per uri.*

### Dependencies
It is important to be able to identify issues with your dependencies. The same signals can be used to monitor such calls. You can get these metrics from Circuit Breaker libraries such as Hystrix or Resilience4J.

To check which metrics are exposed by your Circuit Breaker you can either go through the documentation or hit your `/prometheus` endpoint.

> Hystrix uses keys (in particular command keys and command group keys) to identify and group commands. These are available as `key` and `group` labels when using the Hystrix metrics publisher. The result of the call is stored in the `event` label.
> 
> Resilience4J exposes the `name`, `state`, and `kind` labels as [documented](https://resilience4j.readme.io/docs/micrometer). The `name` is used to identify the call while the `kind` is the result.

#### RPS

The following queries return the RPS per Kubernetes pod for a selected key/name:

#### Hystrix

```
sum by (pod_name,event) (rate(hystrix_execution_total{app="test-app",key="$key"}[2m]))
```
source : https://gist.github.com/nikos912000/2358b819159281e7df8856a1940e3849#file-rps_hystrix

*Figure 13: Query for the RPS per pod for a selected Hystrix key.*

#### Resilience4J

```
sum by (pod_name, kind) (rate(resilience4j_circuitbreaker_calls_seconds_count{app="test-app",name="$name"}[2m]))
```
source : https://gist.github.com/nikos912000/9aa5333a552c4bf332d32db221525b26#file-rps_resilience4j

*Figure 14: Query for the RPS per pod for a selected Resilience4J name.*

### Latency
To plot the latency for a selected quantile (e.g. 0.99):

#### Hystrix

```
max by(pod_name)(hystrix_latency_execution_seconds{app="test-app",key="$key",quantile="$quantile"})
```
source : https://gist.github.com/nikos912000/94eed816d9172119f88a360bc151eb04#file-latency_hystrix

*Figure 15: Query for the latency of a Hystrix call for a selected quantile.*

#### Resilience4J

```
max by(pod_name,kind)(resilience4j_circuitbreaker_calls_seconds{app="test-app",name="$name",quantile="$quantile"})
```
source : https://gist.github.com/nikos912000/94eed816d9172119f88a360bc151eb04#file-latency_hystrix

*Figure 16: Query for the latency of a Resilience4J call for a selected quantile.*

### Errors

Finally, for errors:

#### Hystrix

```
sum by (pod_name) (rate (hystrix_execution_total{app="test-app",key="$key",event!="success"}[2m])) / sum by (pod_name) (rate (hystrix_execution_total{app="test-app",key="$key"}[2m]))
```
source : https://gist.github.com/nikos912000/152ef17d9044b46ae943d21c7814fc88#file-errors_hystrix

*Figure 17: Query for the error rate of a selected Hystrix key.*

#### Resilience4J

```
sum by (pod_name) (rate (resilience4j_circuitbreaker_failure_rate{app="test-app",name="$name"}[2m]))
```
source : https://gist.github.com/nikos912000/87c39c6895a007e975737de878f72044#file-errors_resilience4j

*Figure 18: Query for the error rate of a selected Resilience4J name.*

Building panels for dependencies manually is time-consuming. We recommend using Grafana’s [Repeat panel feature](https://grafana.com/docs/grafana/latest/variables/repeat-panels-or-rows/).

For this you first need to define a variable for your keys/names:

![1_isct89oddr-2ocyxud_iva.webp](/assets/img/supervision/1_isct89oddr-2ocyxud_iva.webp)

*Figure 19: Defining a variable for your Hystrix keys.*

You can then **select a single value for the key/name from the dropdown list**, create one panel, and use the `Repeating` option under the `General` settings of your panel.

Once you click on the `All` option Grafana will render multiple panels, one for each dependency for you.

![1_fmnw9qgitphjmwp9sawj8a.webp](/assets/img/supervision/1_fmnw9qgitphjmwp9sawj8a.webp)

## Dashboard Guidelines
We encourage our teams to create their dashboards inside a [folder](https://grafana.com/docs/grafana/latest/dashboards/dashboard_folders/) and to use the same folder at least for dashboards related to the same service. The name of the folder could match the one of the project, or reflect the pillar, family name, etc.

We also strongly recommend the use of tags. Tags are helpful when [searching for dashboards](https://grafana.com/docs/grafana/latest/reference/search/#filter-by-tag-s) and allow you to [add links to other dashboards or URLs](https://grafana.com/docs/grafana/latest/linking/dashboard-links/).

The taxonomy depends on many factors, including the structure of a company but the following categories are usually company-agnostic:

- **Business area** (for us that would be “search”, “lodging”, etc.)
- **Family or tech pillar name**
- **Technology name** (e.g. micrometer, dropwizard, elasticache)
- **Service/Infrastructure name**

Recent Grafana versions support [Dashboard Links, Panel Links, and Data Links](https://grafana.com/blog/2019/12/10/pro-tips-dashboard-navigation-using-links/). These could be either links to other dashboards or links to useful URLs. They rely on tags and once links have been created they will be available on your dashboard’s page.

![1_ux0lxzv9ntysjnmx1umfow.webp](/assets/img/supervision/1_ux0lxzv9ntysjnmx1umfow.webp)

*Figure 20: Dashboard Links to other dashboards and external monitoring systems.*

On top of Dashboard Links we suggest using [Panel Links](https://grafana.com/docs/grafana/latest/linking/panel-links/). These could be links to monitoring systems used for logging (e.g. Splunk) or distributed tracing (e.g. [Haystack](https://expediadotcom.github.io/haystack/)) and redirect to a particular search associated with the service and the panel.

![1_konirmdsnosfcpsizg_gjq.webp](/assets/img/supervision/1_konirmdsnosfcpsizg_gjq.webp)

*Figure 21: Panel Links to other dashboards and external monitoring systems.*

[Templates](https://grafana.com/docs/grafana/latest/variables/templates-and-variables/) is another key feature of Grafana which allows you to avoid duplication by using variables instead of hard-coded values. We have seen that feature in the queries we used earlier for Hystrix and Resilience4J metrics. You can define variables for the datasource, the application name, the Kubernetes pods, or even the percentiles you want to plot metrics for. The values of these variables will show up as dropdown lists, and you can use the selected values in your queries.

Last but not least, [annotations](https://grafana.com/docs/grafana/latest/dashboards/annotations/) enable you to mark points with events. This is handy for correlating metrics with events such as deployments or A/B tests and we highly recommend using it.


## Conclusion
In this article we went through best practices on monitoring metrics and dashboards and showed you how to create Grafana dashboards based on Prometheus metrics. These examples can be used as a starting point to craft more complex queries and more visualisations. However, always keep in mind that less is more, and simple is better than complex!
 
source : https://medium.com/expedia-group-tech/creating-monitoring-dashboards-1f3fbe0ae1ac