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





------------------
## 질문 답변

### 1. OS Injection에서 3rd party의 라이브러리를 사용하는것이 정확하게 어떤 의미인지 궁금합니다<br>

써드 파티는 프로그래밍을 도와주는 라이브러리 또는 플러그인을 만드는 외부 생산자를 뜻하며,
주로 편한 개발을 위해 플러그인이나 라이브러리 혹은 프레임워크를 사용하는데, 
이처럼 제 3자로 중간다리 역할로 도움을 주는 것이 써드 파티입니다..

(6. 'OS command를 실행시키지 않는다.'가 무슨 뜻인가요? 에대한 설명도 됨)

기본적으로 있는 OS command는 명령 실행 프로세스의 권한이 시스템 수준으로 높고,
명령을 받는 방법을 OS 커멘드로 둘 경우, 명령 인자로 외부 입력 사용시 사용자가 의도치 않은 명령을
사용할 수 있습니다.

반면 대표적인(많이 쓰이는) 써드 파티 라이브러리는 보안 전문가들에게 검토가 완료되어있고,
개발단에서 보안관련 처리가 되어있으며, 보안취약점이 발견될때마다 업데이트가 됩니다.
또한 row하게 입력되는 os커맨드와 달리, 사용자가 안전하게 외부 프로세스를 실행하고 다룰 수 있는 인터페이스를 제공합니다. 
이를 통해 보안 취약점을 최소화하고, 안전한 방식으로 외부 프로세스를 제어할수 있게 합니다.



### 2. OS Injection의 경우 시스템 명령어를 보내는 방식은 XSS나 CSRF/SSRF를 사용하는 것인가요?<br>
OS 인젝션과 그 두종류는 다른 종류의 보안 취약점으로, 시스템을 공격하는 벡터가 다릅니다.
XXS(Cross-Site Scripting)는 악성 스크립트를 삽입하여 사용자 브라우저에서 실행되게 합니다.
CSRF는 피싱 사이트, 이미지, 링크를 통해 사용자의 권한을 이용합니다.
SSRF는 애플리케이션 서버가 신뢰하는 타 서버에 대한 요청을 위조하는것으로, 서버측에서 발생합니다.

반면에 OS Injection은 주로 웹 애플리케이션의 외부 입력을 통해 시스템 명령을 실행하는 것을 말합니다.
일반적으로 사용자가 입력한 데이터를 신뢰하고 필터링하지 않은 경우에 발생합니다.
따라서 OS Injection은 서버 측에서 발생하며, 외부 입력을 통해 시스템 명령을 실행하거나 조작하는 것이 주요 특징입니다.


### 3.수업에서 배운 것 중에 prepareStatement로 파라미터를 추가하는 방식이 Query에 미리 형식을 지정함으로써 보안성을 향상시킨다고 하더라고요
질문은 아니지만 재미있는 지식 +1<br>
1) SQL Injection 방지: prepareStatement를 사용하면 SQL 쿼리에 파라미터를 추가할 때 이를 문자열로 직접 연결하지 않고, 파라미터화된 쿼리를 사용합니다. 이렇게 하면 사용자 입력이 직접 쿼리에 포함되는 것을 방지하여 SQL Injection 공격에 대한 보호를 제공합니다.
2) 자동 형식 변환: prepareStatement는 사용자가 입력한 값의 데이터 형식을 자동으로 변환하여 SQL 쿼리에 적용합니다. 이는 데이터 형식에 따른 오류를 방지하고, 잘못된 데이터 형식이 쿼리에 삽입되는 것을 방지합니다.
3) 파라미터 값의 안전한 처리: prepareStatement를 사용하면 사용자 입력 값이 문자열로 취급되지 않습니다. 대신, 파라미터 값은 값 자체의 형식에 따라 올바르게 처리됩니다. 이는 예기치 않은 결과를 방지하고 보안을 향상시킵니다.
4) 재사용 가능한 쿼리 계획: prepareStatement를 사용하면 데이터베이스 시스템이 쿼리를 컴파일하고 실행 계획을 준비하여 캐시에 저장합니다. 따라서 동일한 쿼리가 여러 번 실행될 때, 재사용 가능한 쿼리 계획을 사용하여 성능을 최적화할 수 있습니다.


### 4. 보안 취약점에서 'String interpolation'은 무엇인가요?<br>
'String interpolation'은 문자열 안에 변수를 삽입하는 기술을 가리킵니다. 이는 문자열 안에 변수를 포함하고, PHP이 해당 변수를 평가하여 실제 값으로 대체하는 것을 의미합니다.
위의 예시 기준으로는, Where문에 password에 "패스워드"라는 값을 넣어 쿼리문을 작성하는 부분이고,
injection을 위해 "패스워드 OR 1=1"를 넣어버림으로서 강제로 true를 반환하도록 유도했습니다.
(interpolation : 보간, 내삽 - 데이터 지점 고립점 내에 새로운 데이터 지점 구성 : 끝점에 추가)

