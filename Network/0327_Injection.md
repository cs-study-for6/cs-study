# SQL Injection 취약점

## 설명
서버에서 실행되는 SQL을 악의적으로 이용하는 공격이다. 이름처럼 기존 SQL에 악의적인 SQL 구문을 삽입하여 데이터 탈취, 삭제 등을 할 수 있다.
데이터베이스와 연동된 웹 애플리케이션에서 공격자가 입력 폼 또는 URL 입력란에 SQL 구문을 삽입하여 DB를 조작한다. 이는 입력란에 입력된 데이터의 유효성을 검증하지 않아 발생되는 보안 취약점이다.
쉬운 공격 난이도에 비해 공격이 성공할 경우 상당히 큰 피해를 낼 수 있는 취약점이다.
<br>
## 종류
공격에 사용되는 방식에 따라 아래와 같이 분류된다.

• Error based SQL Injection<br>
• Union SQL Injection<br>
• Blind SQL Injection<br>
• Stored Procedure SQL Injection<br>
• Time based SQL Injection<br>

<br>

## SQL Injection 취약점 대응 방안 <br>

• 입력 값에 대한 검증 로직을 구현하여 사전에 정의된 특수 문자들이 입력되지 않도록 조치한다.<br>
• Prepared Statement 구문을 사용하여 개발을 함으로써 입력 값에 들어가는 데이터는 단순히 문자열로 처리되도록 한다.<br>
• 불필요한 데이터베이스 에러 메시지가 사용자에게 노출이 되지 않도록 한다.<br>
• 웹 방화벽 (WAF, Web Application Firewall)를 사용하여 비정상적인 데이터가 전송이 될 경우 차단하도록 한다.<br>

• 서버 보안을 활성화한다.<br>
(최소 권한 유저로 DB 운영, 사용하지 않는 저장 프로시저와 내장함수 제거/권한 제어,
목적에 따라 Query 권한 수정, 공용 시스템 객체의 접근 제어, 신뢰할 수 있는 네트워크, 서버에 대해서만 접근 허용
에러 메시지 노출 차단 등)



<br>
<br>

## 예시
POST 요청을 통해 파라메터로 전달 받은 username과 password를 각각 변수에 대입한 후 MySQL 드라이버를 통해 쿼리를 보내는 로직의 PHP코드<br>

PHP
```PHP
    $username = $_POST['username'];
    $password = $_POST['password'];
    $mysqli->query("SELECT * FROM users WHERE username='{$username}' AND password='{$password}'")
```
보안 취약점 : 쿼리에서 String interpolation하는 부분 <br>


공격용 입력 데이터
```PHP
    $username = $_POST['username']; // admin
    $password = $_POST['password']; // password' OR 1=1 --

    $mysqli->query("SELECT * FROM users WHERE username='{$username}' AND password='{$password}'")
    // SELECT * FROM users WHERE username='admin' AND password='password' OR 1=1 --'
```
<br>
결과 : username과 password가 각각 문자열 admin과 password' OR 1=1 --로 들어왔다면 아래 쿼리는 SELECT * FROM users WHERE username='admin' AND password='password' OR 1=1 --'로 구성된다. OR 1=1이 추가되었기 때문에 패스워드가 틀리더라도 해당 쿼리는 True를 반환한다. 
방어가 되어있지 않다면 쉽게 타인의 계정으로 로그인 할 수 있게된다.

<br>

방어 방법 : 웹 보안에서 가장 중요한 부분이 문제가 될 수 있는 문자열을 필터링한다.
- SQL Injection은 \n, \t, |, #, --, & 같은 문자열을 필터링하는 것으로 쉽게 방어가 가능하다.
<br>
<br>

1) SQL에서 특별한 의미를 가지는 문자, (\n, \t, |, /, &, #) 을 이스케이프한다.<br>
2) 준비된 선언을 사용한다.
	 (Placeholder를 넣은 쿼리를 먼저 DB에 보낸 후 Placeholder에 해당하는 데이터를 DB로 보내는 방법이다.)

<br>

## 종류별 injection

- Error based SQL Injection<br>
GET,POSt 요청필드, HTTP헤더값, 쿠키값 등에 특수문자(싱글 쿼드 혹은 세미코론) 삽입 시 SQL관련 에러를 통해 데이터베이스 정보를 예상한다.
고의적으로 SQL 에러를 발생시켜서 원하는 정보를 취득하는 공격 기법으로,
에러가 발생하면 DB에 대한 정보를 단편적으로 얻을 수 있게 되므로 에러 메시지가 노출되지 않도록 주의해야한다.

<br>


