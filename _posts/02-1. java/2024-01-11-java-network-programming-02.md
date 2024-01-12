---
title: "자바 네트워크 프로그래밍 02"
excerpt: "자바로 네트워크 프로그래밍 하기"

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - 네트워크
  - Network
---
## 웹 서버의 동작

동시에 여러 번 동작한다.
- 클라이언트가 접속할때까지 대기
- 클라이언트가 접속하면 연결이 된다.
- 클라이언트가 보내주는 정보를 읽는다(빈 줄 까지).
    - 요청 메세지(GET /)
    - 헤더(헤더명: 헤더 값) ⇐⇐⇐⇽ multiple
    - 빈 줄
- 서버는 응답을 보낸다.
    - 응답 메세지(200 OK)
    - 헤더(헤더명: 헤더 값) - Body의 크기 등 ⇽ multiple
    - 빈 줄
    - Body 내용
- 연결이 끊어진다.

서버의 동작을 스레드로 작동하게 하여 한 번 응답을 보내도 프로그램이 종료되지 않게 한다: 반복문 사용

```java
public class VerySimpleWebServer {
  public static void main(Stringp[] args) throws Exception {
    ServerSocket ss = new ServerSocket(9090);

    System.out.println("클라이언트 접속을 기다립니다.");
    try{
        while(true) {
            // 브라우저(client)와 통신할 수 있는 객체
            Socket clientSocket = ss.accept();

            // client와 읽고 쓸 수 있는 InputStream, OutputStream을 만들 수 있다. 
            OutputStream out = clientSocket.getOutputStream();
            PrintWriter pw = new PrintWriter(new OutputStreamWriter(out)); // 전달
            InputStream in = clientSocket.getInputStream();
            BufferedReader br = new BufferedReader(new InputStreamReader(in));

            // 전달 받은 내용 출력

            String firstLine = br.readLine();
            List<String> headers = new ArrayList<>();

            String line = null;
            // 빈 줄을 만나면 while문을 끝낸다.
            while(!(line = br.readLine()).equals("")) {
                headers.add(line);
            }

            // GET /hello HTTP/1.1
            System.out.println(firstLine);
            for(int i = 0; i < headers.size(); i ++) {
                System.out.println(headers.get(i));
            }

            // http://localhost:9090/hello
            // http://localhost:9090/hi
            String msg = "";
            if(firstLine.indexOf("/hello") >= 0)
                msg = "hello";
            else if(firstLine.indexOf("/hi") >= 0)
                msg = "hi";

            // 전달할 내용 써서 전달
            // HTTP/1.1 200 OK <-- 상태 메세지
            // 헤더 1
            // 헤더 2
            // 빈 줄
            // 전달 내용
            pw.println("HTTP/1.1 200 OK");
            pw.println("name: park");
            pw.println("email: example@gmail.com");
            pw.println("");
            pw.println("<html>");
            pw.println("<h1>" + msg + " World !</h1>");
            pw.println("</html>");

            pw.flush(); // 클라이언트에 보내기
            br.close();
            pw.close();
            clientSocket.close();
        }
    } finally {
        ss.close();
    }
    System.out.println("서버가 종료됩니다.");
  }
}
```

이렇게 하면 한 번에 하나의 요청만 처리할 수 있는데? 스레드로 분리하자.

```java
public class VerySimpleWebServer {
  public static void main(Stringp[] args) throws Exception {
    ServerSocket ss = new ServerSocket(9090);

    System.out.println("클라이언트 접속을 기다립니다.");
    try{
        while(true) {
            // 브라우저(client)와 통신할 수 있는 객체
            Socket clientSocket = ss.accept();

            // 스레드가 생성, 대기
            ClientThread ct = new ClientThread(clientSocket);
            ct.start();
        }
    } finally {
        ss.close();
    }
    System.out.println("서버가 종료됩니다.");
  }
```

