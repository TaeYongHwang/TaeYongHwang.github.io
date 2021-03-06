---
title: "비트코인 계좌"
date: 2021-01-05
categories: Bitcoin
toc: true
toc_sticky: true
---


# 개인키
- 무작위로 추출, 서명에 사용한다. (256bit)

# 공개키
- 개인키에서 타원곡선 곱셈함수를 이용해 파생된다.
- 비트코인에서 생성 포인트가 동일하기 때문에 같은 개인키면 같은 공개키가 파생된다.

# 주소
- 대부분의 경우 공캐키로부터 생성되지만, 모든 주소가 그렇지는 않다. (스크립트 해시 등)
- 공개키에 SHA256 해시 이후 RIPEMD 160 해싱한 것을 Bask58 check 인코딩해서 얻어진다.

## Base 58
- base64에서 0, 0, L, I, +, / 제외한 58개의 문자로 인코딩

### Base 58 check encoding
1. payload 앞에 version prefix 붙인다.
2. 1의 값에 대해 sha256 해싱을 두번한 후 앞 4 byte를 1의 뒤에 붙인다.
3. 2의 경과값을 Base58 인코딩 시키면 된다.

#### version bype
- base58 check로 인코딩된 값의 prefix에 특정 문자를 포함하게 한다.
- 테스트넷, 메인넷 등 프리픽스가 다른 이유는 각 네트워크별로 넘겨주는 버전바이트가 다르기 때문이다.
