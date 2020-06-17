---
title: "Ethereum Transaction에 대하여"
date: 2020-06-11
toc: true
toc_sticky: true
# toc_label: TOC 제목 설정
categories: Ethereum
---

# Web3 getTransaction, getTransactionReceipt 구조

## getTransaction
```
{
  blockHash: '0x43f34ae1b9cda49947a4b1001081356f80144d6ce2299ae7f959befe2125f55c',
  blockNumber: 10240861,
  chainId: '0x1',
  condition: null,
  creates: null,
  from: '0xcDd6a2b9DD3e386C8cd4a7ada5cab2F1c561182D',
  gas: 21000,
  gasPrice: '60000000000',
  hash: '0x663ae686dc57a55262704c8f1a8b133e1548fd0e32ae72d399630ad842d68be7',
  input: '0x',
  nonce: 858,
  publicKey: '0xa760665d6f01d8dad54ecdec06834e84af673405e2c6674d3ccdcfacba8a42e4e21fe623cf612617bcb7197e9b928db97aecf46a7f8d20b89e221040bc2605e2',
  r: '0x6ebe87e156cbc5d540650793832865c0cd234df748b3aaf88680ca87f2f76756',
  raw: '0xf86e82035a850df847580082520894142e6c18e62d1bdca2ce40c05c6ecb3e1af1848c888e12502890835a008026a06ebe87e156cbc5d540650793832865c0cd234df748b3aaf88680ca87f2f76756a01f0e5583f4b8504240d61847bebc5d09989559cfe45a83dc572556c83a646946',
  s: '0x1f0e5583f4b8504240d61847bebc5d09989559cfe45a83dc572556c83a646946',
  standardV: '0x1',
  to: '0x142e6C18e62d1bDCA2cE40c05c6eCB3e1AF1848C',
  transactionIndex: 14,
  v: '0x26',
  value: '10237333038120000000'
}

```
- 위의 트랜잭션 정보는 일반적인 이더 전송 트랜잭션이다. from -> to로 value 만큼의 이더를 전송한다는 의미이며, input 필드 값이 '0x'로 비어있는 것을 확인할 수 있다.
- 만약 토큰 생성 트랜잭션이라면, to 필드가 비어있고 input 필드에 해당 컨트랙트 정보가 적힐 것이다.
- 토큰 전송 트랜잭션인 경우에는 to 필드에 해당 컨트랙트 주소가, input 필드에 호출된 컨트랙트 함수 정보가 저장될 것이다 (function selector, parameters, ...)

## getTransactionReceipt
```
{
  blockHash: '0x43f34ae1b9cda49947a4b1001081356f80144d6ce2299ae7f959befe2125f55c',
  blockNumber: 10240861,
  contractAddress: null,
  cumulativeGasUsed: 1196178,
  from: '0xcdd6a2b9dd3e386c8cd4a7ada5cab2f1c561182d',
  gasUsed: 21000,
  logs: [],
  logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
  status: true,
  to: '0x142e6c18e62d1bdca2ce40c05c6ecb3e1af1848c',
  transactionHash: '0x663ae686dc57a55262704c8f1a8b133e1548fd0e32ae72d399630ad842d68be7',
  transactionIndex: 14
}
```
- receipt 정보는 트랜잭션이 블록체인 상에 deploy된 후에 생긴다.
- 위의 정보는 일반적인 이더 전송 receipt이며, contractAddress가 null임을 확인할 수 있다.
- 컨트랙트 생성 트랜잭션인 경우, contractAddress에 해당 주소가, to 필드는 비어있음을 확인할 수 있다.
- 토큰 전송 트랜잭션인 경우, contractAddress에는 보낼 토큰 주소 정보가, to 필드에는 받는 주소 정보가 들어있다.


# 트랜잭션 처리 순서 ( 코어 이더리움 프로그래밍 책 참조 )
```
특정 주소 A -> B 로 트랜잭션을 보낸다고 할 때.
```
1. 해당 트랜잭션에 대해 사용자 A가 자신의 개인키로 ECDSA 전자 서명 암호화시킨다.
2. 이더리움 클라이언트는 해당 트랜잭션을 네트워크에 연결되어 있는 모든 노드에 브로드캐스팅 한다.
3. 마이너에서 휴효성을 검증한다.   
3.1 트랜잭션이 문법에 맞는지
3.2 A의 공개키를 이용해 해당 전자 서명이 유효한지 (신원 검증)
3.3 A의 Account에 있는 Nonce와 일치하는지 (이중 지불 방지)
3.4 가스 비용을 계산하고, B의 어카운트 주소를 확인한다.
4. 모든 검증 작업이 완료되면 최종 실행을 위해 해당 트랜잭션을 트랜잭션 풀에 등록한다.
5. 마이너가 트랜잭션 풀에서 트랜잭션을 처리 시, 가스 실행 비용이 높은 순으로 트랜잭션을 선택한다.
5.1 이더 전송 : B에 송금
5.2 B가 contract : contract code 실행

# 마이너의 트랜잭션 처리 과정
1. 블록에 담을 트랜잭션들을 Transaction Pool에서 가져온다.
2. 트랜잭션들에 대해 Nonce, Gas Price로 순서를 정한다.
3. 마이너가 트랜잭션에 설정된 값 (Gas_Limit * Gas_Price) 만큼을 발신 주소로부터 tx fee로 가져온다.
4. 트랜잭션을 실행하며, 명령어에 따라 가스를 소모한다.   
4.1 남는 가스는 해당 트랜잭션 발신 주소로 다시 돌려준다.   
4.2 가스가 모자라는 경우 트랜잭션이 Revert되며, 가스는 다시 돌려주지 않는다. (Out Of Gas Exception)
5. 실행된 트랜잭션들을 블록에 담는다. (개별 Gas Limit의 총합은 Block Gas Limit를 넘길 수 없다.)


# Nonce
- 특정 주소에서  __보내는__ 트랜잭션에 할당된 번호로, 거래 전송 시마다 nonce 값이 1씩 증가하며, 한 주소에 동일한 nonce를 갖는 트랜잭션은 존재하지 않는다.
- nonce는 순차적으로 증가되고 처리된다. (역행 불가)
- 아직 트랜잭션이 transaction pool에 남아있는 경우에 한해서, nonce를 이용해 트랜잭션을 재전송하여 취소된 효과를 볼 수 있다.
- 이중 지불 문제를 방지시킨다. (같은 nonce에 여러 트랜잭션이 전송된 경우, 제일 높은 가스비를 지불한 트랜잭션을 처리시킨다.)

# Transaction vs Internal Transaction
- Transaction : EOA 주소에서 발생하는 트랜잭션으로, on-chain 상에 기록된다. 
- Internal Transaction : CA에서 발생하는 트랜잭션으로, Transaction의 결과로 실행되는 트랜잭션이며, on-chain 상에 기록되지 않는다.
















