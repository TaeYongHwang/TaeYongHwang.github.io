---
title: "Hyperledger Indy"
date: 2021-04-02
toc: true
toc_sticky: true
categories: Project
---

__하이퍼레저 인디는 ZKP VC/VP에 특화된 SSI 플랫폼__   


# Hyperledger Indy
1. indy-node 프로젝트
    - 신원 인증에 필요한 데이터를 저장할 블록체인 개발 프로젝트 (SSI 특화 블록체인)
    - indy-plenum 프로젝트 기반으로 개발 (indy-node를 indy-plenum에 플러그인하여 사용)

2. indy-sdk 프로젝트
    - 블록체인과 클라이언트 간의 통신을 위한 API 개발 프로젝트

3. Aries 프로젝트
    - DID, VC, VP 등 사용자가 직접 사용하는 상위 데이터 프로토콜을 개발하기 위한 프로젝트
    - aries-rfcs: DID, VC, VP 등 SSI 구성요소 관련 기술 사양 및 프로토콜 정의
    - aries-framework-go :ACCEPTED 상태가 된 aries-rfcs의 RFC 내용을 바탕으로 Go 언어로 구현된 SSI 프레임워크 개발

4. indy-plenum
    - indy-node의 합의 알고리즘, 원장 구조 등 개발


## indy-node 프로젝트
- 블록에 대한 합의를 수행하지 않고 트랜잭션에 대한 합의를 수행하기 때문에, 블록 단위가 아닌 트랜잭션 단위로 저장된다.

- DID document 구성 요소를 JSON 형태로 보관하지 않고 각각 따로 저장하며, DID resolver를 통해 데이터를 취합하여 DID document 형태로 반환한다.

- 네 가지 종류의 원장을 통해 데이터를 저장
    1. __Domain ledger__ : DID와 같이 사용자의 신원 인증과 관련된 데이터를 저장
    2. __Config ledger__ : 사용자 권한 정보 저장 (Public-Permissioned 구조이므로)
    3. __Pool ledger__ : 블록체인 노드의 IP/port 등의 정보 저장
    4. __Audit ledger__ : 앞의 3가지 원장의 트랜잭션을 순서대로 저장하고 있다가 동기화나 복구 등에 사용
  
- 두 가지 종류의 노드로 운영 
    - 장애 처리에 이점을 가지며, request를 분산하여 처리함으로써 노드의 부하를 줄일 수 있다.
    1. __Validator Node__ : Write request를 처리하며, RBFT 합의 알고리즘을 구동하는 노드
    2. __Observer Node__ : 사용자의 Raed request를 처리하며, Validator에 장애 발생 시 언제든 Validator가 될 수 있도록
                           대기 상태 유지
       
## 합의 알고리즘
__indy-node__ 에서는 PBFT 계열의 합의 알고리즘인 __RBFT(Redundant Byzantine Fault Tolerance)를 사용__

-  __완벽한__ 합의 알고리즘은 세 가지 특성을 모두 만족해야 한다.
    1. Termination(Liveness) : 합의에 참여하는 모든 노드는 언젠가는 값을 결정해야 한다. 
    2. Agreement(Safety) : 모든 노드는 동일한 값을 결정해야 한다.
    3. Validity(Safety) : 결정된 값은 어떠한 과정을 통해 결정되어야만 한다.
    - __FLP Impossibility__ : 비동기 네트워크에서는 위의 3가지 특성 중 최대 2가지까지만 만족할 수 있다. 

- 블록체인은 __Liveness(실시간성)__ 과 __Safety(안정성)__ 의 두 가지 특성으로 다시 분류될 수 있다.
    - __Liveness__ : 비트코인, 이더리움 등
    - __Safety__ : indy-node 


### PBFT
__Safety 특정이 강조된 알고리즘__ 으로, 장애 혹은 악의적인 노드 수가 f일 때, 최소한 __3f + 1__ 대의 노드가 존재하면 정상적인 합의가 가능하다.

- 왜 __3f + 1__ 이 최소한일까?
  1. 장애 노드 f 개가 존재할 시, __N - f__ 대의 올바른 노드 간 합의 알고리즘을 수행해야 함
  2. 악의적인 노드 f 개가 존재할 시, __(N - f) - f__ 대의 올바른 노드 간 합의 과정을 수행해야 함
  3. 합의를 수행하는 노드가 악의적인 노드 수보다 많아야 하므로, __N - 2f > f__를 만족하는 최소 값 __N = N - 3f + 1__ 이 된다.
  
- 전제 조건
    1. 메시지 전달이 지연, 실패, 혹은 무작위 순서인 __비동기 네트워크 환경__ 을 허용
    2. 악의적인 노드가 있다고 가정 (__Byzantine Node__ 라고 명명)
    3. 메시지는 언젠가는 전달
    4. 메시지에는 비대칭키를 이용한 암호화 및 서명, 해시 등이 사용되며 암호 및 해시 함수는 안전하다고 가정
 
- 동작 과정
    1. client는 primary 노드에게 __request__ 메시지를 전송
    2. primary 노드는 수신한 request 메시지를 모든 backup 노드에 전송
    3. backup 노드는 __pre-prepare, prepare, commit__ 이라 불리는 3-phase 과정을 수행 후, 각자 도출한 결괏값을 client에 보냄
    4. client 노드는 f + 1 개의 동일한 응답을 받으면 정상적인 결괏값이 수신됐다고 판단 (악의 노드가 f개라 가정하기 때문)
  

### RBFT
기존 PBFT 계열의 합의 알고리즘에서 발생하는 view-change 실행 시 성능 저하 문제를 해결하기 위해 개발된 합의 알고리즘








> Reference.   
> 윤대근, 자기주권 신원증명 구조 분석서, 제이펍, 2020-07-03   