XSS (Stored)
============
Stored XSS는 Reflected XSS와 달리 웹 사이트의 게시판에 스크립트를 삽입하는 공격 방식   

Low
---
![image](https://user-images.githubusercontent.com/61008728/126567964-73459a7a-3953-4fd3-a005-8566bd9b06b1.png)
Message에 <script>alert("XSS_stored Attack");과 같이 스크립트를 삽입하여 공격을 할 수 있다.

![image](https://user-images.githubusercontent.com/61008728/126568009-c61a1932-e3a7-43da-996e-5dac13c4da15.png)   
공격을 성공하면 alert 함수 안에 작성했던 메시지가 팝업창으로 뜨게 된다.

![image](https://user-images.githubusercontent.com/61008728/126568041-b8206293-e36d-4873-8de7-bcb6823ea3e0.png)

Medium
------
![image](https://user-images.githubusercontent.com/61008728/126569074-38e72271-b8c8-49b0-b599-13fb3ff79da9.png)
![image](https://user-images.githubusercontent.com/61008728/126569109-8c37057f-a08e-44a1-bae3-9a5dc1cfce47.png)   
공격이 실패하고 메시지에 스크립트 문이 문자열로 그대로 들어간 것을 확인할 수 있다. 
코드 안에 스크립트 태그를 ""로 바꾸는 함수가 있을 것이다.
따라서, img 태그를 사용하여 공격을 시도한다.
  
![image](https://user-images.githubusercontent.com/61008728/126569515-4510c7fc-99f6-4ea1-ac4d-1ad0e60c0a6d.png)   
<img src="#" onerror="alert("XSS Medium Attack")">과 같이 입력하여 공격을 시도한다.

![image](https://user-images.githubusercontent.com/61008728/126570287-eca53a97-2e9c-42e2-a02f-23befb5db996.png)   
스크립트 태그를 입력했을 때는 스크립트문이 적용이 되지 않아 Message에 입력한 내용이 그대로 나왔는데,   
이번에는 img 태그가 적용이 되어 Message에 아무것도 없음을 확인할 수 있다.   
(img 태그를 사용해서 그런지 새로운 팝업창이 따로 뜨지 않았다)

High
----
![image](https://user-images.githubusercontent.com/61008728/126570484-308b8ff7-42bc-438e-95cd-16cc5a9c4518.png)   
High도 Medium과 마찬가지로 img 태그로 공격을 시도해 보았다.  

![image](https://user-images.githubusercontent.com/61008728/126570506-c41cb15f-fa7d-4a44-9faa-43d8bba644a3.png)    
img 태그가 무시되지 않고 공격이 된 것을 확인할 수 있다.   

