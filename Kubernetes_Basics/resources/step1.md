기본적인 쿠버네티스 명령어들에 대해 알아보겠습니다.



제일먼저 도움말을 볼까요?  
`kubectl --help`{{execute}}

여러가지 명령어들이 보이는군요.

버젼을 알아보려면,  
`kubectl version`{{execute}}

쿠버네티스 클러스터 정보를 확인하려면,  
`kubectl cluster-info`{{execute}}

우리 클러스터의 노드목록은  
`kubectl get nodes`{{execute}}  
로 알아볼 수 있습니다.

`--output wide`옵션(또는, `-o wide`)을 주면 더 많은 정보를 보여줍니다.  
`kubectl get nodes --output wide`{{execute}}

`help`와 유사하게 쿠버네티스 리소스들의 정의와 설명을 보려면 `kubectl explain` 명령을 사용하면 됩니다.

예를들어 POD에 대해 알아보려면 
`kubectl explain pod`{{execute}}
와 같이 실행하면 됩니다.




이제 우리 클러스터에 존재한는 리소스들 중 `Pod`에 대해 좀 더 자세히 알아볼까요?

먼저 현재 존재하는 POD 목록은  
`kubectl get pods`{{execute}}

를 실행하면 볼 수 있습니다.



음. 아무것도 없군요...  
지금은 default 네임스페이스에서 조회를 한 경우입니다.   

> `--namespace`로 네임스페이스를 지정하지 않으면 default 네임스페이스를 기본으로 합니다.   



다른 네임스페이스는 뭐가 있을까요?  
네임스페이스를 보려면,  
`kubectl get namespaces`{{execute}}

를 실행하면 됩니다.  



이번에는 `Pod`목록을 조회하는데, `--all-namespaces`옵션을 추가해볼까요?  
`kubectl get pods --all-namespaces --output wide`{{execute}}

시스템이 사용하는 `Pod`들을 보려면 `kube-system` 네임스페이스를 보면 됩니다.  
`kubectl get pods --namespace kube-system --output wide`{{execute}}

그 중에 하나, `kube-scheduler`를 좀 더 자세히 볼까요?  
정보를 yaml형태로 볼 수 도 있구요.  
`kubectl get pod kube-scheduler-controlplane --namespace kube-system --output yaml`{{execute}}

`describe`명령으로 자세한 정보를 조회할 수도 있습니다.  
`kubectl describe pod kube-scheduler-controlplane --namespace kube-system`{{execute}}

Pod의 로그를 보려면 아래와 같이 하시면 됩니다.  
`kubectl logs -n kube-system kube-scheduler-controlplane`{{execute}}



여기까지, 기본적인 kubectl 명령어들을 알아보았습니다.
더 많은 내용은 차차 알아볼게요~  ٩(ˊᗜˋ*)و