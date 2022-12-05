# Easy Monitoring in k8s

Guide from https://grafana.com/docs/agent/latest/operator/helm-getting-started/

## Requirements

- helm

## Commands

kubectl create namespace monitoring

helm -n monitoring install grafana grafana/grafana

kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

helm install -n monitoring promtail grafana/promtail

helm install -n monitoring loki grafana/loki-stack

## Hand actions

On kubectl, edit promtail to redirect logs to loki. In spec.template.spec.containers.args add

- -client.url=http://loki.monitoring.svc.cluster.local:3100/loki/api/v1/push

---

On grafana, add Loki as a Data Source. HTTP URL must be http://loki.monitoring.svc.cluster.local:3100.
