
자주 사용되는 Helm 명령어들을 실습해 보겠습니다.

먼저 어떤 명령어들이 있는지 살펴볼까요?  
`helm --help`{{exec}}


이제 설치를 한 버 진행해볼까요?  
먼저 Repository를 add해줍니다.  
`helm repo add stable https://kubernetes-charts.storage.googleapis.com/`{{exec}}
`helm repo add bitnami https://charts.bitnami.com/bitnami`{{exec}}

Repository 목록도 볼 수 있습니다.  
`helm repo list`{{exec}}

검색도 가능하구요.  
`helm search repo stable`{{exec}}
`helm search repo bitnami`{{exec}}

Wordpress를 한 번 찾아볼까요?  
`helm search repo wordpress`{{exec}}

이제 설치를 진행해 보겠습니다.  
`helm repo update`{{exec}}

`helm install my-wordpress bitnami/wordpress`{{exec}}

설치된 Helm chart는 Release라고 합니다.  
Release의 목록은 다음 명령으로 조회할 수 있구요.  
`helm list`{{exec}}

쿠버네티스 명령어로 어떤 리소스들이 생성됐나 볼까요?
`kubectl get all`{{exec}}

와우~ 뭔가 Wordpress 소프트웨어에 필요한 모든게 한 번에 설치가 된 것 같네요. 패키지로...  

이게 바로 Helm 이랍니다.


chart를 다운로드(pull)도 해볼게요.  
`helm pull bitnami/wordpress --version 11.0.11`{{exec}}

tar로 받아지네요.  
압축도 풀어볼까요?  
`tar -xvf wordpress-11.0.11.tgz`{{exec}}

어떤 파일들이 있는지 한 번 살펴보겠습니다.  
`cd wordpress`{{exec}}

`ls -al`{{exec}}


여기까지 Helm 에 대해 알아보았습니다.

수고하셨습니다.  (〃･ิ‿･ิ)ゞ
