앞서 우리는 도커가 사용하는 레이어 파일시스템에 대해 알아보았습니다.  
컨테이너가 실행될 때 마다 새로운 레이어(Container layer, R/W layer)가 생성되고, 컨테이너가 삭제될 경우 그 레이어의 내용은 사라지게 됩니다.  

직접 한번 컨테이너를 생성해서 볼까요?

먼저 ubuntu 를 하나 실행합니다.
`docker run --name my-ubuntu --detach ubuntu bash -c "echo 'Hello Docker...' > /test.txt && tail -f /dev/null"`{{execute}}  
간단한 txt파일을 하나 만들고 컨테이너를 `running`상태로 두기위해서 `tail`명령을 실행했습니다.

이제 우리가 만든 파일을 한 번 볼까요?
`docker exec my-ubuntu cat /test.txt`{{execute}}
우리가 적어놓은 테스트문구가 보일거예요.

이제 같은 ubuntu이미지를 이용해서 새로운 컨테이너를 실행하고, `test.txt`파일이 있나 살펴봅시다.
`docker run -it ubuntu ls /`{{execute}}

당연히 없겠죠...   
우리가 앞에서 알아본 레이어가 정확히 잘 적용되는 것 같네요.

매번 이렇게 우리가 작업한 내용이 저장되지 않고 사라지면, 이런 환경은 쓸 수가 없겠죠?
다음 스템에서는 영속성을 위한 방법을 알아보겠습니다.
