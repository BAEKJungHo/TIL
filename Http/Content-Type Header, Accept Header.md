# Content-Type

- Content-Type 이란 리소스의 미디어 타입을 나타내기 위해 사용된다.
- 요청(request)에서 Content-Type은 클라이언트가 서버에게 어떤 형식의 데이터가 실제로 보내지는 것인지 알려주는 용도로 사용된다.

> The Content-Type entity header is used to indicate the media type of the resource. In requests, (such as POST or PUT), the client tells the server what type of data is actually sent. (Source: MDN Web Docs)

- Content-Type 헤더는 현재 전송하는 데이터가 어떤 타입인지에 대한 설명을 하는 개념이고
- Accept 헤더는 클라이언트가 서버에게 어떤 특정한 데이터 타입을 보낼때 클라이언트가 보낸 특정 데이터 타입으로만 응답을 해야한다.
