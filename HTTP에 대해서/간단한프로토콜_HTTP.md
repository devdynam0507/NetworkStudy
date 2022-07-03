# Request, Response
HTTP는 클라이언트로부터 Request가 송신되며, 그 결과가 서버로부터 Response로 되돌아옵니다.  
즉 클라이언트 측으로부터 통신이 시작됩니다.  
# Request 구조
```text
POST /form/entry HTTP/1.1
// Request 헤더 필드
Host: naver.com
Connection: keep-alive
Content-Type: application/json
Content-Length: 16

name=dynam&age=23
```
- POST: 메소드
- /form/entry: URI
- HTTP/1.1: 프로토콜 버전
Request 메시지는 메소드, URI, 프로토콜 버전, 옵션 리퀘스트 헤더 필드와 엔티티로 구성되어 있습니다.  
# Response 구조
Request를 받은 서버는 결과를 Response로 클라이언트에게 돌려줍니다.  
```text
HTTP /1.1 200 OK
Date: Tue, 10 Jul 2022 06:50:15 GMT
Content-Length: 362
Content-Type: text/html

<html>
...
```
첫 번째 줄을 보면 HTTP /1.1은 서버의 HTTP 버전을 나타냅니다.  
그리고 200 OK는 Request의 처리 결과를 나타내는 상태 코드와 설명입니다.  
다음줄은 Response가 발생한 일시를 나타내고 있는데 헤더 필드라고 불리는 것중 하나입니다.  
그리고 빈 줄로 구분하는데 그 아래에 있는 부분이 body라고 불리는 리소스 본체가 됩니다.  
기본적으로 Response 메시지는 프로토콜 버전, 상태 코드와 그 상태 코드를 설명한 프레이즈.  
옵션의 Response 헤더 필드와 바디로 구성되어 있습니다.  
# Stateless (상태를 유지하지 않는 프로토콜)
HTTP는 상태를 계속 유지하지 않는 Stateless 프로토콜입니다.  
Request, Response를 교환하는 동안에 상태를 관리하지 않습니다.  
결국 HTTP 프로토콜 레벨에서는 이전에 보냈던 Request나 이미 되돌려준 Response에 대해서는 전혀 기억하지 않습니다.  
HTTP 프로토콜에서는 과거의 Request나 Response 정보를 전혀 가지고 있지 않습니다.  
그러나 웹이 진화함에 따라 상태를 유지할 필요성이 생기게 되었습니다.(로그인 기능 등)  
이 상태를 유지하고 싶은 요구에 부응하기 위해 Cookie라는 기술이 도입되었습니다.  
쿠키로 인해 HTTP를 이용한 통신에서도 상태를 계속 관리할 수 있게 되었습니다.  
# Request URI
HTTP는 URI를 사용하여 인터넷 상의 리소스를 지정합니다.  
- http://www.usagidesign.jp/photo/usagi.htm
- http://naver.com
URI를 지정하는 방법에는 여러 종류가 있습니다.  
### 모든 Request를 URI에 포함한다.
```
GET http://hackr.jp/index.html HTTP/1.1
```
### Host 헤더 필드에 네트워크 로케이션을 포함한다.
```
GET /index.htm HTTP/1.1
Host: hackr.jp
```
이것 외에도 특정 리소스가 아닌 서버 자신에게 Request를 송신하는 경우에는 Request URI에 ```*```를 지정할 수 있습니다.  
```
OPTIONS * HTTP/1.1
```
# HTTP 메소드
## GET: 리소스 획득
GET 메소드는 리퀘스트 URI로 식별된 리소스를 가져올 수 있도록 요구합니다.  
## POST: 엔티티 전송
POST 메소드는 엔티티를 전송하기 위해서 사용됩니다.  
GET으로도 엔티티를 전송할 수 있지만 자주 사용하지 않고 일반적으로 POST를 사용합니다.  
## PUT: 데이터 갱신, 작성
PUT은 데이터를 갱신하거나 작성할 떄 사용됩니다.  
주로 데이터를 업데이트 할 떄 사용됩니다.  
## HEAD: 메시지 헤더 취득
HEAD 메소드는 GET과 같은 기능이지만 메시지 바디는 돌려주지 않습니다.  
## DELETE: 삭제
리소스를 삭제할 때 사용됩니다.  
## OPTIONS: 제공하고 있는 메소드의 문의
OPTIONS 메소드는 Request URI로 지정한 리소스가 제공하고 있는 메소드를 조사하기 위해 사용됩니다.  
# 지속 연결로 통신량 절약
초기 HTTP 버전에서는 통신을 한 번 할때마다 TCP에 의해 연결과 종료를 할 필요가 있었습니다.  
초기 당시의 통신에서는 작은 사이즈의 텍스트를 보내는 정도였기 때문에 이렇게 기능을 구현해도 문제는 없었습니다.  
그러나 HTTP가 널리 보급되어감에 따라 다량의 이미지를 포함한 문서 등이 늘어났습니다.  
예를 들면 하나의 HTML에 여러 이미지가 포함되어 있는 경우 브라우저를 사용해서 Request를 하면 HTML 문서에 포함되어 있는 이미지를 획득하기 위해서 여러 Request를 송신합니다.  
그렇기 때문에 리퀘스트를 보낼 때마다 매번 TCP 연결과 종료를 하게 되는 쓸모없는 일이 발생되어 통신량이 늘어나게 됩니다.  
## 지속 연결
그래서 HTTP 1.1과 일부 1.0버전에서는 TCP 연결 문제를 해결하기 위해서 지속 연결이라는 방법을 고안하였습니다.  
지속 연결의 특징은 어느 한 쪽이 명시적으로 연결을 종료하지 않는 이상 TCP 연결을 계속 유지합니다.  
(첫 요청시에만 3-way-handshake 발생 그리고 마지막 한번만 4-way-handshake 발생)  
## 파이프라인화
지속 연결은 여러 Request를 보낼 수 있도록 파이프라인화를 가능하게 합니다.  
파이프라인화에 의해서, 이전에는 Request 송신 후에 Response를 수신할 때까지 기다린 뒤에 Request를 발행하던 것을, Response를 기다리지 않고  
바로 다음 Request를 보낼 수 있습니다.  
이로 인해, 여러 Request를 병행해서 보내는 것이 가능하기 때문에 일일이 Response를 기다릴 필요가 없습니다.  
## 쿠키를 사용한 상태 관리
HTTP는 Stateless한 프로토콜이기 때문에 과거에 교환했었던 Request, Response의 상태를 관리하지 않습니다.  
결국, 과거 상태를 근거로해서 현재 Request를 처리한다는 것은 불가능합니다.  
예를들어 인증이 필요한 웹 페이지에서 상태 관리를 하지 않는다면 인증을 마친 상태를 잊어버리기 떄문에 새로운 페이지로 이동할 떄마다  
재차 로그인정보를 보내든지 Request마다 매개 변수나 추가 정보를 붙여서 로그인 상태를 관리해야 하는 상황이 발생하지만  
Stateless한 프로토콜또한 이점이 없는것은 아닙니다.  
아무래도 상태를 관리하지 않는다는 점에서 서버의 리소스 (CPU, MEMORY)사용을 억제할 수 있긴합니다.  
어찌됬건 이 문제를 해결하기 위해 Cookie라는 기술이 도입되었고 Set-Cookie라는 헤더 필드에 의해 쿠키를 클라이언트에 보존하게 됩니다.  
다음 번에 클라이언트가 같은 서버로 Request를 보낼 때 자동으로 쿠키 값을 넣어서 송신합니다.  
서버는 클라이언트가 보내온 쿠키를 확인해서 어느 클라이언트가 접속했는지 체크하고 서버 상의 기록을 확인해서 이전 상태를 알 수 있습니다.  
