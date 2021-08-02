---
title: "Hyperledger Aries Connection Protocol Happy Path (aries-rfcs 0160)"
date: 2021-08-02
toc: true
toc_sticky: true
categories: Project
---

# Aries-rfcs 0160 Happy Path
이해를 위해선 ACAPY의 Swagger UI가 필요

1. __inviter__ : Create Invitation 
   
2. __invitee__ : Receive Invitation
    - 1,2 까지는 inviter, invitee 사이에 커넥션이 확립되기 전이기 때문에, out-of-band message 전송이 이루어져야 한다.
    - inviter에서 가지고 있는 invitation을 QR Code 형식으로 invitee에 전달하는 방식 등이 가능하다.
    - inviter와 inviee가 갖고 있는 connection id는 다르다! (GUID)
  
3. __invitee__ : Accept Invitation

4. __inviter__ : Accept Request

5. 서로에게 메시지를 하나씩 송수신 하는 경우 수신하는 쪽의 connection의 status가 active로 변경됨.
    

> Reference.   
> [aries-rfcs 0160](https://github.com/hyperledger/aries-rfcs/tree/9b0aaa39df7e8bd434126c4b33c097aae78d65bf/features/0160-connection-protocol)