- Union SQL Injection<br>
Union 명령을 이용해서 정보를 취득할 수 있다.
Union은 2개 이상의 쿼리를 요청하여 결과를 얻는 SQL연산자인데, 공격자가 이를 악용하여 원래의 요청에 추가 쿼리를 삽입하여 정보를 얻어내는 방식이다.
단 유니온 쿼리는 2개의 테이블이 동일한 필드개수와 데이터 타입을 가져야 하기에 사전공격을 통해 해당 정보를 얻는 과정이 필요하다
```
SELECT * FROM users WHERE user_id = '1' or 1=1 UNION SELECT '',id,pw from users#
```
<br>

- Blind SQL Injection<br>
에러가 발생하지 않는 사이트에서는 위 두가지 기법을 사용하기 어렵기에, 공격을 통해 정상적인 쿼리 여부를 가지고 취약점 여부를 판단하는 기법이다.
즉 쿼리 결과의 참/거짓 정보를 보고 원하는 정보가 존재하는지 추론하는 것으로, 이를 통해 데이터베이스, 테이블, 컬럼 명을 파악할 수 있다.
```
SELECT * FROM users WHERE user_id = '1' and substring(database(),1,2)='us'#
```
SQLMap과 같은 툴을 사용하면 Blind SQL Injection 공격 자동화가 가능하다.

<br>


- Stored Procedure SQL Injection<br>
저장 프로시저(Stored Procedure)는 쿼리들을 모아 하나의 함수처럼 사용하기 위한 것이다.
이때 명령어 실행이 가능한 MSSQL의 xp_cmdshell을 악용하여 운영체제 명령어를 사입하는 기법으로,
웹에서 저장 프로시저에 대한 접근 권한을 가질 경우 프로시저에 injection을 통해 공격한다.
공격 난도가 높으나 성공 시 직접적인 피해를 입힐 수 있다.

- Time based SQL Injection<br>
쿼리 결과를 특정 시간만큼 지연시키는 방법으로 수행하며, Blind기법과 마찬가지로 에러가 발생하지 않는 조건에서 사용할 수 있다.


종류별 공격 예시
    https://velog.io/@33bini/DB-SQL-Injection



# OS Injection

## 설명 
시스템의 명령어를 쿼리문에 주입하여 서버 운영체제에 접근하는 공격 즉, 웹페이지에 시스템 명령어를 주입하여 쉘을 획득하는 공격이다.

## 발생가능 문제
- 전체 시스템 탈취
- 서버내 중요정보 유출(비밀번호, 암호키 등)

## 방어 방법
- OS command를 실행시키지 않는다.
  	보안적으로 취약하기에 사용을 권장하지 않으며, 서버단의 작업이 필요하다면 os command 대신 3rd party의 라이브러리를 사용하는것을 추천한다.
- 최소한의 권한으로 실행한다.
  	사용해야만 한다면, 실행 권한을 root가 아닌 제한된 권한을 가진 (tomcat등의) user로 실행하게 하는것
- 시스템 커멘드 실행시 안전한 function사용하기
  	웹에서 단순 명령어로 실행시, 파이프로 코드를 추가 주입하면 exec명령어는 여러개의 파라미터로 인식하고 파이프로 이어진 코드들을 바로 실행시켜 문제가 된다, 하지만 String Array로 파라미터를 감싸면 두개의 배열로 인식하여 추가주입된 코드를 실행시키지 않게된다.
  (하단 링크의 상세 설명 참조)

## 예시
예시 ) DNS lookup으로 입력된 주소의 dns를 확인하는 웹 페이지
![image](https://github.com/ELS-7/CS-Ready/assets/104435415/e4d18324-33c2-48a5-b421-3660b9dc1843)
해당 페이지는 php로 짜져있고, dns lookup을 하기위해 파라미터를 commandi로 받아 shell_exec로 실행시키고 있다.
![image](https://github.com/ELS-7/CS-Ready/assets/104435415/6989b36e-8ae6-40cc-8b4d-41d9c1a84758)
이 페이지는 php로 구성되어있고, 작동을 위해 파라미터를 commandi로 받는다.
이때 실행시킬 파라미터를 그대로 받아오기에, 주소옆에 파이프(‘|‘)를 붙이고 파일의 리스트를 출력하는 ls command를 입력하면 정상적으로 dns리스트가 출력되는 것이 아니라 웹서버 내 파일들이 출력된다.

![image](https://github.com/ELS-7/CS-Ready/assets/104435415/8b0ecee1-e2b2-4663-b979-7ebe56e87ad8)
이처럼 명령어를 주입함으로서 웹  서버내 파일을 파악하거나 password등을 탈취해 서버 계정을 탈취, 웹서버로의 원격 접속 시도등이 가능해진다.

+ Injection 참고 - 실행 쿼리와 사진 링크 : https://gruuuuu.github.io/security/injection/
+ OS Injection 정의 관련 상세한 내용 : https://www.bugbountyclub.com/pentestgym/view/58
+ 실제 Injection을 연습해볼수 있는 프로그램 설명 : https://securitycode.tistory.com/95
