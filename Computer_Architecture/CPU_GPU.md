# CS

## CPU & GPU

- CPU와 GPU 모두 데이터를 읽어 연산처리를 통해 답을 도출하는 기능을 수행한다.
컴퓨터에서의 역할은 비슷하지만 프로세서 내부의 구조를 살펴보면 CPU와 GPU는 차이가 크다

![Untitled](./CS%2098b04128a3e848d49b2cb8a8c2ca38d0/Untitled.png)

## CPU(Central processing unit, 중앙처리장치)

- CPU는 컴퓨터에서 기억, 해석, 연산, 제어라는 4대 주요 기능을 관할하는 장치를 말한다.
- 컴퓨터가 동작하는 데 필요한 모든 계산을 처리하며 컴퓨터를 뇌에 비유하자면 
단기 기억 담당은 RAM, 장기 기억은 SSD와 HDD이고 CPU는 사고를 담당하는 대뇌피질 
정도라 볼 수 있다.
- 명령어가 입력된 순서대로 데이터를 처리하는 **직렬(순차)처리 방식에 특화
>>** 즉 하나를 빠르게 처리하는데 특화

## GPU(Graphics Processing Unit)

- CPU와는 반대로 여러 명령어를 동시에 처리할 수 있는 **병렬 처리 방식
>>** 즉 여러 개를 동시에 처리하는데 특화
- 위와 같은 특징으로 인해 CPU로는 시간이 많이 걸리는 부동소수점 실수 연산과 
벡터연산을 가지는 멀티미디어, 특히 3차원 그래픽과 사운드를 잘 수행하는데 초점을 
맞춰 칩을 생산한다. CPU의 10배 이상의 성능을 낼 수 있다.
    - 게임 그래픽을 예로 들면 1920x1080 해상도에 60FPS를 구현하려면 1초에 1920x1080x60 = 124,416,000개의 픽셀을 그려야 한다. 1프레임을 기준으로 0.016초 안에 다 그려야 하며 3D그래픽으로 표현하기 위해서는  더 복잡한 절차를 거쳐야 하므로 이를 모두 수행하기 위해서는 GPU가 필수적이다.
    - GPU의 병렬처리에서의 우수성은 딥러닝에도 활용된다. 대용량의 벡터 데이터를 
    가지고 연산할 때 각 로우와 칼럼에 대한 합, 곱 연산을 병렬처리하면 CPU를 통한 
    처리보다 훨씬 빠르다.

## 예상 면접 질문

- CPU의 구성 및 그에 대한 설명
    - CPU는 크게 연산장치, 제어장치, 레지스터 3가지로 구성된다.
    
    1. 연산 장치
        
        > 산술연산과 논리연산 수행
        연산에 필요한 데이터를 레지스터에서 가져오고, 연산 결과를 다시 레지스터로 보낸다.
        > 
        
    2. 제어 장치
        
        > 명령어를 순서대로 실행할 수 있도록 제어하는 장치
        주기억장치에서 프로그램 명령어를 꺼내 해독하고, 그 결과에 따라 명령어 실행에 
        필요한 제어 신호를 기억장치, 연산장치, 입출력장치로 보냄
        또한 이들 장치가 보낸 신호를 받아, 다음에 수행할 동작을 결정함
        > 
        
    3. 레지스터
        
        > 고속 기억장치
        명령어 주소, 코드 연산에 필요한 데이터, 연산 결과 등을 임시로 저장
        용도에 따라 범용 레지스터와 특수목적 레지스터로 구분된다.
        중앙처리장치 종류에 따라 사용할 수 있는 레지스터 개수와 크기가 다름
        - 범용 레지스터 : 연산에 필요한 데이터나 연산 결과를 임시로 저장
        - 특수목적 레지스터 : 특별한 용도로 사용하는 레지스터
        > 
- CPU의 구조
    - 물리적인 구조
        - CPU를 통해 계산을 하기 위해 필요한 요소들은 다음과 같다.
            - 프로그램 카운터(Program Counter, PC) : 다음에 수행할 명령어 주소 저장
            - 명령어 레지스터(Instructions Register, IR) : 현재 실행 중인 명령어 저장
            - 데이터 레지스터(Data registers) : 연산에 필요한 피연산자 또는 연산의 결과 등을 
            저장하는 레지스터이다.
                - AC(누산기) : 연산 결과 임시 저장
            - 메모리 주소 레지스터(Memory Address Register, MAR) : 읽기와 쓰기 연산을 수행할 
            주기억장치 주소 저장
            - 메모리 버퍼 레지스터(Memory Buffer Register, MBR) : 주기억장치에서 읽어온 데이터 or 저장할 데이터 임시 저장
            
        - 마이크로 연산과 사이클
            
            ![Untitled](./CS%2098b04128a3e848d49b2cb8a8c2ca38d0/Untitled%201.png)
            
        
        > 명령어 사이클은 
        CPU가 기억장치로부터 명령어를 읽어오는 **명령어 인출**,
        실행하는 **명령어 실행,**
        해당 단계를 부 사이클로 구분하는 **인출 사이클**과 **실행 사이클**이 있다.
        > 
    - 소프트웨어 구조
        - 일반적으로 메인 메모리로부터 메모리 레지스터를 거쳐 프로그램 카운터, 기억장치들에 도달하며, 명령어의 비트 수와 용도 및 구성 방식을 지정하는 형식인 “명령어 형식”으로 지정된다.
        - 기계 명령어의 형식은 일반적으로 **연산 코드 - 피연산자**로 구성되어 있다.
        - CPU가 한 번에 처리할 수 있는 워드 형식을 통해 이를 정의하며, 지정 가능한 워드 수가 많은수록 연산의 종류가 다양화된다.
        
        1. 연산 코드(opcode) : 연산의 종류를 지시하는 비트이다. 이 연산 코드가 차지하는 비트가 많을수록 지정할 수 있는 명령어의 수가 늘어난다.
        2. 피연산자(operand) : 주소, 숫자/문자, 논리 등의 연산에 사용될 데이터가 저장되어 있는 기억장치 주소 또는 데이터 그 자체를 지시하는 비트이다.
- CPU의 동작 과정 및 원리
    - CPU의 동작
        1. 주기억장치는 입력장치에서 입력받은 데이터 또는 보조기억장치에 저장된 프로그램을 읽어옴
        2. CPU는 프로그램을 실행하기 위해 주기억장치에 저장된 프로그램 명령어와 데이터를 
        읽어와 처리하고 결과를 다시 주기억장치에 저장
        3. 주기억장치는 처리 결과를 보조기억장치에 저장하거나 출력장치로 보냄
        4. 제어장치는 1~3 과정에서 명령어가 순서대로 실행되도록 각 장치를 제어
- CPU와 GPU의 장단점 및 차이점과 활용 분야