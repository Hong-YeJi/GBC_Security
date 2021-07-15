bof5문제
=======
bof5.c  
------
![bof5.c](https://ifh.cc/g/M6cqBL.jpg)    
쉘 상에서 쉘을 실행시키려면 /bin/sh라는 명령을 내려야 한다    
그런데, bof5.c 코드를 보면 innocent와 KEY가 같을 때 system(“/bin/sh”); 코드가 아닌 system(buf);로 되어 있다      
그렇기 때문에, system 함수를 실행시켜서 /bin/sh를 인자로 넘겨 쉘을 실행시켜야 한다    
인자로 넘길 때, 문자열을 읽어 오기 때문에 '/bin/sh\x00'형태로 넘겨주어 문자열을 덮어 씌운다    

<buf와 innocent와의 거리 구하기>   
----------------------------
![image](https://user-images.githubusercontent.com/61008728/125748502-a50ccfd0-cb50-4ba0-9128-1f7115c92450.png)   
cmp는 두 값을 비교하는 명령어로, C코드에서 if(innocent == KEY)에 해당하기 때문에 여기에서 innocent의 위치 rbp-0x4를 구할 수 있고,    
lea 부분에서 innocent의 시작 주소 rbp-0x90을 찾을 수 있으므로 둘의 차를 구하면 140이 나온다.   
   
![image](https://user-images.githubusercontent.com/61008728/125748827-039d60d1-0ad5-4b11-a95e-b99e7bc967d3.png)   
스택에서 innocent의 위치를 찾아내서 거기까지 /bin/sh\x00와 아무 문자로 140크기 만큼 문자열로 덮어 씌우고, 이후에 KEY값인 0x12345678을 입력해주면 된다   
/bin/sh\x00이 8바이트를 차지하므로 아무 문자인 ‘x’는 132만큼 전달하고, 이후에 덮어 씌울 KEY값은 little endian 방식으로 \x78\x56\x34\x12와 같이 입력한다

bof6문제
=======
![image](https://user-images.githubusercontent.com/61008728/125749564-dc53ce10-b50e-4705-a48e-182dca0eef52.png)   
buf 사이즈가 128이고 64비트에서 SFP는 8바이트를 덮어줘야 하므로 buf와 return address까지의 거리가 136이다   

![image](https://user-images.githubusercontent.com/61008728/125750072-d3f52f33-2dcf-4829-abdb-1352ef737201.png)
shell code와 아무 문자를 136만큼 전달하는데, 쉘 코드가 27바이트를 차지하므로 136 - 27 = 109바이트만큼 'a'를 전달한다    

그 뒤에 쉘 코드 주소값을 덮어줄 때, ASLR이 적용되어 쉘 코드 주소값이 계속 바뀌는 문제가 있었는데   
docker run -it --privileged ccss17/bof로 접속하면 shell code 주소값이 변하지 않아서 해결할 수 있었다   
![image](https://user-images.githubusercontent.com/61008728/125768221-3e346345-09ba-4365-abca-9d2778920044.png)

bof7문제
=======
![image](https://user-images.githubusercontent.com/61008728/125768498-defc7830-e66e-40c7-b156-af76978ee141.png)
