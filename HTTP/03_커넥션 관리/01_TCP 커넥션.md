- 모든 HTTP 통신은 TCP/IP를 통해 이루어진다.

- 클라이언트 애플리케이션은 서버 애플리케이션으로 TCP/IP커넥션을 맺을 수 있다.

- TCP는 신뢰성이 있으며 순서가 바뀌지 않고 데이터를 전달한다.

- HTTP가 메시지를 전송할 때 세그먼트라는 단위로 데이터 스트림을 잘게 나누고 세그먼트를 IP패킷에 담아 인터넷을 통해 데이터를 전달한다.

- 컴퓨터는 항상 TCP커넥션을 여러 개 가지고 있다. TCP는 포트번호를 통해서 이런 여러 개의 커넥션을 유지한다.

### IP패킷의 구조

- IP패킷 헤더
- TCP  세그먼트 헤더
- TCP데이터 조각

### TCP커넥션의 구성 요소

- 발신지 IP주소
- 발신지 포트
- 수신지 IP주소
- 수신지 포트

---

소켓 API를 사용하면, TCP종단 데이터 구조를 생성하고 원격 서버의 TCP종단에 그 종단 데이터 구조를 연결하여 데이터 스트림을 읽고 쓸 수 있다.

클라이언트와 서버가 TCP 소켓 인터페이스를 사용하여 상호작용하는 방법

서버

- S1 새로운 소켓을 만든다.
- S2 80포트로 소켓을 묶는다.
- S3 소켓 커넥션을 하기한다.
- S4 커넥션을 기다린다.
- S5 애플리케이션 커넥션 통지 => C4
- S6 요청을 읽기 시작한다.
- S7 HTTP 요청 메시지를 처리한다.
- S8 HTTP 응답을 보낸다. => C7
- S9 커넥션을 닫는다.

클라이언트

- C1 IP 주소와 포트를 얻는다.

- C2 새로운 소켓을 생성한다.
- C3 서버의 IP:포트로 연결한다. =>S5
- C4 성공적으로 연결
- C5 HTTP 요청을 보낸다.(write) => S7
- C6 HTTP응답을 기다린다.(read)
- C7 HTTP응답을 처리한다.
- C8 커넥션을 닫는다.

