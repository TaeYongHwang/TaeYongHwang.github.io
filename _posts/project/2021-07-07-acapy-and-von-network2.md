---
title: " 도커를 이용한 에어리즈 프레임워크 (ACA-PY)와 Von Network 연동 (Hyperledger Aries Development environment setting using Docker)"
date: 2021-07-07
toc: true
toc_sticky: true
categories: Project
---


# Tails Server 연결
## 실행
- https://github.com/bcgov/indy-tails-server

```shell
git clone https://github.com/bcgov/indy-tails-server.git
cd indy-tails-server/
./docker/manage start

./docker/manage stop
```
- 디폴트는 6543 포트를 통해 접근 가능

## ngrok 실행
- ACA-PY 연결 시, tails 서버에 public하게 접근 가능한 URL이 필요 (VC 폐기 처리를 위해)
- https://github.com/hyperledger/aries-cloudagent-python/blob/main/docs/GettingStartedAriesDev/CredentialRevocation.md
```shell
ngrok http 6543
```

# ACA-PY 연동
## 설치
```shell
git clone https://github.com/hyperledger/aries-cloudagent-python.git
cd aries-cloudagent-python
docker build -t acapy -f ./docker/Dockerfile.run .
```

## 실행
```shell
docker run -it --rm acapy --help #도움말

# aries-cloudagent-python 의 루트 디렉토리에서 실행
# localhost:8001을 통해 제공하는 Swagger UI에 접속 가능
PORTS="8000:8000 8001:8001" \
scripts/run_docker start \
--storage-type indy \
--inbound-transport http 0.0.0.0 8000 \
--outbound-transport http \
--seed 000000000000000000000000000Agent \
--genesis-url http://dev.greenlight.bcovrin.vonx.io/genesis \
--wallet-type indy \
--wallet-key walletKey \
--wallet-name walletName \
--recreate-wallet \
--replace-public-did \
--auto-provision \
--endpoint http://localhost:8000 \
--admin-insecure-mode \
--admin 0.0.0.0 8001 \
--tails-server-base-url <위 과정을 통해 켜둔 ngrok url> \
```

# Von-Network 연동
## 설치
```shell
git clone https://github.com/bcgov/von-network
cd von-network
./manage build
```

## 실행
```shell
./manage start

docker ps 
```
- localhost:9000 을 통해 web GUI 접근 가능



## 중지
```shell
./manage stop
```

   
Reference.
- https://github.com/hyperledger/aries-cloudagent-python/blob/main/DevReadMe.md#docker
- https://github.com/bcgov/von-network/blob/master/docs/UsingVONNetwork.md
- https://medium.com/@jiachuan.li/setup-hyperledger-indy-and-aries-cloud-agent-development-environment-9051e50fc108


