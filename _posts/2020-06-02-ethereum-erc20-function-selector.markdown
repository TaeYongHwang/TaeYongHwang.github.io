---
title: "이더리움 ERC20 토큰과 Function Selector"
date: 2020-06-02
categories: Ethereum
---

# 코인과 토큰
- 코인이란, 자신만의 메인 블록체인이 존재해 독립적으로 돌아갈 수 있는 암호화폐를 지칭 (ex. Ether, BTC, ...)
- 토큰이란, 메인 블록체인 내의 기본적인 규약을 지키면서 종속되어 돌아가는 암호화폐를 지칭 (ex. Ethereum 안에서 돌아가는 USDT, ...)
- fungible token (대체 가능한 토큰) : unique하지 않고, 동일한 가치의 다른 코인으로 완벽히 대체할 수 있는 코인 (ex. ERC20)
- NFT (Non-Fungible Token - 대체 불가능 토큰) : 본질적으로 unique해 다른 토큰과 구분되는 토큰 (ex. ERC721)

# ERC20 토큰이란,
 이더리움 블록체인 상에서 돌아가는 Smart Contract의 일종으로, Ethereum Request For Comments - 20에서 정한 규격을 만족하는 토큰들을 모아서 지칭한다. 여기서 정한 규격이란 6개의
 함수와 2개의 이벤트를 말한다.
 ```solidity
    function totalSupply() public constant returns (uint);
    function balanceOf(address tokenOwner) public constant returns (uint balance); 
    function allowance(address tokenOwner, address spender) public constant returns (uint remaining);
    function transfer(address to, uint tokens) public returns (bool success);
    function approve(address spender, uint tokens) public returns (bool success);
    function transferFrom(address from, address to, uint tokens) public returns (bool success);

    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
```
위에 적어놓은 것들을 모두 지킨다면, ERC20 토큰이라고 한다.

# Solidity와 Function Selector
 솔리디티에서는 각 함수를 function selector를 이옹해 구분한다. 여기서 function selector란 함수의 keccak256 해시값의 앞 4byte만 사용한다. (빅 엔디안 방식일 때)   
 만약 function selector값이 충돌하는 경우에는 에러를 반환하고 어떠한 바이트코드도 생성하지 않는다. 즉, 컴파일러에서 function selector가 겹치지 않음을 보장해준다.
 ```solidity
transfer(address,uint256) : '0xa9059cbb'
```
 위의 모양을 보면 알겠지만, function selector는 함수의 이름, 각 인자의 타입을 통해서 해싱하게 된다.
 
# event 호출과 이더리움 로그 분석
 ```solidity
emit x(uint indexed age, uint indexed height);
```
1. 이벤트 선언 자체의 해시값 : topics[0]에 저장
2. 첫 번째 (age)에 들어온 값의 16진수 값 : topics[1]에 저장
3. 두 번째 (height)에 들어온 값의 16진수 값 : topics[2]에 저장

indexed가 붙지 않는 변수에 대해서는 data 영역에 저장된다.

이 사실을 알고 Transfer 이벤트를 살펴본다면
```solidity
event Transfer(address indexed from, address indexed to, uint tokens);
```
1. Transfer 이벤트 자체의 해시값이 topics[0]에 저장된다.
2. 특정 토큰에 대한 송신자 (from)가 topics[1]에 저장된다.
3. 특정 토큰에 대한 수신자 (to)가 topics[2]에 저장된다.
4. 토큰에 대한 양 (tokens)이 data 영역에 저장된다.

이 정보를 활용해 특정 트랜잭션에서 토큰에 대한 송수신 정보를 뽑아낼 수 있다. (보편적인 경우)   

하지만, ERC20 토큰으로 분류되어 있음에도 불구하고 해당 방식으로 파싱할 수 없는 경우들이 존재한다.
```solidity
event Transfer(address from, address to, uint tokens);
```
이런 경우처럼 indexed가 붙지 않아도 결국 솔디티디에서의 해싱값은 같기 때문에, 코드를 직접 뜯어보지 않는 경우에서는 ERC20과 완전히 동일하게 보인다.
하지만 이 경우에 대해서 Transfer를 이용해 토큰에 대한 전송 정보를 파싱한다면, topics[1], topics[2] 가 비어있게 될테고, 정상적인 방식으로는 파싱할 수 없게 된다.
또한 transfer 함수 등에서 Transfer 이벤트를 호출해야 된다는 규격은 없기 때문에, 모든 토큰 전송 부분에서 해당 이벤트를 emit하지 않는다면,
토큰 전송 정보에 대해 트랜잭션 로그를 분석해서 정확한 정보를 알아낼 수 있는 방법은 없을 것이다.
