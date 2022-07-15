컨테이너와 이미지, 그리고 Layer에 대한 실습입니다.

​     
먼저 현재 있는 컨테이너 이미지를 모두 삭제하겠습니다.

`docker rmi --force $(docker images -aq)`{{exec}}

​     
모두 삭제됐는지 볼까요?

`docker images`{{exec}}

​     
이제 우리 도커가 어떤 스토리지 드라이버를 사용하는지 알아보겠습니다.

`docker info | grep -i storage`{{exec}}

[스토리지 드라이버](https://docs.docker.com/storage/storagedriver/select-storage-driver/)에 따라서 이미지가 저장되는 장소가 달라집니다.

우리 시스템은 `overlay2` 드라이버이기 때문에 아래 경로가 사용됩니다.

`ls -al /var/lib/docker/overlay2`{{exec}}

깨끗하게 정리된 상태에서 시작해볼게요.

nginx 이미지를 하나 pull 하구요.

`docker pull nginx`{{exec}}

확인을 합니다.

`docker images`{{exec}}

​     
이제 다시 저장된 위치가 어떻게 바뀌었나 확인해볼까요?

`ls -al /var/lib/docker/overlay2`{{exec}}

뭔가 많이 생겼네요.
   
혹시 눈치 채셨나요?

`docker pull` 할때 표시된 layer만큼 overlay 아래 디렉토리가 생성된걸...

​     
이번엔 컨테이너를 실행해 보겠습니다.

`docker run --detach --label "color=red" nginx`{{exec}}

다음 실습을 위해서 label(color=red)을 달아뒀습니다.

​     
잘 실행되고 있나 살펴볼게요.

`docker ps`{{exec}}

​     
이제 `overlay2` 디렉토리는 어떻게 바뀌어 있을까요?

`ls -alt /var/lib/docker/overlay2`{{exec}}

두 개의 디렉토리가 더 생긴걸 볼 수 있습니다.(`-t` 옵션을 사용하여 최근 디렉토리를 상위에 표시함.)

​      
이게 우리가 배운 R/W Layer인 Container layer입니다.

실행되는 컨테이너에서 발생하는 모든 변경사항은 바로 여기 기록되게 됩니다.

​     
그럼, 하나를 더 실행하면 어떻게 될까요?

`docker run --detach --label "color=blue" nginx`{{exec}}

이번엔 `color=blue` label을 달아뒀습니다.

​     
컨테이너를 확인해볼까요?

`docker ps`{{exec}}

​     
그리고, `overlay2`디렉토리에는 R/W Layer만 추가된 걸 확인할 수 있습니다.

`ls -al /var/lib/docker/overlay2`{{exec}}

​     
같은 이미지로 여러개의 컨테이너를 실행해도, R/O Layer는 공유하고 R/W Layer만 추가해서 만들어지네요.

​     
다음 실습을 위해서 blue는 삭제할게요.

`docker rm --force $(docker ps --filter "label=color=blue" --quiet)`{{exec}}
