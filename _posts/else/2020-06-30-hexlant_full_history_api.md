---
title: "헥슬란트 full history API 사용 후기 (Octet API)"
date: 2020-06-30
toc: true
toc_sticky: true
categories: Else
---


# FUll History API
 이더리움 등의 블록체인에 대한 정보를 얻기 위해서는 해당 블록체인에 대한 노드가 필요한데, 이를 위해서는
 자신의 컴퓨터에 많은 리소스를 노드에 할애해야 한다. (현재 이더리움만 4테라 정도)   
 이 정도의 용량을 따로 관리하기에는 무리가 있을 수 있고, 회사 차원에서는 자신들의 제품에 할애할 리소스를 낭비하는 꼴이 된다.
 이를 위해 제공하는 서비스가 __full history api__ 이다. 이런 대용량 노드를 헥슬란트에서 관리해 제공하면
 사용자들은 따로 리소스를 낭비하지 않고 호출해서 사용하면 되는 것이다.   
 해당 API를 헥슬란트에서는 __Octet API__ 로 이름지었는데, 이 __Octet API__ 를 4달 동안 사용해보며 느낀 장단점을 적으려고 한다.


## 장점
 __Octet__ 의 가장 큰 장점은 기존 web3에서 제공하는 API 뿐만 아니라, 헥슬란트 자체에서 만든 유용한 API를 제공한다는
 것이다. 이 중에서도 가장 유용했다고 생각한 2개에 대해 말해보려고 한다.
### 1. 특정 지갑이 갖는 특정 토큰에 대한 토큰량을 알 수 있다. (ERC20 규격 토큰)
 web3를 이용해 이더량을 구해오기는 쉽지만, 정확한 토큰량을 구해오기 위해서는 특정 CA의 ABI와 Address를 이용해
Contract 인스턴스를 만들고 호출하는 귀찮은 과정들이 많이 포함되어있다. 하지만 octet에서는 이런 API를 따로 
제공해준다.

### 2. 특정 지갑에 대한 트랜잭션 정보를 알아낼 수 있다.
 web3에서는 __특정 트랜잭션에 접근__ 하는 방법은 있어도 __특정 지갑이 갖는 트랜잭션 정보__ 를 따로 받아올 수 있는 방법은 없다.
모든 블록 정보를 조회하면서 찾아낸다면 가능하겠지만, 이게 리소스 낭비와 다른 점이 있을까?   
octet에서는 이러한 기능을 추가 제공함으로써 full history에 대한 강점을 더욱 더 살렸다. 
(내가 사용할 때에는 이 기능이 가장 유용했고, 불편함을 많이 덜어주었다.)


## 단점
### web3 연동 방법 부재
 API Docs를 보면, __axios__ 를 이용해 접근하는 방법은 있지만, web3를 직접 이용하는 법은 나와 있지 않았다.
 > [Octet API web3 연동](https://medium.com/hexlant/%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EC%84%9C%EB%B9%84%EC%8A%A4%EB%A5%BC-%EC%89%BD%EA%B2%8C-%EA%B0%9C%EB%B0%9C%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-409e9a8b56e2)   

 헥슬란트 블로그에는 나와 있는데 왜 Docs 에는 적어두지 않은 지는 모르겠다.
```
const options = {
  keepAlive: true,
  withCredentials: false,
  timeout: 30000,
  headers: [
    {
      name: 'Authorization',
      value: '[accessToken]'
    },
  ],
};
const httpProvider = new Web3HttpProvider(`${httpEndpoint}/rpc`, options);
const web3 = new Web3(httpProvider);
web3.eth.getBalance('0xa90742CDD09750829ee41E484C4dD89bEe425479').then(console.log);
```
 발급받은 Access Token을 이용해 이런 식으로 접근하면 __Infura__ 와 동일하게 web3를 이용할 수 있다.
 

## 결론
 Octet의 자체 제공 API가 생각보다 아주 편했기 때문에 이러한 추가적인 기능에 관심이 있다면 사용해 보는 게 좋을 것 같다.
 (필요하다고 생각하는 기능을 요청하면 만들어 주실지도...?)
 
 


