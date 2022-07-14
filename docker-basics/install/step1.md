

기존 설치된 패키지가 있으면 삭제 합니다.
`sudo apt-get -y remove docker docker-engine docker.io containerd runc`{{execute}}


### Install using the repository

이제 시작해볼까요?

먼저 apt 패키지 index 를 한 번 업데이트 해줍니다.
`apt-get update`{{execute}}

그리고, HTTPS 사용을 위해 다음 패키지들을 설치해줍니다.      
```
sudo apt-get -y install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```{{execute}}

Docker 공식 GPG 키를 추가해주고,   
`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`{{execute}}

Stable repository를 추가합니다.   
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```{{execute}}
   
준비과정이 길었네요...  
이제 진짜 Docker engine 설치 입니다.    
`sudo apt-get update`{{execute}}  

`sudo apt-get -y install docker-ce docker-ce-cli containerd.io`{{execute}}

설치확인을 위해서 다음 명렬어를 실행해보세요.  
`sudo docker run hello-world`{{execute}}

--- 끝 ---   
(ง˙∇˙)ว


더 자세한 내용은 https://docs.docker.com/engine/install/ubuntu/ 를 참조하세요.
