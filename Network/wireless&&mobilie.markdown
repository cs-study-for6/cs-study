# Wireless & Mobility
<ul>
    <li> Wireless VS Mobility</li>
        <ul>
            <li> Wireless란 연결이 무선인것을 의미한다
            <li>Mobility란 사용자가 움직이면 네트워크 Access Point가 바뀌는 것을 의미한다.
            <li> 유선과 무선의 MAC 프로토콜은 차이가 있음
            <li> 유선은 링크라는 매체를 공유해서 사용, 무선은 공기라는 매체를 공유해서 사용함
            <li> 무선일 경우 내 컴퓨터에서 google 서버로 통신이 갈때 전체가 무선이 아닌 한 홉만 무선임
            <li>  유선은 안정적인 케이블로 연결되어 있어 외부의 노이즈로부터 보호됨 그래서 연결 거리에 큰 영향을 받지 않음
            <li>무선은 거리에 큰 영향을 받음
        </ul>

</ul>

    중요한것은 무선이라고 반드시 이동성을 의미하는 것이 이니라는 것이다.    

  

-----
## Wireless
### wireless newtwork 구성요소
- base station
    - 주로 유선 네트워크에 연결
    - Relay:Host가 data를 전송하면 base station이 받아서 다음 네트워크로 전달해서 목적지 host까지 전달해주는 기능
- wireless hosts
    - 노트북, 스마트폰, IoT 등의 것
    - mobile 일 수도 있고, stationary 일 수도 있다.
- base station
    - 주로 유선 네트워크에 연결된다
    - Relay : Host가 data를 전송하면 base station이 받아서 다음 네트워크로 전달해서 목적지 host까지 전달해주는 기능
- wireless link
    - 주로 mobile을 base station으로 연결하기 위해 사용된다
    - 다양한 전송 속도와 거리, 주파수 대역을 이용한다

### wirless link의 특징
1. 신호의 세기가 감소한다
2. 다른 source로 부터 간섭을 받는다
3. multipath propagation 

         radio signal은 다른 물체에 부딫히면 꺽이므로 목적지에 서로다른 시간에 도착하게된다. 그리고 목적지에서 여러 방향으로 전파된 신호들이 충돌하기도한다.
        하지만 이러한 특징 덕분에 경로상에 장애물이 가로막혀 있어도 다른 경로를 통해 목적지에 도달한다.

4. Signal to Nosie Ratio

        잡음 대비 신호, 신호가 좋아 질수록 BER(bit Error rate)이 떨어진다. 즉 SNR이 클 수록 신호가 잡음으로부터 영향을 덜 받는다.

5. QAM

        무선은 보통 정현파로 전송되는데 이떄 위상을 변화 시켜서 전송하는 방법이다.
        신호가 셀 때만 하나의 신호에 여러 bit를 전송하여 보낼 수 있다,

6. Hidden Terminal Problem

        Hidden Terminal Problem
        B를 사이에 두고 A, C가 떨어져 있을 때
        A와 C는 서로의 전송신호를 감지하지 못한다.
    따라서 CSMA가 불가하여 A와 C가 동시에 데이터를 전송하면 B 입장에서는 데이터가 막 들어와 충돌이 발생한다. 이는 다음 3개지 방법으로 해결한다
    1. FDMA : 주파수를 나눠서 할당해준다.

    2. TDMA : 시간을 나눠서 할당해준다.

    3. CDMA : 코드를 다르게해서 전송한다.


             최근엔 OFDM 방식을 주로 사용한다.
            할당받은 주파수 대역을 서로 간섭을 일으키지 않는(Orthogonal) carrier로 나눠서 데이터를 실어서 한꺼번에 전송하는 방식

참고 자료 : https://www.nowwatersblog.com/cs/%EC%BB%B4%ED%93%A8%ED%84%B0%EB%A7%9D/7.%20Wireless%20and%20Mobile%20Network

참고자료2 : https://onduway.tistory.com/20

----

# Mobility

    network의 point attachment를 변경하는 움직이는 유저들을 처리하는것

- Handoff를 할 때의 문제
    - 호스트는 다시 IP Address를 받아야 한다.
    - 테이블 엔트리가 너무 커진다.
    - Not Scalable하다.
    - 결국 라우팅의 문제이다.
- 네트워크 엣지에서 라우팅을 지원해야 한다.
    - Home Network : 모바일 기기의 Home Network
    - Home Agent : 모바일기기가 Remote 할 때의 대리인
    - Foriegn Agent
    - Visited Nework : 현재 모바일 기기가 속해있는 네트워크
    - Correspondant : 모바일 기기로 Request를 보내는 기기
- 작동원리
    -  모바일 기기는 Permanant IP를 가지고 있고 Visited Network으로 들어가면 Care of Address를 할당받는다.
    -  Home Agent와 Foreign Agent는 서로 통신해서 모바일 기기의 위치를 파악하고
    - Correspondant로부터 Request가 들어오면 Home Agent는 그 모바일 기기인 척하고
    - Correspondant는 Home Agent와 Foreign Agent를 통해서 간접적으로 Mobile 기기와 통신한다.