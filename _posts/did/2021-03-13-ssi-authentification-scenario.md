---
title: "SSI 인증 시나리오"
date: 2021-03-13
toc: true
toc_sticky: true
categories: DID
---

# SSI 인증 시나리오

## 1. DID 생성 및 등록
> 사용자와 각 기관 전부 수행해야 하는 단계입니다

1. DID Auth에 사용할 비대칭키를 생성합니다.
2. 비밀키는 안전하게 보관하고 공개키를 이용해 DID, DID document를 생성합니다.
3. 생성된 DID document를 블록체인 상에 저장합니다.
        
## 2. VC 발급 요청

1. 발급받을 기관에 외부 인증을 거친 후, 요청자의 DID가 식별자로 사용된 VC 발급을 요청합니다.
2. 기관은 요청자의 DID를 이용해 블록체인으로부터 DID Document를 획득합니다.
3. 기관은 DID document 내 포함된 인증키(공개키)를 이용해 Challenge를 생성한 후 사용자에게 __DID Auth__ 를 요청합니다.
4. 요청자는 DID 생성 시 사용했던 비밀키를 이용해 __Challenge__ 에 대한 Response 데이터를 생성한 후 기관에 전송합니다.
5. 요청자가 알맞은 __Response__ 를 전송한다면 기관에선 DID Auth를 완료한 후 요청받은 VC를 발급합니다.

## 3. VP 생성
> 채용 프로세스를 가정합니다.

1. 사용자가 채용사 홈페이지에 접속하여 입사 지원을 신청합니다.
2. 채용사는 사용자에게 필요한 제출서류에 대한 요구사항을 전달합니다.
3. QR code 등을 통해 SSI App으로 전달되며, 자신이 보유한 VC를 이용해 요구사항을 만족하는 VP를 생성합니다.
4. 생성한 VP를 채용사에 제출하고 채용사에서 VP 검증이 시작됩니다.
     
## 4. VP 검증
> 총 3가지에 대해서 검증을 수행할 수 있으며, 그 중 필요로하는 것들만 수행합니다.   
> VP가 ZKP(영지식 증명) VP를 사용하는 경우는 전혀 다른 검증이 과정이 필요합니다.

1. VP에 포함된 __VC의 proof__   
   : VC를 발행한 발행인이 올바른 발행인이며, 해당 VC가 위/변조되지 않았는지 검증
2. VP에 포함된 __VC credentialSubject DID__ 에 대한 DID Auth
   : 해당 VC가 가리키는 사람이 본인이 맞다는 것을 증명하기 위함
3. VP의 __proof__
   : VP를 제출하는 사람이 본인이 맞는지, 사용자로부터 제출된 VP가 위/변조되지 않았는지 검증    
          
## 5. 완료   
 VP 검증이 성공적으로 종료된 경우, 인증 시나리오가 종료됩니다.
        

    

> 참고   
> 윤대근, 자기주권 신원증명 구조 분석서, 제이펍, 2020-07-03   