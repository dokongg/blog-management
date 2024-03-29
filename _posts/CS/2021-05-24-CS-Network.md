---
layout:     post
title:      "Part 1-3. 네트워크"
date:       2021-05-24
categories: CS


---


1. **HTTP GET/POST**   
   1.1. HTTP Request   
   1.2. HTTP Response   
   1.3. GET   
   1.4. POST   


2. **TCP 3-way handshake**   
   2.1. TCP 3-way Handshake & 4-way Handshake   
   2.2. 2-way가 아닌 3-way Handshake를 사용하는 이유   


3. **TCP와 UDP**   
   3.1. TCP/IP의 중요한 성질   
   3.2. UDP(User Datagram Protocol, 사용자 데이터그램 프로토콜)   
   3.3. TCP(Transmission Control Protocol, 전송제어 프로토콜)   

4. **HTTP와 HTTPS**   
   4.1. HTTP의 문제점   
   4.2. HTTPS     


5. **DNS Round Robin 방식**   
   5.1. DNS Round Robin 방식의 문제점   
   5.2. DNS 스케줄링 알고리즘   


6. **웹 통신의 큰 흐름**   
  6.1. in 브라우저   
  6.2. in 프로토콜 스택, LAN 어댑터   
  6.3. in 허브, 스위치, 라우터   
  6.4. in 액세스 회선, 프로바이더   
  6.5. in 방화벽, 캐시서버   
  6.6. in 웹서버    

- - -

#### **1. HTTP의 GET과 POST 비교**   
##### **1.1. HTTP Request**
```HTML
Request Line
*(( general-header | request-header | entity-header ) CRLF)
CRLF
[ message-body ]
```

(1) Request Line   
Method SP Request-URI SP HTTP-Version CRLF   
`GET /index.html HTTP/1.1`

- Method
  + OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE, CONNECT, PATCH
- REQUEST-URI
- HTTP-Version
  + "HTTP" "/" 1*DIGIT "." 1"DIGIT
  + HTTP/1.0, HTTP/1.1
- CRLF
  + CR: carriage return(13)
  + LF: linefeed(10)

(2) Headers   
general-header, request-header, entity-header
- 요청에 따라 필요한 헤더만 사용
- name: content 형식
- General Header
  + Cache-Control, Connection, Date, Pragma, Trailer, Transfer-Enco, Upgrade, Via, Warning
- Request Header
  + Accept, Accept-Charset, Accept-Encoding, Accept-Language, Authorization, Expect, From, Host, If-Match, If-Modified-Since, If-None-Match, If-Range, If-Unmodified-Since, Max-Forwards, Proxy-Authorization, Range, Referer, TE, User-Agent 등
- Entity Header
  + Allow, Content-Encoding, Content-Language, Content-Length, Content-Location, Content-MD5, Content-Range, Content-Type, Expires, Last-Modified, extension-header

##### **1.2. HTTP Response**
```HTML
Status-Line
*(( general-header | response-header | entity-header ) CRLF )
CRLF
[ message-body ]
```

(1) Status Line   
HTTP-Version SP Status-Code SP Reason-Phrase CRLF   
`HTTP/1.1 200 OK`

- Status-Code
  + 1xx: 정보성
  + 2xx: 성공
  + 3xx: 리다이렉트
  + 4xx: 클라이언트 오류
  + 5xx: 서버 오류

##### **1.3. GET**   
- 요청하는 데이터가 `HTTP Request Message`의 Header 부분에 url이 담겨 전송된다.
  + url 상에 `?` 뒤에 데이터가 붙어 request를 보내게 된다.
  + url이라는 공간에 데이터가 담겨가기 때문에 전송할 수 있는 데이터의 크기가 제한적이다.
  + 데이터가 그대로 url에 노출되므로 보안이 필요한 데이터에 대해서는 적절하지 않은 방식이다.
- 서버에서 어떠한 데이터를 가져와 보여주는 용도이므로, 서버의 값이나 상태 등을 변경하지 않는다.
  + `SELECT`의 성격