```java
class ClietThread extends Thread {
    private Socket clientSocket;

    public ClientThread(Socket clientSocket) {
        this.clientSocket = clientSockt;
    }

    @Override
    public void run(){
        try {
            // client와 읽고 쓸 수 있는 InputStream, OutputStream을 만들 수 있다. 
            OutputStream out = clientSocket.getOutputStream();
            PrintWriter pw = new PrintWriter(new OutputStreamWriter(out)); // 전달
            InputStream in = clientSocket.getInputStream();
            BufferedReader br = new BufferedReader(new InputStreamReader(in));

            // 전달 받은 내용 출력

            String firstLine = br.readLine();
            List<String> headers = new ArrayList<>();

            String line = null;
            // 빈 줄을 만나면 while문을 끝낸다.
            while(!(line = br.readLine()).equals("")) {
                headers.add(line);
            }

            // GET /hello HTTP/1.1
            System.out.println(firstLine);
            for(int i = 0; i < headers.size(); i ++) {
                System.out.println(headers.get(i));
            }

            // http://localhost:9090/hello
            // http://localhost:9090/hi
            String msg = "";
            if(firstLine.indexOf("/hello") >= 0)
                msg = "hello";
            else if(firstLine.indexOf("/hi") >= 0)
                msg = "hi";

            // 전달할 내용 써서 전달
            // HTTP/1.1 200 OK <-- 상태 메세지
            // 헤더 1
            // 헤더 2
            // 빈 줄
            // 전달 내용
            pw.println("HTTP/1.1 200 OK");
            pw.println("name: park");
            pw.println("email: example@gmail.com");
            pw.println("");
            pw.fluxh(); // ⇽ <1>

            pw.println("<html>"); // ⇽ <2>
            pw.println("<h1>" + msg + " World !</h1>");
            pw.println("</html>");

            pw.flush(); // 클라이언트에 보내기
            br.close();
            pw.close();
            clientSocket.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

<1> 실제 서버에서는 알맞은 데이터(예: 이미지)를 전달하기 위해 헤더에 데이터 유형과 크기를 같이  전달한다. 이 헤더는 flush()로 클라이언트에 전달한다(flush안하면 적절한 데이터 타입과 크기를 알 수 없으므로 얼마나 읽어야 하는이 클라이언트가 알 수 없다).

<2> binary 파일을 전달할 때에는 OutputStream을 통해서 byte stream 만큼 전달한다.

<추가> 서버는 /hello, 즉 특정 페이지를 요청 받았을 때, 어느 경로에 있는 파일을 읽어서 전달해야 하지?

→ Apache나 NginX같은 서버의 설정 파일이 어떻게 되어있는지 공부하면 도움이 될 것.

## 자바의 네트워크 프로그래밍 클래스

### 기본 클래스

- `Socket`
    - 클라이언트 측 소켓을 구현
    - 이 클래스 객체를 사용해 서버에 연결하고, 서버와 데이터를 주고받을 수 있다.
    - `getInputStream()`, `getOutputStream()`메서드를 제공하여 소켓을 통한 입출력 스트림을 얻을 수 있다.

-  `ServerSocket`
    - 서버 측 소켓을 구현
    - 이 클래스의 객체를 사용하면 클라이언트의 연결 요청을 기다리고, 요청이 오면 요청을 수락하고 통신용 소켓을 생성한다.
    - `accept()` 메서드를 제공하여 클라이언트의 연결 요청을 수락한다(블로킹 메서드).

### NIO(New Input/Output) 패키지 클래스

블로킹 및 넌 블로킹 I/O모델을 모두 지원하며(블로킹 모드에서는 I/O 작업이 완료될 때까지 스레드가 블로킹되며, 넌 블로킹 모드에서는 I/O작업이 즉시 반환된다.)
, 더 효율적인 네트워킹을 가능하게 한다.
더 복잡한 네트워킹 요구 사항을 처리할 수 있게 해주며, 대규모 네트워크 애플리케이션에서 더 높은 성능을 제공할 수 있다. 하지만 사용이 복잡하므로, 간단한 네트워킹 요구사항의 경우 `Socket`과 `ServerSocket` 클래스가 더 적합할 수 있다.

- `SocketChannel`
    - 클라이언트 측 소켓을 구현
    - TCP 연결을 통해 데이터를 읽고 쓸 수 있다.
- `ServerSocketChannel`
    - 서버 측의 소켓을 구현
    - 클라이언트의 연결 요청을 기다리고 수락
- `DatagramChannel`
    - UDP 연결을 통해 데이터를 읽고 쓸 수 있음.
- `Selector`
    - 여러 채널의 I/O 상태를 동시에 모니터링
    - 한 스레드가 여러 개의 네트워크 연결을 효율적으로 관리할 수 있게 한다.

## Note: 블로킹 메서드

- 특정 조건이 충족될 때까지 실행을 중지하고 대기하는 메서드. 블로킹 메서드는 I/O 작업이나 동기화 등에서 주로 사용되며, 작업이 완료될 때까지 현재 스레드를 대기 상태로 만든다.리소스를 효율적으로 활용할 수 있게 해 주지만, 잘못 사용하면 성능 저하를 일으킬 수 있다.
- 네트워크 프로그래밍에서 블로킹 소켓을 사용할 때, 클라이언트가 서버로 연결 요청을 하면, 서버가 연결을 수락하고 클라이언트와 연결된 소켓을 생성하는 Thread 클래스의 accept() 메서드가 그 예이다.
- SocketChannel의 open()메서드는 블로킹 방식으로 동작시키기 위해 configureBlocking(true)를 호출한다. 기본적으로 블로킹 방식으로 동작하지만, 넌블로킹과 구분하기 위해 명시적으로 설정을 두는 것이다.
