# NGiNX

## INDEX

- [nginx 기본 정보](#nginx-기본-정보)  
- [apache vs. nginx](#apache-vs-nginx)  
- [nginx 관련 명령어](#nginx-관련-명령어)  
- ⭐[nginx 설정](#nginx-설정)  
- ⭐[Reverse Proxy](#reverse-proxy)  
- ⭐[Load Balancer](#load-balancer)

---

## nginx 기본 정보

- 웹 서버 소프트웨어
- 기능
    - 웹 서버
    - reverse proxy 서버
    - HTTP 캐시 서버
    - mail proxy 서버
    - TCP/UDP 서버
- 주로 전달자(proxy) 역할을 함.  
  가볍고, 동시 접속에 특화.

![nginx 흐름](https://user-images.githubusercontent.com/60170616/149441113-24698a28-691c-48b5-87b3-97c792d29ad3.png)

- `event-driven`
- `asynchronous`
- `single-thread`
- `non-blocking`

---

## apache vs. nginx

- `apache`
    - **멀티스레드 환경**  
      하나의 스레드에서 하나의 커넥션을 연결하여 그에 대한 요청만 해결
    - 요청 하나하나를 하나의 스레드에서 처리하기 때문에 동기화 되어있고, 각 요청이 blocking 되어있다.
    - 스레드가 많이 뜨는만큼 무겁고, 효율이 떨어진다.  
      메모리 소모가 크다.
    - 하지만 독립적이기에 안정적이다.  
      에러가 뜨면 하나의 커넥션에 대한 에러이기 때문.
- `nginx`
    - **싱글 스레드 환경**  
      하나의 스레드에서 여러 커넥션을 연결하고 여러 요청을 동시에 처리.
    - 비동기식.  
      각 요청이 non-blocking 방식.
    - non-blocking 방식으로 효율적이다.  
      ex) 10초 걸리는 요청 이후에 1초 걸리는 요청이 들어오면 10초를 기다리지 않아도 됨.
    - apache에 비해 많이 가볍고 효율적이다.  
      하지만 어렵고 에러가 뜨면 전체 스레드에 영향이 갈 수 있다.

---

## nginx 관련 명령어

- nginx -v
    - 버전 확인
- nginx -t
    - 설정파일 문법 검사
- nginx -T
    - 설정파일 문법 검사 및 설정파일 내용 표시
- nginx reload / restart
    - nginx 설정 리로드(worker 프로세스만), nginx 재시작
- nginx quit / stop
    - 하던일을 마치고 stop / 바로 stop

---

## ⭐nginx 설정

- 설정 파일
    - 주 설정 파일
        - /etc/nginx/nginx.conf
    - 포함될 설정 파일
        - /etc/nginx/conf.d/*.conf
- `Directive`
    - 세미콜론으로 끝나는 한 줄짜리 지시어
- `Block`
    - 서로 관련있는 directive들을 중괄호로 묶은 block
- `Context`
    - 최상위 block.  
    서로 다른 트래픽 유형에 적용되는 지시어를 묶는다.
    - 종류
        - events
        - http
        - mail
        - stream
        - main : 어디에도 속하지 않는 directive 들은 main 컨텍스트에 있다고 표현.
    ```bash
    http {
        server {
            listen       127.0.0.1:80;
            server_name  example.com;
            root         /usr/share/nginx/html;
        }

        server {
            listen       127.0.0.2:80;
            server_name  example2.com;
            root         /usr/share/nginx/html2;
        }
    }
    ```
- server_name 선택 순서
1. 정확히 일치하는 이름
2. \*로 시작하는 가장 긴 와일드카드 이름  ex) *.example.com
3. \*로 끝나는 가장 긴 와일드카드 이름  ex) mail.*
4. 설정 파일에 나열되어있는 순서 상으로 가장 먼저 부합되는 정규식
- `default_server`
    - 해당되는 server가 없을때 default_server로 연결
    - default_server 설정이 없을 경우 맨 위 server로 연결
    - 주로 잘못된 요청에 대한 응답을 default로 설정
    ex) deny all, return 404 ...
    ```bash
    server {
    	listen  192.168.1.1 default_server;
    }
    ```
- `location` block
    - 특정 URI를 처리하는 방법 정의
    - 접두어(prefix)
        ```bash
        server {
            listen    192.168.1.1;
            location  /app1 {...}
            location  /app2 {...}
        }
        ```
    - 정규식(regex)
        ```bash
        ##  ~* 대소문자 구분 X
        ##  ~  대소문자 구분 O
        ##  ^~ 정규식 매칭 X
        ##  =  정확한 매칭. 이후 검색 중단.
        
        server {
            listen    192.168.1.2;
            location  ~* ^/app(\d+)$ {
            }
        }
        ```
    - 일치하는 location block 찾는 순서
        1. 순서에 상관없이 설정 파일의 선택된 서버 안의 모든 접두어 타입의 위치를 검사한다.
        2. 가장 긴 접두어 매칭 결과를 선택하고 우선 기억한다.  
          만일 modifier가 = 또는 ^~ 라면 정규식 타입 검사 X
        3. 설정 파일에 나열된 순서대로 정규식을 검사하고 첫번째로 매칭되는 식을 사용한다.
        4. 매칭하는 정규식을 찾지 못했다면, 앞서 찾은 가장 긴 접두어 매칭 결과를 선택한다.
    - 접두어 타입 먼저 나열 → 정규식 타입 나열하는 것이 best practice

---

## ⭐Reverse Proxy

![Reverse Proxy](https://user-images.githubusercontent.com/60170616/149442298-d263c79e-0b30-49bb-8fdf-1059e9cbe283.png)

- 서버를 감춰주는 역할
- 이점
    - `보안 강화`
        - WAS, DB 서버를 내부 네트워크망에 안전하게 보호
        - SSL에 대한 암복호화 처리 및 SSL 하드웨어 가속기 사용
        - 접근 IP 관리, 접근 limit 처리
    - `시스템 효율 향상`
        - 정적 컨텐츠는 웹서버에서, 동적 컨텐츠는 WAS에서 제공
        - KeepAlive
        - 컨텐츠 캐시 기능
    - `로드 밸런싱`
        - 동일 기능의 WAS 여러대 연결 가능
        - 다양한 WAS 제공
        - WAS의 장애에 손쉽게 대응 가능, 무중단 서비스 제공
- `proxy_pass` : 치환
    ```bash
    # 요청:
    # http://www.example.com/app1/usr/index.html

    server {
        listen  80 default_server;
        server_name  www.example.com;
        root  /usr/share/nginx/html;
    
        location  /app1 {
            proxy_pass  http://192.168.1.1:8080/app;
        }
    }

    # 전달된 요청:
    # http://192.168.1.1:8080/app/usr/index.html
    
    # ---

    # 요청:
    # http://www.example.com/app1/usr/index.html

    server {
        listen  80 default_server;
        server_name  www.example.com;
        root  /usr/share/nginx/html;
    
        location  /app1 {
            proxy_pass  http://192.168.1.1:8080; # path가 없는 경우엔 그대로 추가
        }
    }

    # 전달된 요청:
    # http://192.168.1.1:8080/app1/usr/index.html
    ```
- 요청 헤더 재정의
    - 리버스 프록시 서버를 통하기 때문에 전부 내부 ip로 표시된다.  
    실제 ip를 찍기 위해 요청 헤더 재정의
    ```bash
    server {
        listen  80;
        proxy_set_header  Host $host;
        proxy_set_header  X-Real-IP $remote_addr;
        proxy_set_header  X-Forwarded-For $proxy_add_x_forwared_for;
    
        location  /app {
            proxy_pass  http://test.com;
        }
    }
    ```

---

## ⭐Load Balancer

- 로드 밸런싱 방법
    - round robin
    - least_conn
    - hash
    - ip_hash
    - random
```bash
upstream myServers { # 메인 서버
    server  app1.example.com;
    server  app2.example.com:8080;
}

server {
    listen  80 default_server;
    location / {
        proxy_pass  http://myServers; # upstream 서버풀로 요청 전달
    }
}
```