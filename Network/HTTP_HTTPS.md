## HTTP & HTTPS

- **HTTP(HyperText Transfer Protocol)**
    - HyperText - 하이퍼텍스트, Transfer - 전송, Protocol - 통신 규약
    - 인터넷 상에서 클라이언트와 서버가 자원을 주고 받을 때 쓰는 통신 규약
    예를 들어, 인터넷 주소를 지정할 때, `http://www.naver.com`와 같이 시작하는 것은 `www.naver.com`이라는 인터넷 주소가 가진 데이터 정보 등의 교환을 HTTP의 통신 규약대로 처리하라는 것을 의미한다.
    - HTTP는 암호화가 되지 않은 평문 데이터를 전송하는 프로토콜이였기 때문에, HTTP로 
    비밀번호, 주민등록번호 등 개인 정보를 주고받으면 제3자가 정보를 열람할 수 있었다.
    이러한 보안 문제를 해결하기 위해 등장한 프로토콜이 **‘HTTPS’(HyperText Transfer Protocol Secure)**이다.
    
    - **HTTP의 보안 취약점**
        1. `도청이 가능하다.`
            1. 평문으로 통신하기 때문에 도청이 가능하다.
                1. 이를 해결하기 위해 통신 자체를 암호화(HTTPS)하거나 데이터를 암호화 
                하는 방법 등이 있다.
            2. 데이터를 암호화 하는 경우 수신측에서는 복호화 과정이 필요하다.
        2. `위장이 가능하다.`
            1. 통신 상대를 확인하지 않기 때문에 위장한 상대와 통신할 수도 있다.
            2. HTTPS는 CA인증서를 통해 인증된 상대와 통신이 가능하다.
        3. `변조가 가능하다.`
            1. 완전성을 보장하지 않기 때문에 변조가 가능하다.
            2. HTTPS는 메세지 인증 코드(MAC), 전자 서명 등을 통해 변조를 방지한다.
    - 장점
        - 통신 간의 연결 상태 처리나 상태 정보를 관리할 필요가 없어 서버 디자인이 
        간단하다.
        - 각각의 HTTP 요청에 독립적으로 응답만 보내주면 된다.
        - HTTPS에 비해 속도가 빠르다.
    - 단점
        - 이전 통신의 정보를 모르기 때문에 매번 인증을 해줘야 한다.
        - 보안이 취약하다.

