Command Injection
=================
웹 애플리케이션에서 시스템 명령을 사용할 때, ;, |, ||, && 등을 사용해서 commamd를 injection하여 두 개의 명령어가 실행되게 하는 공격이다.     
웹을 통해 시스템 명령어를 실행하게 되며, 특정 명령어 실행에 성공하면 파일정보 유출 부터 시스템 장악까지도 가능해진다.       
Command Injection이 발생할 경우, 현재 Command를 실행시키는 웹 어플리케이션의 권한만큼 해당 서버에서 권한을 획득할 수 있다.         

Low
---
![image](https://user-images.githubusercontent.com/61008728/126728412-358263fe-f71d-4737-b060-4b0eb70e135e.png)     
|| 연산자를 이용하여 '|| cat /etc/passwd' 명령어를 입력하면, /etc/passwd 파일의 정보들을 볼 수 있다.

Medium
------
![image](https://user-images.githubusercontent.com/61008728/126728459-71b6ac9a-6c4a-4ac1-97ed-46f9265dc321.png)      
medium 문제도 마찬가지 '|| cat /etc/passwd' 명령어를 입력하면, /etc/passwd 파일의 정보들을 볼 수 있다.   

High
----
![image](https://user-images.githubusercontent.com/61008728/126728561-45089a0a-dfc4-4687-8510-9a151cee0969.png)
or
![image](https://user-images.githubusercontent.com/61008728/126728587-d01162a8-e1bf-4ab4-b2f1-8a59261ce57d.png)
high 문제도 마찬가지 '|| cat /etc/passwd' 명령어를 입력하면, /etc/passwd 파일의 정보들을 볼 수 있었는데,      
소스코드를 보면 이상한 점이 있다. '||' 연산자를 필터링하고 있다는 코드가 있는데, 공격이 된다는 점이다.   

![image](https://user-images.githubusercontent.com/61008728/126749195-12dc2dc0-d4fd-4bc6-898a-43bddadeae1b.png)
'||'를 사용하지 않고 명령어를 입력하려면, '||' 사이에 '(' 문자를 넣어 '|(|'를 사용할 수 있다.    
'|(| cat /etc/passwd' 명령어를 입력해도 /etc/passwd 파일의 정보들을 볼 수 있다.    
'|'와 '|' 사이에 문자가 있어도 무시되기 때문에, 코드에서 '||'가 필터링 되어도 명령어가 Injection 될 것이다.   

