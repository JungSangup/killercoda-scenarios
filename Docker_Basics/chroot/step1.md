

컨테이너의 격리 개념을 이해하기 위해서 chroot를 이용한 격리를 실습해 보도록 하겠습니다.

먼저 `chroot`가 어떤 명령어인지 알아볼까요?

`chroot --help`{{execute}}

root 디렉토리를 새로운 경로(NEWROOT)로 지정하는거라고 하네요.

​     
그럼, 이제 실제로 `chroot`명령어를 실행해 보겠습니다.

먼저 root로 지정할 디렉토리를 하나 만들구요

`mkdir new_root`{{execute}}

그리고, 방금 만든 디렉토리를 루트로 변경합니다.

`chroot ./new_root /bin/bash`{{execute}}

​     
아마, 오류가 발생할거예요.

`/bin/bash`가 없다고 하네요.

​     

복사를 해줄게요.

`mkdir ./new_root/bin`{{execute}}

`cp /bin/bash ./new_root/bin/`{{execute}}

​     
디렉토리 구조를 한 번 볼까요?

`tree ./new_root`{{execute}}

bin 디렉토리에 bash 파일이 보이네요.

​     
이제 다시한번 `chroot`를 실행해볼까요?

`chroot ./new_root /bin/bash`{{execute}}

​     
역시 아직 안되네요... ( Ĭ ^ Ĭ )  

`/bin/bash` 실행파일 하나만으로는 bash쉘을 실행할 수 없어서 그런거랍니다.

`ldd` 명령어(List Dynamic Dependencies) 를 이용해서 필요한 라이브러리를 알아보고 추가로 복사를 해줄게요.

​     
먼저 뭐가 필요한지 보구요.

`ldd /bin/bash`{{execute}}

​     
그리고, 복사를 해줍니다.

```
mkdir -p ./new_root/lib/x86_64-linux-gnu/ ./new_root/lib64
cp /lib/x86_64-linux-gnu/libtinfo.so.6 ./new_root/lib/x86_64-linux-gnu/
cp /lib/x86_64-linux-gnu/libdl.so.2 ./new_root/lib/x86_64-linux-gnu/
cp /lib/x86_64-linux-gnu/libc.so.6 ./new_root/lib/x86_64-linux-gnu/
cp /lib64/ld-linux-x86-64.so.2 ./new_root/lib64
```{{execute}}  

​     
디렉토리 구조 다시한번 보구요.

`tree ./new_root`{{execute}}

​     
그리고, locale관련 처리를 하지않으면 warning이 발행하니, 아래 명령어도 한번 실행해주세요.

`export LC_ALL=`{{execute}}

​     
다시 `chroot`를 해봅니다.

`chroot ./new_root /bin/bash`{{execute}}

​    
꒰( ˵¯͒ꇴ¯͒˵ )꒱ 드디어 성공입니다.

​     
`pwd`{{execute}} 도 해보시구요, `/bin/bash --version`{{execute}} 도 해보세요.

아래 명령어는 될까요? 안될까요?  ʅ（´◔౪◔）ʃ  

`ls -al`{{execute}}
