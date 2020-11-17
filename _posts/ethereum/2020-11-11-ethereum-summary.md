---
categories: Ethereum
title: "Ethereum 개요"
toc: true
toc_sticky: true
date: 2020-11-11
---

# Ethereum

## Stale  Block (Uncle Block)
- 거의 비슷한 시간에 두개의 블록이 채굴된 경우, 하나의 블록은 버려지게 되는데, 이 때 버려진 블록이 __Stale Block__

## Ghost Protocol (Greedy Heaviest Object Subtree)
- uncle block에 대한 보상을 지급함으로써 uncle block을 메인 체인에 포함하여 블록체인 전체 난이도 상승 유도
- fork 발생 시 __uncle block까지 포함한 자신의 서브트리의 크기가 가장 큰 child가__ 메인체인의 블록으로 선택된다. 
- 사용 이유 :
> uncle 블록 생성시 , 파워가 불필요하게 낭비된다.   
> 이로 인해 평균 블록 생성 시간이 늦춰지게 되며, 난이도가 낮아지게 된다.   
> 결국 블록체인 네트워크 보안 수준이 저하되는 결과를 초래한다.

- 부가 설명 : [Uncle Reward 종류 및 설명](https://taeyonghwang.github.io/ethereum/uncle-reward/)

## Fork
- regular fork, soft fork , hard fork 의 3 경우 존재


## 피어 검색
- 모든 노드와 연결될 필요는 없고, 몇 개의 노드에만 연결하면 된다.
- Kademlia 프로토콜에 기반한 자체 노드 검색 프로토콜 존재

### 부트스트랩 노드
- 일정 시간동안 연결되어 있는 노드의 목록을 유지하고, 해당 목록을 이더리움 네트워크에 연결하려는 클라이언트에 제공
- 블록체인 자체를 보유하고 있진 않음


# Ether 전송 시나리오

## 1. EOA -> EOA, CA -> EOA 
- 전송 가능

## 2. EOA -> CA
- fallback 함수 없는 경우 : reject (이더 EOA에 반환)
- fallback 함수 있는 경우 : ether를 reject 하지 않게 만들어둔 경우에 전송 가능

## 3. CA1 -> CA2
- CA2 폴백 함수 구현에 따라 다를듯 (2와 비슷)














> 출처
> 이더리움을 활용한 블록체인 프로젝트 구축











