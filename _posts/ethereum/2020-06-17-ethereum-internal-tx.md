---
title: "Ethereum Internal Transaction에 대하여"
date: 2020-06-17
toc: true
toc_sticky: true
categories: Ethereum
---

# Internal 트랜잭션이란?
 일반적인 EOA -> CA 트랜잭션을 A라고 할 때, A가 실행되면서 CA의 내부에서 A에 대한 결과로 서브 트랜잭션들이 수행되는데 이것을
Internal Transaction이라고 한다. On-chain 상에 기록되는 정보는 오직 A 까지이며 서브 트랜잭션들은 On-chain 상에 기록되지 않는다.
그렇다면, Internal 트랜잭션 정보들은 어떻게 얻을 수 있을까?

## Internal 트랜잭션 특징
- Normal Tx가 성공했다고 해서 모든 internal tx가 성공하는 것은 아니다. (특정 internal tx 실패 시, 해당 서브트리의 internal tx 모두 실패)
- Internal Tx의 gas는 tx에 종속된다. 
- on-chain 상에 기록되지 않기 때문에, geth, parity 등의 이더리움 클라이언트 API를 사용해야 한다.
- Internal Tx를 통해 이더 전송, function call, contract creation 등이 가능하다. (주체가 EOA일 뿐이다)
- Normal Tx에 종속돼서 일어난다. (parent hash가 존재)

## Internal 트랜잭션 정보 얻는 법
 web3 같은 라이브러리를 이용해서는 해당 정보들을 얻어올 수 없고 geth, parity 같은 이더리움 클라이언트 노드를 이용해야 한다. (여기서는 parity를 사용한다.)
 
 > <https://openethereum.github.io/JSONRPC-trace-module#trace_block>

 위의 링크를 들어가보면, trace module이라고 적혀있는 것을 확인할 수 있다. 이 말 처럼, Internal 트랜잭션을 얻어오기 위해선 특정 블록, 특정 트랜잭션 단위로   
 해당 정보를 replay 시켜야 한다. 이더스캔을 써보면 __Internal Txns__ 필드를 볼 수 있는데, 이 칸 역시 이때까지의 모든 블록을 replay해서 얻은 정보를 가공해
 제공하는 것이라고 생각한다. 
```javascript
  const res = await axios.post('parity rpc 호출 가능한 uri', {
    method: 'trace_block',
    params: ['0x...'],
    id: 1,
    jsonrpc: '2.0',
  });
```
 node.js에서는 위와 같은 방식으로 호출이 가능하다.

## Internal 트랜잭션 정보
- 5개 (내가 확인한 개수)의 주요 인터널 트랜잭션이 존재한다.
- type이 'call'인 것과 'create', 'suicide'이 존재하며, 두 타입에 대해서 action, result 필드가 미묘하게 달라지는 것을 확인할 수 있다.

### 1. delegate call
- A 컨트랙트에서 B 컨트랙트 호출 시, B의 스토리지를 변경시키지 않고, B의 코드를 A에서 실행한다. (caller의 상태만 바꿀 수 있고, callee의 상태
를 바꿀 수 없다.)
- msg.sender, msg.value가 컨트랙트 A 호출 시와 같고, 변동되지 않는다. 


```json
  action: {
    callType: 'delegatecall',
    from: '0xae76c84c9262cdb9abc0c2c8888e62db8e22a0bf',
    gas: '0xdcb4',
    input: '0x6b1d4db700000000000000000000000000000000000000000000002025873626bea40000000000000000000000000000000000000000000000000000000000005eb0177b',
    to: '0x2157a7894439191e520825fe9399ab8655e0f708',
    value: '0x827329bfb5e68c9'
  },
  blockHash: '0xaa20f7bde5be60603f11a45fc4923aab7552be775403fc00c2e6b805e6297dbe',
  blockNumber: 10000000,
  result: {
    gasUsed: '0x7a47',
    output: '0x0000000000000000000000000000000000000000000000000827329bfb5e68c9'
  },
  subtraces: 2,
  traceAddress: [ 0 ],
  transactionHash: '0x3829fcf58a8357fd765b7f2916083405dddcde69dac837a5346e666fb6cd0add',
  transactionPosition: 18,
  type: 'call'
}
```


### 2. static call
- 상태를 변경하거나 읽는 경우를 구분하기 위해 사용하며, 다른 컨트랙트로 redirect되어도 상태 변경이 일어나지 않음을 보장한다.
- msg.sender는 컨트랙트 A가 된다.

```json
{
  action: {
    callType: 'staticcall',
    from: '0xae76c84c9262cdb9abc0c2c8888e62db8e22a0bf',
    gas: '0xc805',
    input: '0x70a08231000000000000000000000000ae76c84c9262cdb9abc0c2c8888e62db8e22a0bf',
    to: '0xe41d2489571d322189246dafa5ebde1f4699f498',
    value: '0x0'
  },
  blockHash: '0xaa20f7bde5be60603f11a45fc4923aab7552be775403fc00c2e6b805e6297dbe',
  blockNumber: 10000000,
  result: {
    gasUsed: '0x4d5',
    output: '0x0000000000000000000000000000000000000000000037b98d72379c5068c813'
  },
  subtraces: 0,
  traceAddress: [ 0, 0 ],
  transactionHash: '0x3829fcf58a8357fd765b7f2916083405dddcde69dac837a5346e666fb6cd0add',
  transactionPosition: 18,
  type: 'call'
}
```

