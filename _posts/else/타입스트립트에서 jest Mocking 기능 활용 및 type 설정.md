---
title: "타입스트립트에서 jest Mocking 기능 활용 및 type 설정"
date: 2022-08-28
toc: true
toc_sticky: true
categories: Else
---


# 토이 프로젝트를 하면서..
 토이 프로젝트를 진행하면서 타입스크립트로 테스트 코드를 작성하였는데, 이를 진행하며 겪은 문제들을 다시 겪지 않기 위해 관련 이슈를 정리해 두려고 한다.
 
 
 일단 구글에 존재하는 대부분의 자료들이 js 기반이며, 클래스를 mocking하는 정확한 방식을 찾기 힘들었던 문제가 있었다. 
 지금 주로 사용하는 java(+spring)에서는 junit + mockito를 통해 클래스 테스트를 용이하게 할 수 있었는데, javascript에서는 
 클래스를 테스트하는 좋은 설명을 발견하지 못하였다.   
 
 좋은 설명을 발견하지 못했던 이유에 대해 먼저 설명하자면, 내가 자바스크립트를 자바와 동일하게 **클래스를 제공**하는 언어로 생각하였기 때문이었다.   
 
 자바스크립에서 Class란, ECMA6로 넘어오면서 새로 제공하는 **Syntactic Sugar**이다.
 간단하게 말하면, 기존에 존재하는 기능을 사용하기 쉽게 **class** 라는 단어를 사용할 수 있게 해주었다는 것이다. 
 이는 ECMA6 이전에도 어떻게든 클래스를 사용할 수 있었다는 의미이며, 이는 자바스크립트의 prototype 기능을 활용해서 진행되었다. (자세한 설명은 구글링)   
 
## mock 테스트 구현 시 클래스의 메서드를 어떻게 mocking할 수 있을까 
 
 자바스크립트의 클래스가 실제로는 프로토타입으로 제공된다는 사실이 왜 중요한걸까?
 간단하게 코드를 확인해보자.
 ```javascript
 const myMockFn = jest 
    .fn()
    .mockImplementationOnce(cb => cb(null, true))  
    .mockImplementationOnce(cb => cb(null, false));
 ```
 위는 jest 안에 있는 예제 코드이다. 대부분의 예시가 이런 형태인데, 이를 보면 **fn()** 이 눈에 띌 것이다. 결국 jest를 활용하기 위해선 함수 형태로 값을
 모킹해야 한다는 것이다.      
 
 간단하게 시나리오를 하나 생각해보자. **axios** 라는 라이브러리를 사용하는데, 우리가 mocking 할 메서드가 axios.Axios 클래스의 get 메서드라고 해보자
 ```javascript
 // axios/index.d.ts 파일
 export class Axios {
  constructor(config?: AxiosRequestConfig);
  get<T = any, R = AxiosResponse<T>, D = any>(url: string, config?: AxiosRequestConfig<D>): Promise<R>;
  ...
}
 ```
 이를 어떻게 모킹해야 할까?
 1. 일단 jest.mock 메서드를 사용하여 특정 파일에 대해 mock을 설정하겠다고 지정해야 한다.
 2. 클래스는 결국 prototype의 syntactic sugar라고 하였으므로, 이를 생각한다면, `Axios.prototype.get` 형태로 생각해볼 수 있을 것이다.
 3. Promise를 반환하므로, 관련 메서드를 적용하는 게 좋아보인다.   
 
 아래는 이를 적용한 테스트 코드이다.
 ```javascript
 import axios from "axios";
jest.mock("axios"); // axios mock 지정

describe('StudyApis test', () => {
    const sut: StudyApis = new StudyApis();
    const mockedAxios = axios as jest.Mocked<typeof axios>; // 타입스크립트를 위한 axios 타이핑 진행 
    
    it("TEST", async () => { 
      const data = { someData : true };
      mockedAxios.Axios.prototype.get.mockResolvedValueOnce(data); // get이 Promise를 반환하는 메서드이니 mockResolvedValue 사용 
      ...
    
    })
    
})
```


 
 
 
 

 
 

 
 
