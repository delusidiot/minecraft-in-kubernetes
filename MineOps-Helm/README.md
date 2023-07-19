# MineOps Minecraft server

MineOps 프로젝트의 Minecraft관련 Helm파일을 모아놓은 repository입니다.

단순히 마인크래프트만 실행한다면 Minecraft Helm만 설정하면 됩니다.

아래 script는 모두 default namespace로 설정한 예시입니다.

## minecraft

itzg의 minecraft server helm을 참고하여 생성한 helm입니다.

```shell
helm install mine-release ./minecraft -f ./minecraft/values.yaml
```

nodeSelector가 지정되어 있으니 원하지 않는다면 install시 비워주세요.

```shell
helm install mine-release ./minecraft -f ./minecraft/values.yaml --set nodeSelector={}
```

서버 데이터 백업을 위해 `mc-backup`을 사용합니다. 해당 기능은 서버 리소스를 많이 사용하기 때문에 기본 상태는 `false`입니다.

```shell
helm install mine-release ./minecraft -f ./minecraft/values.yaml --set mcbackup.enabled=true
```

데이터 백업을 위한 pvc는 aws eks의 기본 storageClass (gp2)를 이용합니다.

monitoring을 위해 promethus에 metric정보를 보내기위해 `mc-monitor`를 사용합니다.

- `minecraft_status_healthy`
- `minecraft_status_response_time_seconds`
- `minecraft_status_players_online_count`
- `minecraft_status_players_max_count`

# Minecraft logs

Minecraft 로그를 kafka로 받기 위해서 minecraft -> kafka -> fluent-bit 순으로 실행해야 합니다. kafka가 제대로 Running상태로 들어갔는지 확인후 fluent-bit를 실행해야합니다. kafka topic을 생성해야하는데 fluent-bit를 먼저 실행하면 생성되지 않기 때문입니다. kafka topic은 `minelogs`입니다.

> 해당 문제는 Kafka를 사용하고 있지 않아 미루고 있습니다.

> kafka를 install한 뒤 pvc가 생성됩니다. kafka는 statefulset이라 해당 PVC는 삭제되지 않고 남아 있습니다. Kafka를 uninstall 한뒤 다시 install 했을때 제대로 topic이 쌓이지 않는 이유는 높은 확률로 이전 pvc가 남아있어서 topic이 제대로 생성되지 않아서 그렇습니다. 해당 문제를 찾는 작업은 현재 Kafka를 사용하지 않아 미루고 있습니다.

## kafka

bitnami의 kafka helm을 그대로 사용합니다.

```shell
heml install kafka ./kafka -f ./kafka/values.yaml
```

## fluent-bit

fluent의 fluent-bit helm에서 조금 변경한 Chart입니다.
마인크래프트 pvc에서 log를 가지고 오기 위해 마인크래프트 helm과 같은 node에서 실행해야 합니다. 그래서 nodeSelector가 마인크래프트와 같은 `mine-server`로 지정되어 있습니다.

> 해당 부분을 지정하지 않으면 모든 노드에 fluent-bit가 생성되며 생성된 pod중에서도 마인크래프트가 install 되어 있는 node에서만 Running됩니다. 즉, 서버 리소스가 낭비됩니다.

```shell
heml install fluent-bit ./fluent-bit -f ./fluent-bit/values.yaml
```