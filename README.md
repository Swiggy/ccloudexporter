# Prometheus exporter for Confluent Cloud Metrics API

A simple prometheus exporter that can be used to extract metrics from [Confluent Cloud Metric API](https://docs.confluent.io/current/cloud/metrics-api.html).
By default, the exporter will be exposing the metrics on [port 2112](http://localhost:2112)
To use the exporter, the following environment variables need to be specified:

* `CCLOUD_USER`: Your Confluent Cloud login, or the API Key created with `ccloud api-key create --resource cloud`
* `CCLOUD_PASSWORD`: Your Confluent Cloud password, or the API Key Secret when created with `ccloud api-key create --resource cloud`

`CCLOUD_USER` and `CCLOUD_PASSWORD` environment variables will be used to invoke the https://api.telemetry.confluent.cloud endpoint.

## Usage
```
./ccloudexporter -cluster <cluster_id>
````

### Options

```
Usage of ./ccloudexporter:
  -cluster string
    	Cluster ID to fetch metric for. If not specified, the environment variable CCLOUD_CLUSTER will be used
  -listener string
    	Listener for the HTTP interface (default ":2112")
  -delay int
    	Delay, in seconds, to fetch the metrics. By default set to 120, this, in order to avoid temporary data points. (default 120)
  -endpoint string
    	Base URL for the Metric API (default "https://api.telemetry.confluent.cloud/")
  -granularity string
    	Granularity for the metrics query, by default set to 1 minutes (default "PT1M")
  -no-timestamp
    	Do not propagate the timestamp from the the metrics API to prometheus
  -timeout int
    	Timeout, in second, to use for all REST call with the Metric API (default 60)
```

## Examples

### Building and executing
```
go get github.com/Dabz/ccloudexporter/cmd/ccloudexporter
go install github.com/Dabz/ccloudexporter/cmd/ccloudexporter
export CCLOUD_USER=toto@confluent.io
export CCLOUD_PASSWORD=totopassword
./ccloudexporter -cluster lkc-abc123
```

### Using docker
```
docker run \
  -e CCLOUD_USER=$CCLOUD_USER \
  -e CCLOUD_PASSWORD=$CCLOUD_PASSWORD
  -p 2112:2112
  dabz/ccloudexporter:latest -cluster lkc-abc123
```

### Using docker-compose
```
export CCLOUD_USER=toto@confluent.io
export CCLOUD_PASSWORD=totopassword
export CCLOUD_CLUSTER=lkc-abc123
docker-compose up -d
```

## How to build
```
go get github.com/Dabz/ccloudexporter/cmd/ccloudexporter
```

## Grafana
A Grafana dashboard is provided in [./grafana/](./grafana) folder.

![Grafana Screenshot](./grafana/grafana.png)
