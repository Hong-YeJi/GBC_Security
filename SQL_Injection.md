SQL Injection
=============
SQL Injection은 DB에 비정상적인 쿼리문이 실행되도록 하여 공격을 수행한다.   
공격에 이용되는 SQL 쿼리문은 문법적으로는 정상적인 SQL 구문이지만, 실행되지 말아야 할 쿼리문이 실행되어 공격에 이용된다.   

Low
---
![image](https://user-images.githubusercontent.com/61008728/126572360-7b50a59a-f43b-44a9-8d74-70104cc1a9d8.png)   
User ID에 1을 넣어보면 User ID가 1인 First name과 Surname을 출력한다.   

![image](https://user-images.githubusercontent.com/61008728/126572540-63313873-5042-4651-9c58-76171812d13c.png)   
User ID에 2를 넣어보면 User ID가 2인 First name과 Surname을 출력한다.   

<Boolean 식 구성>   
항상 참이 되도록 불린 식을 구성하여 쿼리문의 where 조건이 적용되지 않게 할 수 있다. 항상 참이 되도록 or로 연결을 하면 결과가 무조건 참이 되도록 하는 것이다.   

![image](https://user-images.githubusercontent.com/61008728/126589802-d5b74c39-dbb0-4aaf-82a2-15f2efb8af50.png)   
앞에서 User ID에 1을 넣으니 User ID가 1인 First name과 Surname이 나왔었다.   
이것을 이용하여 1' or '1'='1'#을 입력하면 User ID가 1인 것 또는 true를 반환하여 쿼리문 결과가 항상 참이 되기 때문에 테이블의 전체 내용이 출력된다.  
SQL 구문의 주석(#)을 의도적으로 삽입하여 Where 조건이 무시되게 할 수 있다.    
UserID 값에 주석을 삽입하면 주석 이하의 구문은 실행되지 않게 되고 조건이 항상 참이 되어서 테이블의 전채 내용을 출력할 수 있다.   

이렇게 출력된 정보를 보면 User ID가 총 5개가 있음을 알 수 있다.
![image](https://user-images.githubusercontent.com/61008728/126573618-96b7158f-0727-42ab-8e74-68600df9ae78.png)   
User ID에 5를 넣으면 전체 출력에 나온 것처럼 First name이 Bob, Surname이 Smith로 알맞게 출력되는 것을 확인할 수 있다.   

![image](https://user-images.githubusercontent.com/61008728/126573686-ed4ecd6f-0cff-45bc-86b5-bc39ef06c6ab.png)   
그리고 User ID가 5개가 출력되었으므로, 6을 넣어서 아무것도 출력되지 않음을 확인할 수 있었다.    

Medium
------
![image](https://user-images.githubusercontent.com/61008728/126576665-bcdab18d-a936-4067-87e4-d75ba7b9e4d5.png)

<Burp Suite의 Proxy 이용>      
![image](https://user-images.githubusercontent.com/61008728/126576830-939c27ef-3377-4b68-ba96-9086346c585c.png)    
'Intercept is off' 상태로 만든 다음, browser를 open한다.   
이 브라우저로 'http://localhost/dvwa-master/login.php'에 들어가서 로그인한 다음 sql injection문제를 medium으로 들어간다.   
![image](https://user-images.githubusercontent.com/61008728/126580186-58b220b3-ea78-4f09-9e94-5e54eb23eb9c.png)   
'Intercept is on' 상태로 한 다음, User ID를 1로 하여 input을 한다.   

![image](https://user-images.githubusercontent.com/61008728/126580219-52bfb6a2-e4e5-45a6-937f-dfb7980e26ea.png)  
이 창이 뜨게 되고, input으로 넘겨준 User ID가 1인 것을 맨 아래 줄에서 확인할 수 있다.   

![image](https://user-images.githubusercontent.com/61008728/126580398-07f5b030-9cc4-48e0-9091-ce2a17b22deb.png)   
이 id를 1 or 1=1이라는 불린식으로 id를 가로채서 테이블의 전체 내용이 출력된다.   

![image](https://user-images.githubusercontent.com/61008728/126580463-5499b685-2183-4df1-818a-d5616322293c.png)

High
----
![image](https://user-images.githubusercontent.com/61008728/126579745-69ca00ea-6021-4012-ac1b-6ab6ee4ffec7.png)   

![image](https://user-images.githubusercontent.com/61008728/126579679-5aceb411-78f2-4394-9839-f4e76d9dc287.png)   
1' or '1'='1'#을 입력하여 User ID가 1인 것 또는 true를 반환하여 쿼리문 결과가 항상 참이 되게 한다.    
SQL 구문의 주석(#)을 의도적으로 삽입하여 Where 조건이 무시되게 할 수 있다.    
UserID 값에 주석을 삽입하면 주석 이하의 구문은 실행되지 않게 되고 조건이 항상 참이 되어서 테이블의 전채 내용을 출력할 수 있다.   

![image](https://user-images.githubusercontent.com/61008728/126579688-78681352-8e2a-4844-b53c-3d4a1534b612.png)   
테이블의 전체 내용이 출력된 것을 확인할 수 있다.   