### 5. Blind SQL Injection 공격을 막으려면 어떻게 해야 하나요?<br>
우선 블라인드 sql 인젝션은, 데이터베이스 에러 메세지나 데이터가 직접 노출되지 않는 최후의 경우에
sql문의 참/거짓 여부로 따지는 방법으로, 러프하게 말해서 로그인 페이지에 값을 넣어가면서 로그인 성공or실패를 알려주는 로그를 가지고
sql문을 한글자씩 바꿔가면서 추출해야하는 시간이 매우 오래걸리는 방법입니다.
그래서 SQLMap툴 등으로 공격 자동화가 필요한것입니다. 

대표적인 방어 방법들은 다음과 같습니다.<br>
1) 시간지연 - 응답 시간에 차이를 두어 공격자가 결과 확인에 많은 시간을 소비하게 만들어서 공격을 어렵게 하는 방법입니다.
2) 제한된 쿼리 수행 - 허용된 쿼리 수를 제한하여 쿼리반복으로 결과 확인을 방어합니다.
3) 로그 감시 - 웹 앱 로그를 모니터링하여 이상한 활동을 탐지하여 공격을 식별합니다.

+ SQLMap은 Blind SQL Injection 공격을 자동화하는데 사용되는 강력한 보안 도구 중 하나입니다. Blind SQL Injection은 웹 응용 프로그램의 취약성 중 하나로, 사용자의 입력이 데이터베이스 쿼리에 직접 삽입되어 발생합니다. 그러나 쿼리의 결과가 웹 페이지에 직접 노출되지 않으므로,
공격자는 결과를 확인하기 위해 추가적인 기술이 필요합니다.
SQLMap은 이러한 Blind SQL Injection 취약점을 자동적으로 탐지하고, 데이터베이스의 내용을 추출하는 데 사용됩니다. 이를 통해 공격자는 데이터베이스의 구조와 내용을 알아내고, 적절한 공격을 수행할 수 있습니다.
SQLMap은 다양한 Blind SQL Injection 공격 기술을 사용하여 취약점을 악용하며, 시스템에 대한 정보를 수집하고 데이터베이스를 조작하는데 사용될 수 있습니다. 또한, 명령 줄 인터페이스 및 GUI를 제공하여 사용자가 쉽게 사용할 수 있도록 지원합니다.


### 6. 'SQL Injection은 \n, \t, |, #, --, & 같은 문자열을 필터링하는 것으로 쉽게 방어가 가능하다.' 은 어떤 원리로 방어를 하는지 궁금합니다.<br>
바로 위에 공격용 입력 데이터를 보면 OR를 사용해서 쿼리가 강제로 true를 반환하도록 했는데,
이처럼 SQL에서 특별한 의미를 가지는 문자가 input됐을경우 자동으로 이스케이프 되도록 필터링함으로써 injection을 막는것입니다.
예를 들자면, 'mysqli_real_escape_string'이라는 함수를 사용하면 사용자 입력에서 특수문자가 발견될 경우 따옴표를 붙여(이스케이프하여) 문자열로 바꿔버림으로써 쿼리에 삽입되더라도 안전하게 바꿔줍니다.

### 7. Placeholder를 넣은 쿼리를 먼저 DB에 보낸 후 Placeholder에 해당하는 데이터를 DB로 보내는 방법 << 이 부분이 조금 이해가 어렵습니다.<br>
준비된 선언 (Prepared Statements) 이라는 방법으로, SQL 쿼리를 먼저 데이터베이스 서버로 보내고, 그 후에 파라미터를 채워서 실행하는 방법입니다.
데이터베이스 서버가 쿼리를 미리 컴파일하고, 나중에 파라미터를 채워서 실행하기 때문에 악의적인 쿼리가 삽입되는 것을 방지할 수 있습니다.

예시)
$stmt = $mysqli->prepare("SELECT * FROM users WHERE username=?");
$stmt->bind_param("s", $username);
$username = $user_input;
$stmt->execute();
위의 코드에서 prepare() 함수는 SQL 쿼리를 준비하고, bind_param() 함수는 쿼리의 ?에 변수를 바인딩합니다. 이렇게 함으로써 사용자 입력이 안전하게 처리되며, SQL Injection 공격을 방지할 수 있습니다.
보다 명확하게 말하자면, 준비된 선언에서는 쿼리와 파라미터가 분리되어 처리되기 때문에 사용자 입력이 SQL 쿼리에 직접 삽입되지 않습니다.
즉 사용자 입력은 쿼리에 직접 삽입되는 것이 아니라, 쿼리의 파라미터로 처리됩니다.





+ Injection 참고 - 실행 쿼리와 사진 링크 : https://gruuuuu.github.io/security/injection/
+ OS Injection 정의 관련 상세한 내용 : https://www.bugbountyclub.com/pentestgym/view/58
+ 실제 Injection을 연습해볼수 있는 프로그램 설명 : https://securitycode.tistory.com/95
