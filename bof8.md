bof8문제
=======
bof8.c
------
![image](https://user-images.githubusercontent.com/61008728/125883211-85c93942-a8b9-431d-aaf1-8d1024548452.png)     

<접근 방식>   
이 문제는 RET Sled 공격 방식으로 접근한다     

가장 먼저, RET Sled의 필수 요소인 ret 명령어의 주소가 필요하다   
ret 명령어의 주소를 찾기 위헤 gdb bof8을 실행시킨다   
![image](https://user-images.githubusercontent.com/61008728/125891753-64870c77-999b-4691-8eac-69755008c895.png)
-> 맨 마지막 줄에서 ret 명령어의 주소 '0x00000000000012b6'를 찾을 수 있다   

Cf. [getenv() 함수]   
: SHELLCODE라는 이름의 환경 변수 주소를 알아내기 위해 사용되는 함수   
![image](https://user-images.githubusercontent.com/61008728/125883746-88714cd1-1ad8-41f0-adf9-dc2f08ef32fe.png)   
bof8.c 코드를 보면 getenv() 함수가 사용되었다   
SHELLCODE라는 이름의 환경변수를 지정하는 명령을 입력한 후, getenv() 함수를 사용하여 SHELLCODE 환경변수의 주소를 구해야 한다   
   
![image](https://user-images.githubusercontent.com/61008728/125892185-61f97881-2733-4534-a4a3-6019bb960047.png)
(env:SHELLCODE -> 0x7fffffffee65) -> shellcode 환경변수의 주소   

<페이로드 구성>   
RET Sled 방실을 사용하여 지정된 환경변수의 주소를 찾았으니, 이제 payload를 구성해볼 차례이다   
'dummy + ret 주소 + 리턴 주소' 형태로 전달이 된다   

우선 리턴 주소값 전까지 채워줘야 하는 바이트를 계산해 주고, 거기까지 채운 다음 리턴 주소값을 덮어야 한다   
![image](https://user-images.githubusercontent.com/61008728/125893342-0956dba6-5c06-4499-8c4c-765da0306fec.png)
bof8.c코드를 보면 buf 사이즈가 8이고, 64비트에서 SPF의 사이즈는 8바이트이므로 총 16바이트가 buf와 리턴 주소값 사이의 거리이다   

buf와 리턴 주소값 사이의 길이(16바이트) + 쉘코드 환경변수의 주소(8바이트)로 총 24바이트가 전달되는 것이다   
그런데 ret 명령어 주소가 '0x00000000000012b6'로 8바이트를 차지하고 있다   
dummy + ret 주소값이 16바이트가 되어야 하므로, dummy값은 16-8 = 8바이트가 된다   

정리해보면 아래 그림과 같다   
![image](https://user-images.githubusercontent.com/61008728/125894373-7172a96d-2656-4cb3-a9f1-76b3479bd486.png)   

이에 따라 아무 문자 'a'로 8바이트 만큼 채우고 그 뒤에 ret 주소값으로 총 16바이트를 채운 다음, 리턴 주소값을 덮어 씌워서 페이로드를 구성한다   
![image](https://user-images.githubusercontent.com/61008728/125894579-7d16a768-6f75-46b7-ae5e-6673dee30e0f.png)

이후, cat bof9.pw를 입력하여 bof9의 패스워드를 찾을 수 있다   

bof10문제
=======
bof10.c
------
![image](https://user-images.githubusercontent.com/61008728/125967188-0e89bfe1-7a35-44c1-bcca-e0f1fd0abb6e.png)
