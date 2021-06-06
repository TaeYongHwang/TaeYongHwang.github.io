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







> Reference.   
> [Becoming an Aries Developer](https://www.edx.org/course/becoming-a-hyperledger-aries-developer)