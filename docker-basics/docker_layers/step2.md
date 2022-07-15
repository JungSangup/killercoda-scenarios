이제 우리만의 새로운 이미지를 만들어 보겠습니다.

여려가지 방법이 있지만 이번 실습은 실행중인 컨테이너의 내용을 반영한 새로운 이미지를 만드는 것입니다.

​     
앞에서 실행한 nginx를 활용하도록 하겠습니다.

먼저 잘 실행되고 있나 보구요.

`docker ps`{{exec}}

잘 살아있으면, 다음 명령으로 새로운 이미지를 만들어 보겠습니다.

`docker commit $(docker ps --filter "label=color=red" --quiet) nginx:my-tag`{{exec}}

뭔가 복잡해 보이지만 별거 아닙니다.

$(...) 이 부분은 우리가 Label(color=red)을 붙인 컨테이너의 ID를 조회하는 명령입니다.

git의 `commit` 명령어 처럼, docker도 `commit` 명령을 이용해서 새로운 이미지를 만들어 줍니다.

​     
이제 어떤 이미지가 있나 조회해볼까요?

`docker images`{{exec}}

방금 우리가 `docker commit` 명령으로 만든 `nginx:my-tag`가 보이네요.

​     
`overlay2` 디렉토리는요?

`ls -alt /var/lib/docker/overlay2`{{exec}}

네, 여기도 하나가 더 생겼네요.

​     
이제 실행중인 컨테이너까지 멈추고 삭제까지 해볼게요.

`docker rm -f $(docker ps --filter "label=color=red" -q)`{{exec}}

​     
이제 `overlay2` 디렉토리는 어떻게 되었을까요?

`ls -alt /var/lib/docker/overlay2`{{exec}}

​     
처음 `docker pull`해서 생성된 layer에, `docker commit`해서 생성된 레이어만 하나 추가되어 있네요. (+1)

아래 그림을 다시한 번 떠올려 보면서 마무리 하겠습니다.

<img src="./assets/container-layers.jpeg" alt="Container layers" />
​     


이번 실습은 여기까지 입니다.  

＿〆(。╹‿ ╹ 。)
