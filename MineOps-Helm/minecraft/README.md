# Minecraft

[Minecraft](https://minecraft.net/)

## Introduction

해당 Helm chart는 마인크래프트 서버를 실행하는 chart입니다.
- 마인크래프트 서버를 실행
- 서버 운영진의 관리를 위한 RCON-web 서버 실행

## Installing the Chart

Chart를 설치하기전 [Minecraft EULA](https://account.mojang.com/documents/minecraft_eula) 라이센스를 확인하세요.

```shell
helm install -n [namespace] minecraft [minecraft helm path] -f [minecraft helm path]/values.yaml
```

기본적인 옵션은 values.yaml파일에 정의되어 있습니다.
Rcon Web을 사용하시는 경우 values.yaml파일에서 `minecraftServer.rcon.web`의 `USERNAME/PASSWORD`를 확인하세요.

- `minecraftServer.rcon.enable` : rcon web을 사용 true/false
- `minecraftServer.rcon.web.enable` : rcon web을 사용여부 true/false
- `mcbackup.enabled` : minecraft world data backup 사용여부 true/false

## Uninstalling the Chart

```shell
helm delete -n [namespace] minecraft
```
Chart와 연결된 모든 Kubernetes구성요소를 삭제합니다.

## Persistence

[itzg/minecraft-server](https://hub.docker.com/r/itzg/minecraft-server/) 이미지는 저장된 게임과 모드는 `/data`에 저장합니다.
persistence.dataDir.enabled이 true로 설정되면 PVC가 생성됩니다.
false로 설정되어 있으면 emptyDir Volume이 생성되며 Pod가 노드에 할당될 때 world가 생성되어 Pod가 실행되는 동안 존재합니다. 만일 Pod가 제거된다면 영원히 삭제됩니다.
