이제 한 단계 더 나가서, 세련된 Pod운영 방법을 알아보겠습니다.  

먼저 실습파일이 있는 곳으로 이동하시구요.  
`cd /root/lab`{{execute}}

첫 번째는 ReplicaSet 입니다.

yaml파일을 한 번 볼까요?
`cat nginx-replicaset.yaml`{{execute}}

아래 링크에서 보셔도 됩니다.  
https://github.com/JungSangup/katacoda-scenarios/blob/master/Kubernetes_Basics/resources/assets/nginx-replicaset.yaml

`spec` 부분을 보시면, 우리가 원하는 Pod(Contailer)에 대한 spec이 보이고, 그 위에 `replicas: 3` 이라는 부분이 보이네요.  
이 부분이 핵심입니다.  
나는 nginx Pod를 세 개 원한다고 선언한 것입니다.

특별한 얘기가 없으면 yaml파일을 이용한 리소스 생성은 `kubectl apply`를 쓰시면 됩니다. (-> 국룰!!!)  

ReplicaSet을 생성해볼까요?  
`kubectl apply -f nginx-replicaset.yaml`{{execute}}

조회도 해보시구요.  
`kubectl get replicasets -o wide`{{execute}}

상세조회도...  
`kubectl describe replicasets nginx-replicaset`{{execute}}

우리는 Pod를 생성하지는 않았지만 `Pod`도 생성됐습니다.  
ReplicaSet이 하는 일이 그런거니까요.  
`kubectl get pods --show-labels`{{execute}}

이제 뭔가 좀 자동으로 돌아가는 모양새가 나오네요~

이제 생성한 리소스들을 삭제해볼게요.  
`apply` 를 `delete` 로 바꿔주시면 됩니다. (ง˙∇˙)ว
`kubectl delete -f nginx-replicaset.yaml`{{execute}}


좀 더 나가서, 이번엔 Deployment 입니다.  
사실 얘가 메인이예요.

이렇게 생겼구요.  
`cat nginx-deployment.yaml`{{execute}}

어디서 많이 본 형식인데... 하시면 맞습니다.  
ReplicaSet과 유사해요. (거의 동일)

일단 한번 생성해볼게요.  
`kubectl apply -f nginx-deployment.yaml`{{execute}}

이번엔 새로운 명령어 `kubectl get all`을 해볼까요?  
`kubectl get all`{{execute}}

오~ 다 나오네요...  ٩(ˊᗜˋ*)و

`Pod`들을 `Label`까지 같이 보려면,  
`kubectl get po --show-labels`{{execute}}

이제 Pod 하나를 삭제(delete)해 볼까요?  
`kubectl delete po [POD-NAME]`{{copy}}

그리고, 다시 조회를 해보면...  
`kubectl get po --show-labels`{{execute}}

ReplicaSet이 자기 역할을 다하고 있는 듯 하네요~
이제 든든합니다.


이번엔 Scale-up/down 방법을 알아보겠습니다.  
replica 를 조정하는거죠.  

이렇게 명령형으로 할 수도 있구요.  
`kubectl scale deployment nginx-deployment --replicas=5`{{execute}}

조회는 이렇게...  
`kubectl get all`{{execute}}

생성된 리소스를 수정할 수도 있습니다.  
`kubectl edit deployment nginx-deployment`{{execute}}

`kubectl get all`{{execute}}

그리고, 마지막으로 선언형 방법을 적용하려면 먼저 yaml파일을 수정해주시구요.
`vi nginx-deployment.yaml`{{execute}}

`spec.replicas`를 수정해주시면 됩니다.  

그리고, 마법의 주문 `kubectl apply`를 하는거죠.  
`kubectl apply -f nginx-deployment.yaml`{{execute}}


`kubectl apply -f abc.yaml` , `kubectl delete -f abc.yaml` 이 두개만 아셔도 어지간한 관리는 가능할거예요.
