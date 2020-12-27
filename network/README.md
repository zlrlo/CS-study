# Network

## 브라우저의 렌더링 동작과정

1. 유저가 웹 브라우저에 URL을 입력한다. 
2. URL로 지정한 웹 서버에게 HTTP 요청을 송신한다. 
3. 웹 서버가 요청을 해석한다. 
4. 웹 서버가 요청받은 파일을 웹 브라우저에 돌려보낸다. 
5. 웹 브라우저가 수신한 데이터를 해석해서 표시한다.

## URL(Uniform Resource Locator)

- **인터넷상의 `데이터 위치`를 가리키는 표준적인 표기법**
- 스킴://호스트명/패스 ex) http://www.n-study.com/network/index.html
- www.n-study.com 이라는 웹 서버가 디렉토리 network 내의 index.html을 HTTP로 전송하도록 요청하는 내용
- 호스트명 뒤 포트번호가 생략된 경우에는 HTTP의 잘 알려진 포트인 TCP `80번`으로 연결


