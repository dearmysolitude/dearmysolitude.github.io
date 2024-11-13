---
title: "웹서버 Intro - 2"
excerpt: "소켓과 소켓 인터페이스, "

categories:
  - Jungle
tags:
  - proxyserver
  - CS:APP
---
## 소켓과 소켓 인터페이스✨

![Untitled](https://1drv.ms/i/c/c4f97b3b64ae3e7a/IQQsJEB_FtXvRJr6mVTeEOCfAThe0B5FNci73wO0WTFuBFw?width=1024)

![**소켓 인터페이스의 기반 네트워크 응용프로그램의 개요 - 중요✨**](https://1drv.ms/i/c/c4f97b3b64ae3e7a/IQT1kiWEs-B9T6PO2SxvQ3u3AVuUKl77X3nrIsLMu_ndFQ4?width=976&height=676)

**소켓 인터페이스의 기반 네트워크 응용프로그램의 개요**

## 클라이언트-서버 모델

서비스 제공자인 서버와 서비스 요청자인 클라이언트를 분리하여 구성하는 분산 어플리케이션 구조.

## Datagram Socket vs Stream Socket

stream: 흐름 데이터 세트 간에서 데이터 전송이 실행되고 있는 것. 문자 형식의 데이터 항목이 연속한 열로 되어있는 것

datagram: 패킷 교환에서, 데이터 단말 장치와 망과의 사전 접속 절차에 의하지 않고, 하나하나의 패킷이 발신 데이터 단말 장치와 수신처 데이터 단말 장치 간의 경로 지정을 위한 충분한 정보를 가지고 있는 패킷

### Datagram Socket

연결형 스트림 소켓은 두 개의 시스템이 연결된 다음 서로 데이터를 주고받기 시작하여 연결된 상태의 데이터 주고받기가 끝난 다음 연결을 끊게 되는 형식을 TCP프로토콜을 기본으로 한다.

### Stream Socket

### 웹 컨텐츠 / MIME(Multipurpose Internet Mail Extensions)

전자우편을 위한 인터넷 표준 포맷. 이메일과 함께 동봉할 파일을 텍스트 문자로 전환해서 이메일 시스템을 통해 전달하기 위해 개발됨. 현재에는 웹을 통해 여러 형태의 파일 전달하는데 쓰이고 있음. 과거에는 텍스트 파일 주고 받는데에 ASCII로 공통 표준을 따르기만하면 되었지만 바이너리 파일을 보내는 경우가 생기게 됨(미디어 컨텐츠). ASCII만으로는 전송이 불가능하며 이련 바이너리 파일들을 기존의 시스템에서 문제없이 전달하기 위해 텍스트파일로 변환이 필요하였음.

텍스트파일로 변환을 인코딩Encoding, 텍스트 파일을 바이너리 파일로 변환하는 과정을Decoding이라고 한다.

[참고 자료: MIME](https://server-talk.tistory.com/183)


CGI

### HTTP Hyper Text Transfer Protocol

서버와 클라이언트간 데이터가 교환되는 방식. ASCII로 인코딩된 정보이며 여러 줄로 되어있음. ~HTTP/1.1 까지는 클라이언트-서버 사이 연결을 통해 공개적으로 전달되었음
메시지 타입은 다음의 두가지가 있음.

- 요청 Request: 클라이언트가 서버로 전달해서 서버의 액션이 일어나게끔 하는 메시지
- 응답 Response: 요청에 대한 서버의 답변

- HTTP 메시지를 직접 작성하는 경우는 매우 드물다. 대신, 스프트웨어, 브라우저, 프록시 또는 웹 서버가 그 일을 한다. 메시지는 설정파일(프록시 혹은 서버의 경우), API(브라우저의 경우), 혹은ㄷ ㅏ른 인터페이스를 통해 제공된다.
- Stateless: 요청/응답 정보를 저장하지 않음

[HTTP 메세지 - Mozilla Developer](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages
)

### 헤더

[HTTP 헤더 구조](https://12bme.tistory.com/325)

### 상태코드

### 메소드

[HTTP 메소드에 대해 설명해 보세요](https://velog.io/@haron/HTTP-%EB%A9%94%EC%84%9C%EB%93%9C%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%84%EB%8A%94%EB%8C%80%EB%A1%9C-%EB%A7%90%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94)

### HEAD 메소드

GET 메소드의 요청과 동일한 응답을 요구하지만, 응답 본문을 포함하지 않는다. → 포함되더라도, 이를 무시

- 데이터 양이 줄어들기 때문에 빠르게 서버의 상태를 조회할 수 있다.
- 응답 헤더의 Content-Length또한 동일하기 때문에 resource 양에 대한 조회만 할 때에는 HEAD 메소드가 유용할 수 있다.

### HTTP transaction

### HTTP 캐싱

동일한 HTTP 요청이 시도되면 저장해 둔 HTTP 응답을 재활용한다. 응답을 저장하는 저장소는 다음과 같이 두 가지로 분류해볼 수 있다.

![Http Caching](https://1drv.ms/i/c/c4f97b3b64ae3e7a/IQTD79s5_F8tTZgAtb2gWdJFARey4T5O5ypxHJQmwAI_-o4?width=902&height=282)

- 사설 캐시 Private Cache: 한 사용자에 의해서만 재활용될 수 있다. 대표적으로 브라우저 캐시가 있다.
- 지역 캐시 Local Cache: 여러 사용자들에 의해 재활용될 수 있는 것들이 저장되는 캐시. 대표적으로 프록시 캐시가 있다.

#### HTTP 캐시 엔트리(캐싱하는 대상)

HTTP 캐시에 저장되는 데이터 덩어리 각 캐시 엔트리를 구분하는 기준은 캐시키(Cache Key)이다. 

#### 캐시 컨트롤

HTTP 캐싱은 HTTP 응답을 저장해 두고 다음번에 동일한 HTTP 요청이 시도되면 저장해 둔 HTTP 응답을 재활용하는 것이다. 

프록시 서버는 캐시 서버로서의 기능도 담당한다. 웹 컨텐츠마다 캐시가 갱신될 정책을 헤더에 저장할 수 있으므로 이를 기반으로 브라우저에서 정보를 가져올 때 캐시 서버에서 가져올지, 혹은 원래 서버에 요청하여 가져올지 판단한다.

프록시 서버에서 캐시의 유효성 여부를 확인하기 위해 본 서버에서 컨텐츠에 시행하는 캐시 업데이트 정책

[1)](https://it-eldorado.com/posts/f1f0d9a7-1840-45e6-8340-2d45a4c3becc)


## 프록시 서버

[VPN 사업자가 설명하는 프록시 서버](https://surfshark.com/ko/blog/proxy-server)

중간 서버 역할.

### 프록시 서버의 기능

1. **보안**: 서버의 IP를 숨기는 것이 가능하고 이는 외부로부터의 위험을 막아주는 역할을 한다.
2. 익명성(우회): 클라이언트의 IP가 프록시 서버의 IP로 바뀌어 어떤 IP가 접근하는지 숨길 수 있다.
3. 캐시: 캐시 서버로의 기능
4. 인터넷 사용 제어

### 프록시 서버의 종류: 프록시 서버의 운영 주체와 그 위치에 따름

#### 포워드 프록시Forward Proxy

주로 이더넷 등의 내부망에 사용. 보안을 위해 사용된다. 

![forward-proxy](https://1drv.ms/i/c/c4f97b3b64ae3e7a/IQSwdRjdBWGbTawqJiRKXpbcAfBBopsgtWVdgPgm6BnMf1E?width=780&height=283)

#### 리버스 프록시Reverse Proxy

![reverse-proxy](https://1drv.ms/i/c/c4f97b3b64ae3e7a/IQTjeJ7j185fQJl950uMatAjAe-gDgxDzc9Ib0FZhgnNV6Y?width=810&height=287)

프록시 구축은 망을 제공하는 사업자일 수도 있고, 혹은 LAN이나 이더넷을 통해 내부망을 구축한 망 사용 주체(주로 산업체)일 수도 있다.