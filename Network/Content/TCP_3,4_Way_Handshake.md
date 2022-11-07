## 3-Way Handshake와 4-Way Handshake

`3-way handshake`는 `TCP`의 **접속**, `4-Way Handshake`는 `TCP`의 **접속 해제** 과정이다.

- **포트(PORT) 상태 정보**
  - `CLOSED` : 포트가 닫힌 상태
  - `LISTEN` : 포트가 열린 상태로 연결 요청 대기 중
  - `SYN_RCV` : SYNC 요청을 받고 상대방의 응답을 기다리는 중
  - `EXTABLISHED` : 포트 연결 상태

## TCP 3-Way Handshake

TCP는 장치들 사이에 논리적인 접속을 성립하기 위하여 `3-way handshake`를 사용한다.

- TCP 통신을 이용하여 데이터를 전송하기 위해 네트워크 연결을 설정(Connection Establish) 하는 과정
- 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 데이터 전달이 시작학기 전에 한 쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다
- 즉, TCP/IP 프로토콜을 이용해서 통신을 하는 응용 프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립한느 과정을 의미한다

### 작동 방식

간단하게 `표현하면`

- A → B : 내 말 들려 ?
- B → A : 잘 들려, 내 말은 들려 ?
- A → B : 잘 들려

![Untitled](https://user-images.githubusercontent.com/101804857/200210061-b399c1c6-834a-4b48-9538-0734b69011a4.png)

- `SYN` (synchronize sequence numbers) : 연결 확인을 위해 보내는 무작위의 숫자값 (내 말 잘 들려 ?)
- `ACK` (acknowledgements) : Client 혹은 Server로부터 받은 SYN에 1을 더해 SYN을 잘 받았다는 ACK (잘 들려)

**Step 1 (SYN)**

**클라이언트는 서버와 커넥션을 연결하기 위해 `SYN`을 보낸다. (seq : x)**

- 송신자가 최초로 데이터를 전송할 때 Sequence Number를 읨의로 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- PORT 상태
  - Client : `CLOSED` - `SYN_SENT`로 변함
  - Server : `LISTEN`

**Step 2 (SYN + ACK)**

**서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 `ACK`와 `SYN` 패킷을 보냄 (seq : y, ACK : x + 1)**

- 접속 요청을 받은 Q가 요청을 수락했으며, 접속 요청 프로세스인 P도 포트를 열어달라는 메세지를 전송 (SYN-ACK signal bits set)
- ACK Number 필드를 Sequence Number + 1로 지정하고 SYN과 ACK 플래그 비트를 1로 설정한 세그먼트 전송 ( Seq=y, Ack=x+1, SYN, ACK)
- PORT 상태
  - Client : `CLOSED`
  - Server : `SYN_RCV`

**Step 3 (ACK)**

**클라이언트는 서버의 응답은 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄**

- 마지막으로 접속 요청 프로세스 P가 수락 확인을 보내 연결을 맺음 (ACK)
- 이때, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.
- PORT 상태
  - Client : `ESTABLISHED`
  - Server : `SYN_RCV` ⇒ ACK ⇒ `ESTABLISHED`

## TCP 3-Way Handshake

`3-way handshake`와 반대로 가상 회선 연결을 해제할 때 주고 받는 확인 작업이다. 여기서는 `FIN` 플래그를 이용한다.

- `FIN` (finish) : 세션을 종료시키는데 사용되며, 더 이상 보낸 데이터가 없음을 의미한다.

### 작동 방식

간단하게 표현하면

- A → B : 나는 다 보냈어. 이제 끊자!
- B → A : 알겠어! 잠시만~
- B → A : 나도 끊을게!
- A → B : 알겠어 !

![Untitled (1)](https://user-images.githubusercontent.com/101804857/200210135-c15067d0-e737-4cc2-af2b-2cc6355c1215.png)

**STEP1 (Client → Server : FIN(+ACK)**

- 서버와 클라이언트가 연결된 상태에서 클라이언트가 close()를 호출하여 접속을 끊는다.(으려한다.)
- 이때, 클라이언트는 서버에게 연결을 종료한다는 `FIN` 플래그를 보낸다.
  - 이때 FIN 패킷에는 실질적으로 ACK도 포함되어있다.

**STEP2 (Server → Client : ACK)**

- 서버는 FIN을 받고, 확인했다는 `ACK`를 클라이언트에게 보내고 자신의 통신이 끝날때까지 기다린다. (이상태가 TIME_WAIT 상태)
  - Server(수신자)는 ACK Number 필드를 (Sequence Number + 1)로 지정하고, ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- 서버는 클라이언트에게 응답을 보내고 **`CLOSE_WAIT` 상태**에 들어갑니다. 그리고**아직 남은 데이터가 있다면 마저 전송을 마친 후에 close( )를 호출**
- 클라이언트에서는 서버에서 ACK를 받은 후에 서버가 남은 데이터 처리를 끝내고 FIN 패킷을 보낼 때까지 기다리게 됩니다. (**`FIN_WAIT_2`)**

**STEP3 (Server → Client : FIN)**

- 데이터를 모두 보냈다면, 서버는 연결이 종료에 합의 한다는 의미로 `FIN` 패킷을 클라이언트에게 보낸 후에, 승인 번호를 보내줄 때까지 기다니는 `LAST_ACK` 상태로 들어간다.

**STEP4 (Client → Server : ACK)**

- 클라이언트는 FIN을 받고, 확인했다는 `ACK`를 서버에게 보낸다.
- 아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 `TIME_WAI`T을 통해 기다린다. (실질적인 종료과정 `CLOSED`에 들어가게 된다.)
  - 이때 `TIME_WAIT` 상태는 **의도치 않은 에러로 인해 연결이 데드락으로 빠지는 것을 방지**
  - 만약 에러로 인해 종료가 지연되다가 타임이 초과되면 `CLOSED`로 들어갑니다.
- 서버는 ACK를 받은 이후 소켓을 닫는다 (Closed)
- TIME_WAIT 시간이 끝나면 클라이언트도 닫는다 (Closed)