### 3. call
- A 컨트랙트에서 B 컨트랙트 호출 시, B의 스토리지를 변경시키고, msg.sender는 컨트랙트 A가 된다.
- 실제 이더 전송, 토큰 전송 등의 주체가 되는 콜타입

```json
{
  action: {
    callType: 'call',
    from: '0xae76c84c9262cdb9abc0c2c8888e62db8e22a0bf',
    gas: '0xa979',
    input: '0xa9059cbb0000000000000000000000008455a0a413b5304a6780f4a249315de75640094100000000000000000000000000000000000000000000002025873626bea40000',
    to: '0xe41d2489571d322189246dafa5ebde1f4699f498',
    value: '0x0'
  },
  blockHash: '0xaa20f7bde5be60603f11a45fc4923aab7552be775403fc00c2e6b805e6297dbe',
  blockNumber: 10000000,
  result: {
    gasUsed: '0x41d2',
    output: '0x0000000000000000000000000000000000000000000000000000000000000001'
  },
  subtraces: 0,
  traceAddress: [ 0, 1 ],
  transactionHash: '0x3829fcf58a8357fd765b7f2916083405dddcde69dac837a5346e666fb6cd0add',
  transactionPosition: 18,
  type: 'call'
}
```

### 4. create
- 새로운 컨트랙트를 생성한다.

```json
{
  action: {
    creationMethod: 'create',
    from: '0x8a91c9a16cd62693649d80afa85a09dbbdcb8508',
    gas: '0x16e230',
    init: '0x608060405234801561001057600080fd5b506040516020806101f6833981018060405261002f9190810190610109565b60008054600160a060020a031916600160a060020a0383811691909117918290556040517f095ea7b300000000000000000000000000000000000000000000000000000000815291169063095ea7b39061009190339060001990600401610165565b602060405180830381600087803b1580156100ab57600080fd5b505af11580156100bf573d6000803e3d6000fd5b505050506040513d601f19601f820116820180604052506100e3919081019061012f565b50506101a5565b60006100f68251610180565b9392505050565b60006100f6825161018f565b60006020828403121561011b57600080fd5b600061012784846100ea565b949350505050565b60006020828403121561014157600080fd5b600061012784846100fd565b61015681610180565b82525050565b61015681610194565b60408101610173828561014d565b6100f6602083018461015c565b600160a060020a031690565b90565b151590565b600061019f8261018c565b92915050565b6043806101b36000396000f3006080604052600080fd00a265627a7a723058204db7b4c23d84c45bc6bfca313e9bf687bc7d21aece12952499166b478d5fb0496c6578706572696d656e74616cf500370000000000000000000000008e870d67f660d95d5be530380d0ec0bd388289e1',
    value: '0x0'
  },
  blockHash: '0xaa20f7bde5be60603f11a45fc4923aab7552be775403fc00c2e6b805e6297dbe',
  blockNumber: 10000000,
  result: {
    address: '0xbce5fe052b25e422550f6012fdd1941f9353f001',
    code: '0x6080604052600080fd00a265627a7a723058204db7b4c23d84c45bc6bfca313e9bf687bc7d21aece12952499166b478d5fb0496c6578706572696d656e74616cf50037',
    gasUsed: '0xfd0b'
  },
  subtraces: 1,
  traceAddress: [ 0 ],
  transactionHash: '0xa9b04ff42f93868886c1258ba59967ceb6aafa0118655032210a867bdad7cff1',
  transactionPosition: 24,
  type: 'create'
}
```

### 5. suicide (self-destruct)
- 배포된 컨트랙트에 존재하는 이더를 특정 주소로 보내고, 더 이상 사용하지 않는 컨트랙트라는 걸 명시

> [self-destruct 관련 정보](https://ethereum.stackexchange.com/questions/315/why-are-selfdestructs-used-in-contract-programming)

```json
    {
        "action": {
            "address": "0x6f268ae132e4a05197cdd0ca4cb0f29e92edfe2f",
            "balance": "0x0",
            "refundAddress": "0x0000000000b3f879cb30fe243b4dfee438691c04"
        },
        "blockHash": "0xb73b9ffc32d91b4b712aaa281993668a2345fafcb150c3fd3c0586efeb87070a",
        "blockNumber": 10000004,
        "result": null,
        "subtraces": 0,
        "traceAddress": [
            5,
            0,
            0,
            0
        ],
        "transactionHash": "0x3a0fda35d511cf590ac5138dd95604cff5a922722b5d1a751c7b20b37ead55d1",
        "transactionPosition": 115,
        "type": "suicide"
    }
```
 
