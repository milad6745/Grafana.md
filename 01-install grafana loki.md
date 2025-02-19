
## add grafana repository


```
helm repo list
NAME                    URL
prometheus-community    https://prometheus-community.github.io/helm-charts
grafana                 https://grafana.github.io/helm-charts
```
```
root@myubuntu:~#helm update

helm search repo grafana
NAME                                            CHART VERSION   APP VERSION     DESCRIPTION
grafana/grafana                                 8.10.0          11.5.1          The leading tool for querying and visualizing t...
grafana/grafana-agent                           0.42.0          v0.42.0         Grafana Agent
grafana/grafana-agent-operator                  0.5.1           0.44.2          A Helm chart for Grafana Agent Operator
grafana/grafana-operator                        v5.16.0         v5.16.0         Helm chart for the Grafana Operator
grafana/grafana-sampling                        1.1.2           v1.5.1          A Helm chart for a layered OTLP tail sampling a...
grafana/alloy                                   0.11.0          v1.6.1          Grafana Alloy
grafana/beyla                                   1.7.2           2.0.2           eBPF-based autoinstrumentation HTTP, HTTP2 and ...
grafana/enterprise-logs                         2.5.0           v1.5.2          Grafana Enterprise Logs
grafana/enterprise-logs-simple                  1.3.0           v1.4.0          DEPRECATED Grafana Enterprise Logs (Simple Scal...
grafana/enterprise-metrics                      1.10.0          v1.7.0          DEPRECATED Grafana Enterprise Metrics
grafana/fluent-bit                              2.6.0           v2.1.0          Uses fluent-bit Loki go plugin for gathering lo...
grafana/k6-operator                             3.11.0          0.0.19          A Helm chart to install the k6-operator
grafana/k8s-monitoring                          2.0.11          2.0.11          Capture all telemetry data from your Kubernetes...
grafana/lgtm-distributed                        2.1.0           ^7.3.9          Umbrella chart for a distributed Loki, Grafana,...
grafana/loki                                    6.27.0          3.4.2           Helm chart for Grafana Loki and Grafana Enterpr...
grafana/loki-canary                             0.14.0          2.9.1           Helm chart for Grafana Loki Canary
grafana/loki-distributed                        0.80.1          2.9.10          Helm chart for Grafana Loki in microservices mode
grafana/loki-simple-scalable                    1.8.11          2.6.1           Helm chart for Grafana Loki in simple, scalable...
grafana/loki-stack                              2.10.2          v2.9.3          Loki: like Prometheus, but for logs.
grafana/meta-monitoring                         1.3.0           0.0.1           A Helm chart for meta monitoring Grafana Loki, ...
grafana/mimir-distributed                       5.6.0           2.15.0          Grafana Mimir
grafana/mimir-openshift-experimental            2.1.0           2.0.0           Grafana Mimir on OpenShift Experiment
grafana/oncall                                  1.15.0          v1.15.0         Developer-friendly incident response with brill...
grafana/phlare                                  0.5.4           0.5.1           🔥 horizontally-scalable, highly-available, mul...
grafana/promtail                                6.16.6          3.0.0           Promtail is an agent which ships the contents o...
grafana/pyroscope                               1.12.0          1.12.0          🔥 horizontally-scalable, highly-available, mul...
grafana/rollout-operator                        0.24.0          v0.24.0         Grafana rollout-operator
grafana/snyk-exporter                           0.1.0           v1.4.1          Prometheus exporter for Snyk.
grafana/synthetic-monitoring-agent              0.6.0           v0.29.3         Grafana's Synthetic Monitoring application. The...
grafana/tempo                                   1.18.2          2.7.1           Grafana Tempo Single Binary Mode
grafana/tempo-distributed                       1.32.1          2.7.1           Grafana Tempo in MicroService mode
grafana/tempo-vulture                           0.7.1           2.6.1           Grafana Tempo Vulture - A tool to monitor Tempo...
prometheus-community/kube-prometheus-stack      69.3.2          v0.80.0         kube-prometheus-stack collects Kubernetes manif...
prometheus-community/prometheus-druid-exporter  1.1.0           v0.11.0         Druid exporter to monitor druid metrics with Pr...
```

## download values loki-stack
```
 helm show values grafana/loki-stack > /root/loki-stack.yml

## change to

cat /root/loki-stack.yml
loki:
  enabled: true

promtail:
  enabled: true

grafana:
  enabled: false
  sidecar:
    datasources:
      label: ""
      labelValue: ""
      enabled: true
      maxLines: 1000
  image:
    tag: 10.3.3
```
