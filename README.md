# Tempo
Basic repo for tempo PoC with compose v2


## Usage

```bash

mkdir -p /root/otel

curl -L \
  -o /root/otel/opentelemetry-javaagent.jar \
  https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar


docker compose -f /root/compose.yml up -d --force-recreate tempo otel-collector


docker compose -f /root/compose.yml ps -a

NAME             IMAGE                                         COMMAND                  SERVICE          CREATED              STATUS              PORTS
grafana          grafana/grafana:latest                        "/run.sh"                grafana          2 hours ago          Up 17 minutes       0.0.0.0:3000->3000/tcp, [::]:3000->3000/tcp
loki             grafana/loki:latest                           "/usr/bin/loki -conf…"   loki             2 hours ago          Up 2 hours          0.0.0.0:3100->3100/tcp, [::]:3100->3100/tcp
otel-collector   otel/opentelemetry-collector-contrib:latest   "/otelcol-contrib --…"   otel-collector   About a minute ago   Up About a minute   0.0.0.0:4317-4318->4317-4318/tcp, [::]:4317-4318->4317-4318/tcp, 0.0.0.0:8889->8889/tcp, [::]:8889->8889/tcp, 55679/tcp
prometheus       prom/prometheus:latest                        "/bin/prometheus --c…"   prometheus       2 hours ago          Up 17 minutes       0.0.0.0:9090->9090/tcp, [::]:9090->9090/tcp
tempo            grafana/tempo:latest                          "/tempo -config.file…"   tempo            3 seconds ago        Up 2 seconds        0.0.0.0:3200->3200/tcp, [::]:3200->3200/tcp
tomcat           tomcat:10-jdk21                               "catalina.sh run"        tomcat           2 hours ago          Up 2 hours          0.0.0.0:8080->8080/tcp, [::]:8080->8080/tcp
```

## Check deployment

```bash
curl http://localhost:3000/api/health
{
  "database": "ok",
  "version": "13.2.2",
  "commit": "1bea008f7e4e858b6824c9e364d608bd4d10b13a"
}

curl http://localhost:9090/-/ready
Prometheus Server is Ready.


curl http://localhost:3100/ready
ready


curl http://localhost:3200/ready
ready


curl -s http://localhost:8889/metrics | grep -i tomcat | head

jvm_class_count{instance="46428ae2-36fc-4820-9559-2e3dc3bf180e",job="tomcat-demo",otel_scope_name="io.opentelemetry.runtime-telemetry-java8",otel_scope_schema_url="",otel_scope_version="2.31.1-alpha"} 7437
jvm_class_loaded_total{instance="46428ae2-36fc-4820-9559-2e3dc3bf180e",job="tomcat-demo",otel_scope_name="io.opentelemetry.runtime-telemetry-java8",otel_scope_schema_url="",otel_scope_version="2.31.1-alpha"} 7437
jvm_class_unloaded_total{instance="46428ae2-36fc-4820-9559-2e3dc3bf180e",job="tomcat-demo",otel_scope_name="io.opentelemetry.runtime-telemetry-java8",otel_scope_schema_url="",otel_scope_version="2.31.1-alpha"} 0
jvm_cpu_count{instance="46428ae2-36fc-4820-9559-2e3dc3bf180e",job="tomcat-demo",otel_scope_name="io.opentelemetry.runtime-telemetry-java8",otel_scope_schema_url="",otel_scope_version="2.31.1-alpha"} 2
jvm_cpu_recent_utilization_ratio{instance="46428ae2-36fc-4820-9559-2e3dc3bf180e",job="tomcat-demo",otel_scope_name="io.opentelemetry.runtime-telemetry-java8",otel_scope_schema_url="",otel_scope_version="2.31.1-alpha"} 0.0020161290322580645
jvm_cpu_time_seconds_total{instance="46428ae2-36fc-4820-9559-2e3dc3bf180e",job="tomcat-demo",otel_scope_name="io.opentelemetry.runtime-telemetry-java8",otel_scope_schema_url="",otel_scope_version="2.31.1-alpha"} 10.96
jvm_gc_duration_seconds_bucket{instance="46428ae2-36fc-4820-9559-2e3dc3bf180e",job="tomcat-demo",jvm_gc_action="end of major GC",jvm_gc_name="MarkSweepCompact",otel_scope_name="io.opentelemetry.runtime-telemetry-java8",otel_scope_schema_url="",otel_scope_version="2.31.1-alpha",le="0.01"} 0
jvm_gc_duration_seconds_bucket{instance="46428ae2-36fc-4820-9559-2e3dc3bf180e",job="tomcat-demo",jvm_gc_action="end of major GC",jvm_gc_name="MarkSweepCompact",otel_scope_name="io.opentelemetry.runtime-telemetry-java8",otel_scope_schema_url="",otel_scope_version="2.31.1-alpha",le="0.1"} 1
jvm_gc_duration_seconds_bucket{instance="46428ae2-36fc-4820-9559-2e3dc3bf180e",job="tomcat-demo",jvm_gc_action="end of major GC",jvm_gc_name="MarkSweepCompact",otel_scope_name="io.opentelemetry.runtime-telemetry-java8",otel_scope_schema_url="",otel_scope_version="2.31.1-alpha",le="1"} 1
jvm_gc_duration_seconds_bucket{instance="46428ae2-36fc-4820-9559-2e3dc3bf180e",job="tomcat-demo",jvm_gc_action="end of major GC",jvm_gc_name="MarkSweepCompact",otel_scope_name="io.opentelemetry.runtime-telemetry-java8",otel_scope_schema_url="",otel_scope_version="2.31.1-alpha",le="10"} 1
```


## Add datasources

Enter to grafana using `http://YOUR_IP:3000/login` and admin/admin login

Prometheus
URL: http://prometheus:9090

Loki
URL: http://loki:3100

Tempo
URL: http://tempo:3200

