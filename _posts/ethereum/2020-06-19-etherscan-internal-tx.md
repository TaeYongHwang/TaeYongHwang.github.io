---
title: "Etherscan Internal Transaction 방식 해석"
date: 2020-06-19
toc: true
toc_sticky: true
categories: Ethereum
---

# 이더스캔에서의 Internal Transaction (메시지) 표현
 위의 사진을 보면 21개의 메시지가 있는 것을 볼 수 있다. 하지만 실제로 콜스택을 따라 메시지의 개수를 전부 세본다면 해당 블록에서
 184개의 메시지가 있는 것을 확인할 수 있다. (이에 관한 자세한 파싱 방법은 생략한다.)
 그렇다면 왜 21개의 메시지가 있다고 표현되는지 알아보자
 
## Simple 모드
 위의 사진을 보면, 우측 상단에 __Simple__ 을 볼 수 있을 것이다. 즉, 이더스캔에서는 모든 메시지 중에서 특정 알고리즘을 거쳐 쓸만하다고
 생각하는 것들을 보여주는 것이다.

## Advanced 모드
 위의 사진의 우측 상단을 보면 __Advanced__ 를 볼 수 있다. 여기서 보이는 6개의 정보 중, __Simple__ 모드에 보이는 것은 오직 하나 뿐이다.
 이 정보들을 자세히 보고 싶다면 __Parity Trace__ 를 클릭해 볼 수 있다.
 
## 이더스캔 알고리즘
 이 부분에서 하는 말과 관련된 정보들 링크이다.    
> [Internal Tranaction 정보](https://taeyonghwang.github.io/ethereum/ethereum-internal-tx/)

 내가 알아봤을 때, 총 3가지 경우에 대해 메시지를 파싱해서 적어준다.
 1. Contract Creation 
 2. Suicide (self-destruct)
 3. 이더 전송 중에서도 Normal call (delegate, static은 파싱하지 않는다.)   
 이 세 가지를 이더스캔에서는 가장 중요한 메시지라고 생각하는 듯 하다.