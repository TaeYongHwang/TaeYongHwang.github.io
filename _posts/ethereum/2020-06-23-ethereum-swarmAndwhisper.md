---
title: "Ethereum Swarm, Whisper 간략 정리"
date: 2020-06-19
toc: true
toc_sticky: true
categories: Ethereum
---

# Swarm이란?
- 이더리움의 탈중앙화된 데이터 저장소 (분산 p2p), IPFS와 유사
- 해시를 사용해 임의의 스웜 파일에 접근 가능함 (max 4kb)
- ENS를 통해 사람이 읽을 수 있는 이름을 등록할 수 있다.
- geth에 포함되어있고, DAPP 전체를 패키징해서 올리는 것도 가능하다.
- web3.bzz 

## 3 crucial notions

### 1. Chunk
- 제한된 크기의 데이터 조각
- 스웜에서 검색, 저장되는 기본 단위
- __The Network layer only knows about chunks and has no notion of file or collection__

### 2. Reference
- 클라이언트가 컨텐츠에 대해 검색, 접근할 수 있게 해주는 파일의 unique id
- id는 64 bytes, 앞 32bytes 는 파일의 해싱값, 뒤 32bytes는 복호화 키 
- __collision free__ : two different blobs of data will never map the samp identifier
- __deterministic__ : 같은 컨텐츠는 같은 Id를 갖는다.
- __uniformly distributed__

### 3. Manifest
- manifest is a data structure describing file collections
- URL 기반 컨텐츠 검색을 가능하게 하는 패쓰, 컨텐츠 해시를 명시.


# Whisper
- 탈중앙화된 메시지 프로토콜
- geth에 포함
- Ethereum P2P protocol 상에서 유저 간 암호화 메시지 송수신 프로토콜
- Ethereum blockchain과 동일한 네트워크를 사용하며, whisper node간 모든 whisper 메시지를 공유한다.
- 공유되는 메시지는 암호키를 갖고있는 수신자만 확인 가능하다.
- 1:1, 1:N 메시지 전송 가능
- web3.shh




> 출처 : 마스터링 이더리움, [스웜 가이드](https://swarm-guide.readthedocs.io/en/latest/introduction.html)