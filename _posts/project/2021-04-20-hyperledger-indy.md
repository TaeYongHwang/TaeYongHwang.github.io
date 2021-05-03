---
title: "Hyperledger Indy"
date: 2021-04-02
toc: true
toc_sticky: true
categories: Project
---

__하이퍼레저 인디는 ZKP VC/VP에 특화된 SSI 플랫폼__   


# Hyperledger Indy
1. __indy-node__ 프로젝트
    - 신원 인증에 필요한 데이터를 저장할 블록체인 개발 프로젝트 (SSI 특화 블록체인)
    - indy-plenum 프로젝트 기반으로 개발 (indy-node를 indy-plenum에 플러그인하여 사용)

2. __indy-sdk__ 프로젝트
    - 블록체인과 클라이언트 간의 통신을 위한 API 개발 프로젝트

3. __Aries__ 프로젝트
    - DID, VC, VP 등 사용자가 직접 사용하는 상위 데이터 프로토콜을 개발하기 위한 프로젝트
    - aries-rfcs: DID, VC, VP 등 SSI 구성요소 관련 기술 사양 및 프로토콜 정의
    - aries-framework-go :ACCEPTED 상태가 된 aries-rfcs의 RFC 내용을 바탕으로 Go 언어로 구현된 SSI 프레임워크 개발

4. __indy-plenum__
    - indy-node의 합의 알고리즘, 원장 구조 등 개발


# indy-node 프로젝트
- 블록에 대한 합의를 수행하지 않고 __트랜잭션__ 에 대한 합의를 수행하기 때문에, 블록 단위가 아닌 트랜잭션 단위로 저장된다.

- DID document 구성 요소를 JSON 형태로 보관하지 않고 각각 따로 저장하며, DID resolver를 통해 데이터를 취합하여 DID document 형태로 반환한다.
  
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

## 블록체인 원장
- RocksDB 기반의 key-value 데이터베이스로 개발
- __순서대로 정렬된 트랜잭션(Ordered log of transactions)__ , __머클 패트리샤 트리(Merkle Patricia tree)__ 의 두 가지 데이터를 블록체인 원장을 통해 관리
- __type__ 항목을 통해 어떤 트랜잭션인지 알 수 있다. (ex. type에 1이 적혀있는 경우, NYM 트랜잭션 등)

- 네 가지 종류의 원장을 통해 데이터를 저장
    1. __Domain ledger__ 
    2. __Config ledger__
    3. __Pool ledger__
    4. __Audit ledger__ 

### Domain ledger
DID와 같이 사용자의 신원 인증과 관련된 데이터를 저장
> __Schema__ : 신원 증명 양식 중 사용자 속성에 대한 항목을 정의한 데이터   
> __Credential definition__ : issuer가 Schema가 포함된 Credential definition에 사용자 ID 속성값을 채운 뒤 추가적인 필드와 함께 사용자에게 신원증명을 발행한다.


- NYM 트랜잭션
    - 사용자의 DID document 관련 정보와 사용자를 어떤 권한을 가진 그룹으로 분류하는지에 대한 정보가 담겨 있다.
    - dest 항목의 DID가 이미 저장되어 있는 경우는 업데이트, 저장되어 있지 않은 경우는 신규 사용자 등록으로 간주
    
- SCHEMA 트랜잭션
    - 사용자 속성에 대한 항목이 정의 
    - VC를 발급하고자 할 경우, SCHEMA를 가져와서 양식에 맞게 채운 후 발급해줄 수 있다.

- 이 외에도 __ATTRIBN, CLAIM_DEF, REVOC_REG_DEF__ 등의 트랜잭션 존재  

### Config ledger
- 사용자 권한 정보 저장 (Public-Permissioned 구조이므로)
- 블록체인 네트워크에 대한 다양한 설정 정보 저장

### Pool ledger
- 블록체인 노드의 IP/port 등의 정보 저장
- 최초 블록체인 구동 시 사용하는 __Genesis Transaction__에 최초의 블록체인 노드 정보가 포함되어 있다.

 - NODE 트랜잭션
    - 해당 트랜잭션을 전송하여 노드 CRUD 작업 수행이 가능
    - dest 항목의 DID가 존재하는 경우 기존 노드 정보를 수정하며, 저장되어 있지 않은 경우 새로운 노드를 추가
 
### Audit ledger
- 앞의 3가지 원장에서 발생하는 트랜잭션을 모두 순서대로 취합하여 저장 (각 트랜잭션은 서로에게 의존성이 있기 때문)
- 서로 다른 원장 간의 동기화와 추가되거나 장애로부터 복구된 노드 업데이트를 위해 사용



## [사용자 그룹 별 권한](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/auth_rules.html#)
- 사용자를 5가지 그룹으로 분류해 관리
    - 권한은 Trustee > Steward > Endorser > Network_Monitor > User 순
    
    - None: common User
        - 블록체인에 직접적으로 참여하지 않는 일반 사용자로서 블록체인 데이터를 읽어 오는 권한만 갖고 있는 그룹
    
    - 0: Trustee
        - indy-node 블록체인을 구동한 최초의 구성원들로 구성되며, 블록체인 운영에 관한 모든 권한을 갖는다.
    
    - 2: Steward
        - Trustee 그룹의 허가하에 블록체인 노드 운영 권한을 갖게된 그룹
        
    - 101: Endorser
        - 블록체인에 트랜잭션을 쓸 수 있는 권한을 갖는 그룹
        
    - 201: Network Monitor
        - 블록체인 노드의 장애 여부나 성능을 모니터링하는 그룹         

## indy-sdk 프로젝트



## 생각 정리 (불확실)
- 지갑은 그저 DID, key 정보를 보관하는 역할 (실제 노드에 반영된지 여부는 상관이 없다)   
- 노드에 반영하기 위해선 트랜잭션을 작성해야됨 (NYM 등)   
- 노드에 반영된 정보는 getNYumRequest 등으로 받아올 수 있지만, 노드에 등록 되지 않은 did를 이용해서도 값을 받아볼 수 있다.
 하지만 실제 트랜잭션을 작성하기 위해선 Endorser 이상만 가능하기 때문에 노드에 등록된 did를 이용해야 가능하다. (public - permissioned 라고 불리는 이유인듯)

  
- Schema sample
```json
{
  "id": "1", 
  "name": "gvt",
  "version": "1.0",
  "ver": "1.0",
  "attrNames": ["age", "sex", "height", "name"]
}
```





> Reference.   
> 윤대근, 자기주권 신원증명 구조 분석서, 제이펍, 2020-07-03   