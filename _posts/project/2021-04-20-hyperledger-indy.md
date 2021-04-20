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
- 네 가지 종류의 원장을 통해 데이터를 저장
    1. __Domain ledger__ : DID와 같이 사용자의 신원 인증과 관련된 데이터를 저장
    2. __Config ledger__ : 사용자 권한 정보 저장 (Public-Permissioned 구조이므로)
    3. __Pool ledger__ : 블록체인 노드의 IP/port 등의 정보 저장
    4. __Audit ledger__ : 앞의 3가지 원장의 트랜잭션을 순서대로 저장하고 있다가 동기화나 복구 등에 사용
  
- 두 가지 종류의 노드로 운영
    1. __Validator Node__ : Write request를 처리하며, RBFT 합의 알고리즘을 구동하는 노드
    2. __Observer Node__ : 사용자의 Raed request를 처리하며, Validator에 장애 발생 시 언제든 Validator가 될 수 있도록
                           대기 상태 유지















> Reference.   
> 윤대근, 자기주권 신원증명 구조 분석서, 제이펍, 2020-07-03   