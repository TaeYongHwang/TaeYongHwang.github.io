---
title: "Ethereum Gas"
date: 2020-06-05
categories: Ethereum
---

# Gas 란?
 트랜잭션을 처리하는데 드는 비용, 즉 Transaction Fee 라고 생각하면 되며 Max_Tx_fee = Gas_Price * Gas_Limit 로 표현한다.   
 무한루프 코드가 컨트랙트에 존재하는 경우 다른 적법한 코드들이 수행될 수 없기 때문에, 즉 의도적인 공격에서 네트워크를 보호하는 측면도 존재
 
# Gas 종류
1. Gas Limit : 트랜잭션을 보내는 사용자가 설정하는 것으로, 해당 트랜잭션을 보낼 때 사용할 수 있는 최대 가스량이다.
2. Gas Price : 동일하게 사용자가 설정하는 것으로, 1 Gas 당 가격이라고 생각하면 된다.
3. Block Gas Limit : 이더리움 마이너들이 설정하는 값으로, 한 블록에 들어갈 수 있는 최대 가스량이다. (개별 Gas Limit의 총합은 Block Gas Limit를 넘을 수 없다.)

# Gas 에 대하여
 - 트랜잭션이 종료된 후 남은 가스는 원래 소유자에게 다시 돌려주며, 가스라 모자라서 트랜잭션의 오퍼레이션을 전부 실행할 수 없는 경우에는 트랜잭션이 revert 되며,
 사용한 가스 역시 돌려주지 않는다. (사용한 가스는 마이너의 주소로 보내짐) 
 - Gas Limit를 낮게 설정하는 경우 실제 소모가스가 Gas Limit를 넘길 수 있는 문제 (Out Of Gas)가 존재하며, 너무 높게 설정한다면 Block Gas Limit로 인해
 블록에 담길 수 없는 문제가 존재한다.   
 - 이더리움 송금에 대해서는 21000 Gas를 default로 사용하기 때문에 Gas Limit를 설정해주기 편하지만, Contract Call에 대해서는 가스 사용량이 제각각이기 때문에
 정확한 가스 리미트를 맞춰서 전달하기가 어렵다 (각 오퍼레이션 별로 추정 가스치를 알려주는 사이트도 존재)
 - 실제로 트랜잭션이 블록에 쌓일 때 사용된 정확한 가스량을 알고 싶다면, web3.eth.getTransaction 함수를 호출해 나오는 gasPrice 필드와 web3.eth.getTransactionReceipt에
 존재하는 gasUsed를 곱해 알아낼 수 있다.

  
