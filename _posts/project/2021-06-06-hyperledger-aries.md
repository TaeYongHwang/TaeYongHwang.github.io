---
title: "Hyperledger Aries"
date: 2021-06-06
toc: true
toc_sticky: true
categories: Project
---

__Hyperledger Aries is a toolkit for building solutions focused on creating, transmitting, storing and using verifiable credentials__

# Hyperledger Aries

## 개념
- __public key__ : verkey
- __private key__ : signing key
- __wallet__ : DIDs, Credentials, metadata 등 SSI 위한 데이터 저장
- __agent__ : 다른 entity와 상호작용하는 소프트웨어를 의미 
    - 대부분의 agent는 DID, key, VC를 안전하게 관리하기 위한 secure storage 존재. 
    - Each interacts with other agents using secure, peer-to-peer messaging protocols.
  
## Aries Agent Architecture
__framework__ , __controller__ 로 구성

- framework
> The framework provides all of the core Aries functionality such as interacting with other agents and the ledger, managing secure storage, sending event notifications to, and receiving instructions from the controller.

- controller
> The controller executes the business logic that defines how that particular agent instance behaves—how it responds to the events it receives, and when to initiate events. The controller might be a web or native user interface for a person or it might be coded business rules driven by an enterprise system.


### Aries Agent 연결
A, B라는 __running agent__ 가 존재할때,
1. A에서 B를 반견한 후 __invitation__을 보낸다
    - __invitation__ : QR코드 등으로 보내지며, B -> A로 메시지를 안전하게 보낼 수 있는 정보들이 포함되어 있다.
  
2. B에서 동의한다면, 관계에 대한 __private DID__ 를 만든 후 B -> A 연결 요청 메시지에 함께 보낸다
    - __invitation__ 에 있는 정보를 활용해 안전하게 메시지를 보낼 수 있다.
  
3. A는 B가 보낸 메시지를  invitation과 연관짓는다.

4. A에서 동의한다면, B와의 연결 정보를 저장하고 __private DID__ 를 만든 후 B에 응답 메시지를 전송한다.

5. A, B 는 서로 메시지를 송수신할 수 있게 된다.
    - 새로 생긴 채널을 통해 메시지가 송수신되며, 비공개이며 안전하다.






> Reference.   
> [Becoming an Aries Developer](https://www.edx.org/course/becoming-a-hyperledger-aries-developer)