- **HTTPS(HyperText Transfer Protocol Secure)**
    - 인터넷 상에서 정보를 암호화하는 SSL 프로토콜을 사용해 클라이언트와 서버가 자원을 주고 받을 때 쓰는 통신 규약
    - HTTPS를 사용하면 서버와 클라이언트 사이의 모든 통신 내용이 암호화된다.
    - HTTPS는 대칭키 암호화 방식과 비대칭키 암호화 방식을 모두 사용하고 있다.
        - `대칭키 암호화`
            - 클라이언트와 서버가 동일한 키를 사용해 암호화/복호화를 진행
            - 키가 노출되면 매우 위험하지만 연산 속도가 빠름
        - `비대칭키 암호화`
            - 공개키/개인키 암호화 방식을 이용해 데이터를 암호화
            - 공개키와 개인키는 서로를 위한 1쌍의 키
            - 1개의 쌍으로 구성된 공개키와 개인키를 암호화/복호화하는데 사용함
            - 키가 노출되어도 비교적 안전하지만 연산 속도가 느림
    
    - **HTTPS 통신 흐름**
        1. 애플리케이션 서버(A)를 만드는 기업은 HTTPS를 적용하기 위해 공개키와 개인키를 만든다.
        2. 신뢰할 수 있는 CA 기업을 선택하고, 그 기업에게 내 공개키 관리를 부탁하며 계약을 한다.
            
            <aside>
            💡 CA 기업 : Certificate Authority, 공개키를 저장해주는 신뢰성이 검증된 
            민간기업
            
            </aside>
            
        3. 계약 완료된 CA 기업은 해당 기업의 이름, A서버 공개키, 공개키 암호화 방법을 담은 인증서를 만들고, 해당 인증서를 CA 기업의 개인키로 암호화해서 A서버에게 제공한다.
        4. A서버는 암호화된 인증서를 갖게 되었다. 이제 A서버는 A서버의 공개키로 암호화된 HTTPS 요청이 아닌 다른 요청이 오면, 이 암호화된 인증서를 클라이언트에게 건내준다.
        5. 클라이언트가 `main.html` 파일을 달라고  A서버에 요청했다고 가정하자. HTTPS 요청이 아니기 때문에 CA기업이 A서버의 정보를 CA 기업의 개인키로 암호화한 인증서를 받게 된다.
        6. 브라우저는 해독한 뒤 A서버의 공개키를 얻는다.
        7. 클라이언트가 A서버와 HandShaking 과정에서 주고받은 난수를 조합하여 pre-master-secret-key를 생성한 뒤, A서버의 공개키로 해당 대칭키를 암호화하여 서버로 보낸다.
        8. A서버는 암호화된 대칭키를 자신의 개인키로 복호화하여 클라이언트와 동일한 대칭키를 획득한다.
        9. 클라이언트, 서버는 각각 pre-master-secret-key를 master-secret-key로 만든다.
        10. master-secret-key를 통해 session-key를 생성하고 이를 이용하여 대칭키 방식으로 통신한다.
        11. 각 통신이 종료될 때마다 session-key를 파기한다.
        
        ![Untitled](./CS%20fd38dc95fd994134b26b2d76972ab569/Untitled%203.png)
        
        ![Untitled](./CS%20fd38dc95fd994134b26b2d76972ab569/Untitled%204.png)
        
        ![그림 출처 : [https://eun-jeong.tistory.com/27](https://eun-jeong.tistory.com/27)](./CS%20fd38dc95fd994134b26b2d76972ab569/Untitled%205.png)
        
        그림 출처 : [https://eun-jeong.tistory.com/27](https://eun-jeong.tistory.com/27)
        
    - 장점
        - 네트워크 상에서 열람, 수정이 불가능하므로 비교적 안전하다.
        - 키워드 검색 시 상위 노출되는 기준 중 하나가 보안 요소이다.
    - 단점
        - 설치 및 인증서를 유지하는 데 추가 비용이 발생한다.
        - 암호화하는 과정이 웹 서버에 부하를 줄 수 있다.
        - HTTP에 비해 속도가 느리다.
        - 인터넷의 연결이 끊긴 경우 재인증 시간이 소요된다.
            - HPPT는 비연결형으로 웹페이지를 보는 중에 인터넷의 연결이 끊겼다가 다시 연결되어도 페이지를 계속 볼 수 있다.
            - HTTP는 소켓(데이터를 주고 받는 경로) 자체에서 인증을 하기 때문에 인터넷의 연결이 끊기면 소켓도 끊어져서 다시 HTTPS 재인증이 필요하다.

![그림 출처 : [https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/](https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/)](./CS%20fd38dc95fd994134b26b2d76972ab569/Untitled%206.png)

그림 출처 : [https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/](https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/)

- 예상 질문
    - HTTP or HTTPS 프로토콜에 대해 설명해주세요.
    - HTTP와 HTTPS의 장단점에 대해 설명해주세요.
    - HTTP와 HTTPS의 차이점은 무엇인가요?

- **공개키/개인키**
    - `공개키 암호화` : 공개키로 암호화를 하면 개인키로만 복호화할 수 있다. 개인키는 나만 가지고 있으므로 나만 볼 수 있다.
    - `개인키 암호화` : 개인키로 암호화하면 공개키로만 복호화할 수 있다. 공개키는 모두에게 공개되어 있으므로 내가 인증한 정보임을 알려 신뢰성을 보장할 수 있다.
    
    ![Untitled](./CS%20fd38dc95fd994134b26b2d76972ab569/Untitled%207.png)
    

참고

- [https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer Science/Network/HTTP %26 HTTPS.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/HTTP%20%26%20HTTPS.md)
- [https://mangkyu.tistory.com/98](https://mangkyu.tistory.com/98)
- [https://velog.io/@inyong_pang/HTTP-HTTPS](https://velog.io/@inyong_pang/HTTP-HTTPS)

참고하면 좋은 사이트

- [https://inpa.tistory.com/category/개발 지식/HTTP 지식?page=2](https://inpa.tistory.com/category/%EA%B0%9C%EB%B0%9C%20%EC%A7%80%EC%8B%9D/HTTP%20%EC%A7%80%EC%8B%9D?page=2)