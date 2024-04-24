# aws-mock

AWS サービスのモックを DockerCompose で提供するあれこれの実装例

## LocalStack 概要

- LocalStack は AWS サービスを Docker Compose などでローカル実行させてくれるサービス
- 有償版（Pro と呼ばれるもの）と無償版がある
- 有償版
  - https://www.localstack.cloud/pricing
  - Pro でしか使えないサービスがある（Cognito など）
- 無償版
  - データが永続化されない。
  - `/etc/localstack/init/ready.d` に初期化スクリプトを配置できるので活用する

## 提供対象 AWS サービス対応表

https://docs.localstack.cloud/user-guide/aws/feature-coverage/

## docker-compose.yml

[公式サンプル](https://github.com/localstack/localstack/blob/master/docker-compose.yml)

```yml
version: "3.8"

services:
  localstack:
    container_name: "${LOCALSTACK_DOCKER_NAME:-localstack-main}"
    image: localstack/localstack
    ports:
      - "127.0.0.1:4566:4566" # LocalStack Gateway
      - "127.0.0.1:4510-4559:4510-4559" # external services port range
    environment:
      # LocalStack configuration: https://docs.localstack.cloud/references/configuration/
      - DEBUG=${DEBUG:-0}
    volumes:
      - "${LOCALSTACK_VOLUME_DIR:-./volume}:/var/lib/localstack"
      - "/var/run/docker.sock:/var/run/docker.sock"
```

## 動作確認

```sh
curl -s localhost:4566/_localstack/init | jq .
```

---

## moto 概要

## moto 対応サービス

https://docs.getmoto.org/en/latest/docs/services/index.html
