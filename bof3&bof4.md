Buffer Overflow
---------------
Buffer Overflow : 시스템 해킹의 대표적인 공격 방법 중 하나   

![overflow](https://ifh.cc/g/BfKp5N.jpg)
버퍼를 초과하는 양이 입력될 경우, 다른 영역을 침범하여 갚을 덮어쓰는 성질이 있다.   
그림을 보면 버퍼의 공간은 4칸인데, abcdef가 들어가서 다른 영역까지 침범하여 6칸을 차지하게 된다.    
이때, 버퍼가 아닌 다른 영역에 123456이 차지하고 있었다고 하면, 이 영역을 밀어서 차지하는 것이 아니라 위에 그대로 덮어쓰게 된다.   
이런 일이 발생하는 이유는 gets, scanf, strcpy와 같은 함수들이 취약하기 때문이다.   

Stack Frame
-----------
![overflow](https://ifh.cc/g/blCnig.png)
RBP : Stack의 시작점을 알리는 용도로 사용   
RET : return address   
-> main 함수에서 vuln 함수로 갔다가 main 함수로 다시 돌아가기 위해 스택에 main 함수의 복귀주소를 저장   
빨간 영역을 초과하여 입력하면, 함수가 끝난 후 돌아갈 주소가 담겨있는 RET에 원하는 값을 넣을 수 있다   

bof3 문제
--------
bof3.c
![bof3](https://ifh.cc/g/a1znrl.jpg)   

if(innocent == KEY)를 만족하면 system("/bin/sh");를 사용할 수 있다   
스택에서 innocent의 위치를 찾아내서 거기까지 아무 문자열로 덮어 씌우고, 이후에 KEY값인 a(0x61)을 입력해주면 된다   

gdb bof3으로 gdb 시작   
![disas](https://ifh.cc/g/JZ8HCF.jpg)   

cmp는 두 값을 비교하는 명령어로 C코드에서 if(innocent == KEY)에 해당   
rbp-0x4가 innocent, 0x61이 KEY값    
innocent 위치를 찾아서 해당 값을 KEY값으로 덮어 씌우면 된다  

<시작 주소 찾기>
시작 주소를 찾아야 rbp-0x4까지 값을 입력해서 덮어 씌울 수 있다   
lea 명령어는 데이터를 옮겨주는 명령어로 이 부분을 보면 시작 주소를 알 수 있다      
rbp-0x90이 innocent의 시작주소가 되므로, 위에서 구한 rbp-0x4와 시작 주소 rbp-0x90이 얼마나 떨어져 있는지 알아야 한다   

![pd](https://ifh.cc/g/gAnnS0.jpg)   

breakpoint를 지정하고 run한 다음, 두 값의 거리를 구한다   
아무 값이나 140만큼 채워서 innocent 값에 도달하게 하고 이후에 KEY값인 a를 입력한다   
![input](https://ifh.cc/g/COYbt1.png)   

cf) ;cat을 입력해서 해당 파일을 실행하게 함

이렇게 cat bof4의 패스워드를 찾을 수 있다

