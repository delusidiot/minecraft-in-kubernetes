# Monitoring and Logging

## Monitoring

### Prometheus & Grafana

- Minecraft 서버를 먼저 실행하고 마인크래프트와 같은 네임스페이스에서 install해야 마인크래프트 관련 notify를 받을 수 있습니다. (문서는 default namespace로 가정합니다.)

- 사용할 slack webhook url을 넣어야 slack에서 알람을 받을 수 있습니다.

```shell
helm install prometheus ./kube-prometheus-stack -f ./kube-prometheus-stack/values.yaml --set alertmanager.config.global.slack_api_url="[slack_webhook_url]"
```

### values 변경점

- `additionalPrometheusRulesMap` 수정

- `additionalScrapeConfigs` 수정

### Grafana Default ID/PW
> admin / prom-operator

### Delete
```shell
helm uninstall prometheus
```

## Logging

- EleasticSearch -> kibana -> fluent-bit 순으로 설정

- fluent-bit의 namespace가 logging으로 고정되어 있기 때문에 logging ns를 생성해야 합니다.

```shell
kubectl create ns logging
```

### ElasticSearch

```shell
helm install -n logging elasticsearch ./elasticsearch -f ./elasticsearch/values.yaml
```

### kibana

```shell
helm install -n logging kibana ./kibana -f ./kibana/values.yaml
```

## fluent-bit

Fluent bit는 해당 폴더 [README.md](./fluent-bit/README.md) 파일을 참조

### Delete
```shell
helm uninstall elasticsearch
helm uninstall kibana
```