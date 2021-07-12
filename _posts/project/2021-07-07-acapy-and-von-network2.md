---
title: " 도커를 이용한 에어리즈 프레임워크 (ACA-PY)와 Von Network 연동"
date: 2021-07-07
toc: true
toc_sticky: true
categories: Project
---

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
--admin 0.0.0.0 8001
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