- GET 방식의 요청은 브라우저에서 Caching 할 수 있다.

##### **1.4. POST**
- `HTTP Request Message`의 Body 부분에 데이터가 담겨서 전송된다.
  + 바이너리 데이터를 요청하는 경우 POST 방식으로 보낸다.
- 서버의 값이나 상태를 변경 또는 추가하지 위해서 사용된다.



#### **2. TCP 3-way Handshake**
##### **2.1. TCP 3-way Handshake & 4-way Handshake**
(1) 연결 성립(Connection Establishment)   

![Alt Text](https://ars.els-cdn.com/content/image/3-s2.0-B0122272404001878-gr10.gif)

- 클라이언트가 서버에게 접속을 요청(`SYN`)한다.
  + 클라이언트는 서버에게 접속을 요청하는 `SYN(a)` 패킷을 보낸다.
- 서버가 클라이언트의 요청(`SYN`)을 수락한 후, 확인 메시지(`ACK`/`SYN`)을 보낸다.
  + 서버는 클라이언트의 요청인 `SYN(a)`를 받은 후, 클라이언트에게 요청을 수락한다는 `ACK(a+1)`과 `SYN(b)`가 설정된 패킷을 발송한다.
- 클라이언트가 서버의 응답(`ACK`/`SYN`)을 받아 다시 서버로 확인 메시지(`ACK`)를 보낸다.
  + Client: 서버의 수락 메시지인 `ACK`(a+1)과 `SYN`(b) 패킷을 받고 `ACK`(b+1)을 서버로 보낸다.
- 연결이 성립(establish)된다.

(2) 연결 해제(Connection Termination)   

![Alt Text](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile9.uf.tistory.com%2Fimage%2F21201133536D8D992FC07A)
- 클라이언트가 연결을 종료하겠다는 `FIN 플래그`를 전송한다.
- 서버는 클라이언트의 요청(`FIN`)을 받고 알겠다는 확인 응답(`ACK`)를 보낸다.
  + 데이터를 모두 보내기 전 까지 잠시 `TIME_OUT`이 된다.
- 데이터를 모두 보내고 통신이 끝난 후, 클라이언트에게 연결이 종료되었다는 `FIN 플래그`를 전송한다.
- 클라이언트는 `FIN` 메시지를 확인 했다는 메시지(`ACK`)를 보낸다.
- 클라이언트의 `ACK` 메시지를 받은 서버는 소켓 연결을 close 한다.
- 클라이언트는 아직 서버로부터 받지 못한 데이터가 있을 것을 대비해 일정 시간 동안 세션을 남겨놓고 잉여 패킷을 기다리는 과정을 거친다(`TIME_WAIT`).


> **SYN Packet과 ACK Packet**
>
SYN(Synchronize Sequence Number)
ACK(Acknowledgement)
>
(1) TCP Header의 Code Bit(Flag bit)
- 6Bit로 구성되어 있으며, 각각의 bit들은 Urg-Ack-Psh-Rst-Syn-Fin 순서로 되어 있다.
- 해당 위치의 비트가 1이면, 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다.
  + SYN: 000010
  + ACK: 010000   
>
(2) 두 종류의 Packet을 사용하는 이유
- 요청과 응답에 대한 패킷을 주고 받아야 하기 때문이다.
>
(3) 임의의 sequence number를 사용하는 이유
- 처음 클라이언트에서 SYN 패킷을 보낼 때, Sequence Number에는 랜덤한 숫자가 담겨진다. 초기 sequence number를 ISN이라고 한다. ISN이 0부터 시작하지 않고 난수를 생성해서 number를 설정하게 된다.
- Connection을 맺을 때 사용하는 포트(port)는 유한 범위 내에서 사용하고 시간이 지나면 재사용된다. 따라서 두 통신 호스트가 과거에 사용된 포트 번호 쌍을 사용할 가능성이 있다.
  + 서버 특에서는 패킷의 SYN을 보고 패킷을 구분하게 되는데, 난수가 아닌 순차적인 number가 전송된다면 이전의 connection으로 부터 오는 패킷으로 인식할 수 있는 문제점이 있다.


##### **2.2. 2-way가 아닌 3-way Handshake를 사용하는 이유**
- TCP connection은 양방향성(bidirectional) 연결이기 때문에, 클라이언트가 서버에게 존재를 알리고 패킷을 보낼 수 있다는 것을 알리듯 서버 또한 클라이언트에게 존재를 알리고 패킷을 보낼 수 있다는 신호를 보내야 한다.



#### **3. TCP와 UDP**
##### **3.1. TCP/IP의 중요한 성질**
(1) Connection oriented   
- 두 개의 엔드포인트(로컬, 리모트) 사이에 연결을 먼저 맺고 데이터를 주고 받는다.
- TCP 연결 식별자
  + 두 엔드 포인트의 주소를 합친 것
  + <로컬 IP 주소, 로컬 포트번호, 리모트 IP 주소, 리모트 포트번호>   

(2) Bidirectional byte stream
- 양방향 데이터 통신을 하고, 바이트 스트림을 사용한다.

(3) In-order delivery
- 송신자(sender)가 보낸 순서대로 수신자(receiver)가 데이터를 받는다. 이를 위해서는 데이터의 순서가 필요하다. 순서를 표시하기 위해 32-bit 정수 자료형을 사용한다.

(4) Reliability through ACK
- 데이터를 송신하고 수신자로부터 ACK(데이터 받았음)를 받지 않으면, 송신자 TCP가 데이터를 재전송 한다.
  + 따라서 송신자 TCP는 수신자로부터 ACK를 받지 않은 데이터를 보관한다(buffer unacknowledged data).

(5) Flow control
- 송신자는 수신자가 받을 수 있는 만큼 데이터를 전송한다.
  + 송신자는 수신자 receive window가 허용하는 바이트 수만큼 데이터를 전송한다.
- 수신자가 자신이 받을 수 있는 바이트 수 (사용하지 않은 버퍼 크기, receive window)를 송신자에게 전달한다.

(6) Congestion control
- 네트워크에 유입되는 데이터 양을 제한하려는 목적으로(네트워크 정체를 방지하기 위해) receive window와 별도로 congestion window를 사용한다.
- receive window와 마찬가지로 congestion window가 허용하는 바이트 수 만큼의 데이터를 전송한다.
  + 여기에는 TCP Vegas, Westwood, BIC, CUBIC 등의 다양한 알고리즘이 있다.
- Flow control과 달리 송신자가 단독으로 구현한다.


##### **3.2. UDP(User Datagram Protocol, 사용자 데이터그램 프로토콜)**
- 비연결형 프로토콜
- IP 데이터그램을 캡슐화하여 보내는 방법과 연결 설정을 하지 않고 보내는 방법을 제공
- 흐름제어, 오류제어 또는 손상된 세그먼트의 수신에 대한 재전송을 하지 않는다.
  + 사용자 프로세스의 몫이다.
- 포트들을 사용하여 IP 프로토콜에 인터페이스를 제공한다.
- 클라이언트 - 서버 상황에서 특별히 유용하다.
  + 클라이언트는 서버로 짧은 요청을 보내고, 짧은 응답을 기대한다.
- 코드가 간단할 뿐만 아니라, TCP처럼 초기설정(initial setup)이 요구되는 프로토콜보다 적은 메시지가 요구된다.
  만약 요청 또는 응답이 손실된다면, 클라이언트는 time out 되고, 다시 시도할 수 있으면 된다.
- 예시
  + DNS
    + 어떤 호스트 네임의 IP 주소를 찾아야 하는 프로그램은 DNS 서버로 호스트 네임을 포함한 UDP 패킷을 보낸다. 이 서버는 호스트의 IP 주소를 포함한 UDP 패킷을 응답한다.
    + 사전에 설정이 필요하지 않으며, 그 후에 해제가 필요하지 않다.

##### **3.3. TCP(Transmission Control Protocol, 전송제어 프로토콜)**
- 대부분의 인터넷 응용 분야들이 신뢰성과 순차적인 전달을 요구하는 필요성으로 인해 탄생
- 신뢰성이 없는 인터넷을 통해 종단간에 신뢰성 있는 바이트 스트림을 전송하도록 특별히 설계되었다.
- 송신자와 수신자 모두가 소켓이라고 부르는 종단점을 생성함으로써 이루어진다.
- TCP에서 연결 설정(connection establishment)는 `3-way handshake`를 통해 이루어진다.
- 모든 TCP 연결은 전이중(full-duplex), 점대점(point-to-point) 방식이다.
  + 전이중(full-duplex): 전송이 양방향으로 동시에 일어날 수 있음
  + 점대점(point-to-point): 각 연결이 정확히 2 개의 종단점을 가지고 있음
- 멀티캐스팅이나 브로드캐스팅을 지원하지 않는다.



#### **4. HTTP와 HTTPS**   
##### **4.1. HTTP의 문제점**
- HTTP는 평문 통신이기 때문에 도청이 가능하다.
- 통신 상대를 확인하지 않기 때문에 위장이 가능하다.
- 완전성을 증명할 수 없기 때문에 변조가 가능하다.

(1) TCP/IP는 도청 가능한 네트워크이다.
- TCP/IP 구조의 통신은 전부 통신 경로 상에서 엿볼 수 있다.
  + 패킷을 수집하는 것만으로도 도청할 수 있다.
  + 평문으로 통신을 할 경우, 메시지의 의미를 파악할 수 있기 때문에 암호화하여 통신해야 한다.

- 보완 방법
  + 통신 자체를 암호화
    + SSL(Secure Socket Layer) or TLS(Transport Layer Security)라는 다른 프로토콜을 조합함으로써 HTTP 통신 내용을 암호화할 수 있다(HTTPS, Http over SSL).
  + 콘텐츠를 암호화
    + HTTP를 사용해서 운반하는 내용인, HTTP 메시지에 포함되는 콘텐츠만 암호화하는 것이다.
    + 암호화해서 전송하면 받은 측에서는 그 암호를 해독하여 출력하는 처리가 필요하다.   

(2) 통신 상대를 확인하지 않기 때문에 위장이 가능하다.
- HTTP에 의한 통신에서는 상대가 누구인지 확인하는 처리가 없기 때문에 누구든지 request를 보낼 수 있다.
- IP 주소나 포트 등에서 그 웹 서버에 액세스 제한이 없는 경우, request가 오면 상대가 누구든지 무언가의 response를 반환한다.
- 문제점
  + 리퀘스트를 보낸 곳의 웹 서버가 원래 의도한 response를 보내야 하는 웹 서버인지를 확인할 수 없다.
  + response를 반환한 곳의 클라이언트가 원래 의도한 request를 보낸 클라이언트인지 확인할 수 없다.
  + 통신하고 있는 상대가 접근이 허가된 상대인지를 확인할 수 없다.
  + 어디에서 누가 request 했는 지 확인할 수 없다.
  + 의미없는 request도 수신한다(Dos 공격을 방지할 수 없다).

- 보완 방법
  + SSL 사용

> **SSL**   
상대를 확인하는 수단으로 증명서를 제공한다.
- 증명서는 신뢰할 수 있는 제 3자 기관에 의해 발행되는 것이기 때문에, 서버나 클라이언트가 실재한다는 사실을 증명한다.
- 이 증명서를 이용함으로써 통신 상대가 내가 통신하고자 하는 서버임을 나타내고 이용자는 개인 정보 누설 등의 위험성이 줄어들게 된다.
- 이 증명서로 본인 확인을 하고 웹 사이트 인증에서도 이용할 수 있다.

(3) 완전성을 증명할 수 없기 때문에 변조가 가능하다.
- 완전성: 정보의 정확성
- 서버 또는 클라이언트에서 수신한 내용이 송신측에서 보낸 내용과 일치한다는 것을 보장할 수 없다.
- request나 response가 발신되고 나서 상대가 수신하는 사이에 누군가에 의해 변조되어도 이를 알 수 없다.
  + 중간자 공격(Main-in-the-Middle): 공격자가 도중에 request나 response를 빼앗아 변조하는 공격

- 보완 방법
  + HTTPS 사용


##### **4.2. HTTPS**   
HTTP에 암호화와 인증, 그리고 완전성 보호를 더한 HTTPS
- SSL의 껍질을 덮어쓴 HTTPS
  + 새로운 애플리케이션 계층의 프로토콜이 아니다.
  + HTTP 통신하는 소켓 부분을 SSL(Secure Socket Layer) or TLS(Transport Layer Security)라는 프로토콜으로 대체하는 것 뿐이다.
- TCP와 직접 통신하는 HTTP와 다르게, HTTPS에서 HTTP는 SSL과 통신하게 되고 이 SSL이 TCP와 통신하게 된다.
- SSL을 사용한 HTTPS는 암호화와 증명서, 안전성 보호를 이용할 수 있게 된다.
- HTTPS의 SSL에서는 공통키 암호화 방식과 공개키 암호화 방식을 혼합한 하이브리드 암호 시스템을 이용한다.
  + 공통키를 공개키 암호화 방식으로 교환한 다음에 그 이후의 통신은 공통키 암호를 사용하는 방식


#### **5. DNS Round Robin 방식**   
##### **5.1. DNS Round Robin 방식의 문제점**   
- 서버의 수 만큼 공인 IP 주소가 필요하다.
  + 부하 분산을 위해 서버의 대수를 늘리려면 그 만큼의 공인 IP가 필요하다.
- 균등하게 분산되지 않는다.
  + 스마트폰의 접속은 캐리어 게이트웨이라고 하는 프록시 서버를 공유한다. 프록시 서버에는 이름변환 결과가 일정 시간 동안 캐싱되므로, 같은 프록시 서버를 경유하는 접속은 항상 같은 서버로 접속된다.
  + PC용 웹 브라우저도 DNS 질의 결과를 캐싱하기 때문에 균등하게 부하분산 되지 않는다.
    + DNS 레코드의 TTL 값을 짧게 설정함으로써 어느 정도 해소가 되지만, TTL에 따라 캐시를 해제하는 것은 아니므로 반드시 주의가 필요하다.
- 서버가 다운되도 확인 불가하다.
  + DNS 서버는 웹 서버의 부하나 접속 수 등의 상황에 따라 질의 결과를 제어할 수 없다.
  + 웹 서버의 부하가 높아서 응답이 느려지거나 접속 수가 꽉 차서 접속을 처리할 수 없는 상황인지를 전혀 감지할 수 없기 때문에 어떠한 원인으로 다운되더라도 이를 검출하지 못하고 유저들에게 제공한다.
    + 이 때문에 유저들은 간혹 다운된 서버로 연결이 되기도 한다.

##### **5.2. DNS 스케줄링 알고리즘**   
(1) Weighted Round Robin(WRR)
- 각각의 웹 서버에 가중치를 가미해서 분산 비율을 변경한다.
- 가중치가 큰 서버일수록 빈번하게 선택되므로, 처리 능력이 높은 서버는 높게 설정하는 것이 좋다.   

(2) Least connection
- 접속 클라이언트의 수가 가장 적은 서버를 선택한다.
- 로드밸런서에서 실시간으로 connection 수를 관리하거나 각 서버에서 주기적으로 알려주는 것이 필요하다.



#### **6. 웹 통신의 큰 흐름**   
Chrome을 실행시켜 주소창에 특정 URL 값을 입력했을 때 일어나는 일

##### **6.1. in 브라우저**  
- url에 입력된 값을 브라우저 내부에서 결정된 규칙에 따라 그 의미를 조사한다.
- 조사된 의미에 따라 HTTP Request 메시지를 만든다.
- 만들어진 메시지를 웹 서버로 전송한다.   

이 때 만들어진 메시지 전송은 브라우저가 직접하는 것이 아니다. 브라우저는 메시지를 네트워크에 송출하는 기능이 없으므로, OS에 의뢰하여 메시지를 전달한다.   
단, OS에 송신을 의뢰할 때는 도메인 명이 아니라 ip 주소로 메시지를 받을 상대를 지정해야 하는데, 이 과정에서 DNS 서버를 조회해야 한다.


##### **6.2. in 프로토콜 스택, LAN 어댑터**
- 프로토콜 스택(운영체제에 내장된 네트워크 제어용 소프트웨어)이 브라우저로부터 메시지를 받는다.
- 브라우저로부터 받은 메시지를 패킷 속에 저장한다.
- 수신처 주소 등의 제어정보를 덧붙인다.
- 패킷을 LAN 어댑터에 넘긴다.
- LAN 어댑터는 다음 Hop의 MAC 주소를 붙인 프레임을 전기신호로 변환시킨다.
- 신호를 LAN 케이블에 송출시킨다.   

프로토콜 스택은 통신 중 오류가 발생했을 때, 이 제어 정보를 사용하여 고쳐 보내거나 각종 상황을 조절하는 등 다양한 역할을 하게 된다.   
네트워크 세계에는 비서가 있어서 우리가 비서에게 물건만 건네주면, 받는 사람의 주소와 각종 유의사항을 써준다(프로토콜 스택 = 비서).


##### **6.3. in 허브, 스위치, 라우터**
- LAN 어댑터가 송신한 프레임은 스위칭 허브를 경유하여 인터넷 접속용 라우터에 도착한다.
- 라우터는 패킷을 프로바이더(통신사)에게 전달한다.
- 인터넷으로 들어가게 된다.


##### **6.4. in 액세스 회선, 프로바이더**
- 패킷은 인터넷의 입구에 있는 액세스 회선(통신 회선)에 의해 POP(Point Of Presence, 통신사용 라우터)까지 운반된다.
- POP를 거쳐 인터넷의 핵심부로 들어가게 된다.
- 수 많은 고속 라우터들 사이로 패킷이 목적지를 향해 흘러가게 된다.


##### **6.5. in 방화벽, 캐시서버**
- 패킷은 인터넷 핵심부를 통과하여 웹 서버측의 LAN에 도착한다.
- 기다리고 있던 방화벽이 도착한 패킷을 검사한다.
- 패킷이 웹 서버까지 가야하는지, 아니면 말아야 하는 지를 판단하는 캐시서버가 존재한다.   

굳이 서버까지 가지 않아도 되는 경우를 골라낸다. 액세스 한 페이지의 데이터가 캐시서버에 있으면 웹 서버에 의뢰하지 않고도 바로 그 값을 읽을 수 있다. 페이지의 데이터 중에 다시 이용할 수 있는 것이 있으면 캐시 서버에 저장한다.   


##### **6.6. in 웹서버**
- 패킷이 물리적인 웹 서버에 도착하면, 웹 서버의 프로토콜 스택은 패킷을 추출하여 메시지를 복원하고 웹 서버 애플리케이션에게 넘긴다.
- 메시지를 받은 웹 서버 애플리케이션은 요청 메시지에 따른 데이터를 응답 메시지에 넣어 클라이언트로 회송한다.
- 왔던 방식대로 응답 메시지가 클라이언트에게 전달된다.




참고
- <https://github.com/JaeYeopHan/Interview_Question_for_Beginner>
- <https://blog.outsider.ne.kr/888>
- <https://asfirstalways.tistory.com/356>
- <https://d2.naver.com/helloworld/47667>
