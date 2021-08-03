---
title: "Hyperledger Aries ACAPY Happy Path"
date: 2021-08-02
toc: true
toc_sticky: true
categories: Project
---

이해를 위해선 ACAPY의 Swagger UI가 필요합니다.   
ACAPY에서 진행할 수 있는 기본적인 Happy Path에 대한 정리입니다.

# Connection (Aries-rfcs 0160)
__두 에이전트 사이에 통신 가능한 채널을 생성해 안전하게 정보를 송수신 할 수 있게 만들어준다.__
1. __inviter__ : Create Invitation 
   
2. __invitee__ : Receive Invitation
    - 1,2 까지는 inviter, invitee 사이에 커넥션이 확립되기 전이기 때문에, out-of-band message 전송이 이루어져야 한다.
    - inviter에서 가지고 있는 invitation을 QR Code 형식으로 invitee에 전달하는 방식 등이 가능하다.
    - inviter와 inviee가 갖고 있는 connection id는 다르다! (GUID)
  
3. __invitee__ : Accept Invitation

4. __inviter__ : Accept Request
    - 1~4 의 과정을 모두 마친 후에 Credential을 해당 connection channel을 통해 송수신할 수 있게 된다.

5. 서로에게 메시지를 하나씩 송수신 하는 경우 수신하는 쪽의 connection의 status가 active로 변경됨.

# Basic Message (Aries-rfcs 0095)

## 메시지 송신
![img.png](../../images/acapy-send-message.png)
- 위에서 확립된 Connection의 id를 통해 메시지를 송신할 수 있다.

## 메시지 수신
- ACAPY agent 실행할 때 `--webhook-url {URL}`을 통해 명시해둔 URL을 통해 이벤트를 받을 수 있다. (POST 요청)
    - basic message의 경우는 `{URL}/topic/basicmessage`로 이벤트가 전송되며 이를 통해 메시지를 수신할 수 있다.
- 송신자도 동일하게 자신이 지정해둔 위치로 이벤트가 전송된다.
- [이 곳](https://github.com/hyperledger/aries-cloudagent-python/blob/main/AdminAPI.md) 을 통해 ACAPY에서 처리 가능한 웹훅에 대해 자세히 알 수 있다.
  


 

> Reference.   
> [aries-rfcs 0160](https://github.com/hyperledger/aries-rfcs/tree/9b0aaa39df7e8bd434126c4b33c097aae78d65bf/features/0160-connection-protocol)   
> [aries-rfcs 0095](https://github.com/hyperledger/aries-rfcs/tree/527849ec3aa2a8fd47a7bb6c57f918ff8bcb5e8c/features/0095-basic-message)   
> [유용한 데모](https://github.com/hyperledger/aries-cloudagent-python/blob/main/demo/AriesOpenAPIDemo.md#start-the-faber-agent-1)   
> https://github.com/hyperledger/aries-cloudagent-python/blob/main/AdminAPI.md   