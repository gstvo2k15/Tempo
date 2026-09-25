# Tempo
Basic repo for tempo PoC with compose v2


## Usage

```bash
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