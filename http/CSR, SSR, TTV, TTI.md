# CSR(Client Side Rendering)

- 서버가 클라이언트에게 index.html 파일을 전송
- index.html 파일 내용은 비어있고 스크립트를 불러오는 코드가 들어있음
- 서버가 index.html 내용을 그리기 위한 비지니스 로직이 들어있는 JS 파일들을 클라이언트에게전송
- 클라이언트가 파일을 수신하면 사용자가 바로 사용가능(TTI, TTV 가능)

> 단점. SEO(server engine optimization) 성능이 떨어진다. index.html 내용이 비어있기 때문이다.

# SSR(Server Side Rendering)

- 서버가 이미 만들어진 html 을 클라이언트에게 전송
- 하지만 아직 JS 가 전송되지 않아서 TTI 불가능, TTV 가능
- 서버가 추가적으로 전송한 JS 파일을 클라이언트가 수신하게 되는 순간 사용자들이 사용가능

> 단점. 화면 깜빡임이 있다. 사용자가 많을 시 과부하가 발생할 수 있다.
