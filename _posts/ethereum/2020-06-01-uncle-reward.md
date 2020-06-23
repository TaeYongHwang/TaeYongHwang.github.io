---
title: "Uncle Reward 종류 및 설명"
date: 2020-06-01
categories: Ethereum
---

# Uncle Block 이란?
 이더리움 메인 체인으로 선택받지 못한 블록으로, stale block 이라고도 한다.

#### 왜 Uncle Block에 대한 정보가 on chain 상에 기록되어 있는지?
 블록체인의 분산 컴퓨팅의 특성 상, 불가피하게 fork가 생긴다. 이 경우 선택받지 못한 블록들에 대해서 불필요하게 CPU 파워가 낭비되는데,
 이 때문에 Uncle Block을 그냥 버리게되면 블록체인에 3가지의 악영향을 끼치게 된다. 첫째, 평균 블록 생성 시간이 늦춰지게 되며, 둘째로 블락체인의 블록 생성 난이도가 낮아지게 된다.
 마지막으로, 네트워크 보안 수준이 저하되는 결과가 초래된다. 
 
#### GHOST (Greedy Heaviest Object SubTree) Protocol의 사용
 다중 체인 방지를 위한 기법으로, Uncle Block에 대한 Reward를 지급함으로써 Uncle Block을 메인 체인에 포함시켜 난이도를 하락시키지 않게끔 만든다.   
 Bitcoin의 경우 Longest Chain Selection 기법을 사용하며, Ethereum에서는 Heaviest Chain Selection 기법을 사용한다.   
 (여기서 Heaviest란 각 블락을 Root로 하는 Subtree의 크기를 말함)
 
 
#### 2가지의 Uncle Rewards
 
 1. Uncles Reward
 > (Uncle_Block_Height - Block_Height + 8) * Miner's Fixed Reward / 8    

- Uncle Block의 마이너에게 들어가는 reward. 위의 수식을 통해 리워드가 지급된다.   
- Uncle_Block_Height는 web3의 getUncle을 통해 알아낼 수 있다.
- Fixed Reward란 Gas, Uncle reward를 제외한 기본 Block Reward를 말함
 
 2. Uncle Inclusion Reward
 > Fixed Reward의 3.125% 
  
- 메인 체인의 Block Miner에게 들어가는 reward   
- 최대 2개의 Uncle Block까지 리워드를 받을 수 있다.   
 ex) 2개의 Uncle Block을 포함시 6.25%의 리워드를 추가로 받을 수 있다. 
  
  

