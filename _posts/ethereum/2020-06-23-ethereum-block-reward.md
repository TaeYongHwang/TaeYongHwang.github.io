---
title: "Ethereum 블록 리워드 계산"
date: 2020-06-19
toc: true
toc_sticky: true
categories: Ethereum
---

# 블록 리워드를 계산하기 위해서 필요한 것들
- 해당 블록에서의 Fixed Reward 를 알아야 한다.
> 각 블록헤이트 마다의 Fixed Reward   
> [0, 4370000) : 5 Ether
> [4370000, 7280000) : 3 Ether   
> [7280000, ~) : 2 Ether

- 해당 블록에 담긴 모든 트랜잭션이 사용한 Transaction Fee 를 알아야 한다.
> web3.eth.getTransaction 시에 나오는 __gasPrice__ 와 web3.eth.getTransactionReceipt 시에 나오는 __gasUsed__ 값을 곱해주는 과정을 모든
> 트랜잭션에 대해서 수행한 후 더해주면 된다. 

- Uncle Reward를 알아와야 한다.
> 해당 블록 마이너에게는  Fixed Reward의 3.125% (최대 2개까지, uncle의 개수만큼 곱해줌)   
> uncle 에게는 (Uncle_Block_Height - Block_Height + 8) * Miner’s Fixed Reward / 8   
> [Uncle Reward 관련 정보](https://taeyonghwang.github.io/ethereum/uncle-reward/)

__위의 3가지 방법을 통해 나온 결과를 알맞은 주소해 더해줌으로써 해당 블록에서 reward 받은 주소들과 각각의 이더량을 알 수 있다.__



 
 
