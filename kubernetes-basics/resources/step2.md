이번에는 Pod를 관리하는 명령어들을 알아보겠습니다.

먼저 실습용 파일들이 있는 위치로 이동하시구요.  
`cd /root/lab`{{execute}}

`Pod`를 생성하는 세 가지 방법을 알아보겠습니다.

첫 번째는 `명령형 커맨드 (Imperative commands)` 입니다.  
`Pod`를 직접 동작시키는 방법입니다.  
`kubectl run my-nginx1 --image=nginx`{{execute}}

생성된 `Pod`를 볼까요?  
`kubectl get pods -o wide`{{execute}}

두 번째는, `명령형 오브젝트 구성(Imperative object configuration)` 입니다.  
미리 정의된 yaml파일을 이용해서 생성(creat) 합니다.  
`kubectl create -f nginx2-pod.yaml`{{execute}}

생성된 `Pod`를 볼까요?
`kubectl get pods -o wide`{{execute}}

세 번째는, `선언형 오브젝트 구성(Declarative object configuration)` 입니다.  
어떤 작업을 할지(create, update) 명시하지 않고 단순히 `apply`라는 키워드를 씁니다.  
무엇을 할지는 쿠버네티스가 알아서 해줍니다.   (ง˙∇˙)ว
`kubectl apply -f nginx3-pod.yaml`{{execute}}

생성된 `Pod`를 볼까요?
`kubectl get pods -o wide`{{execute}}

세 가지 방법의 차이는 아래 링크에 자세히 나와있습니다.  
참고하세요.  
https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/object-management/

그리고, 선언형 관리에 대해 좀 더 자세히 알아보려면 아래 링크를 참조하세요.  
https://kubernetes.io/ko/docs/tasks/manage-kubernetes-objects/declarative-config/

선언형(Declarative) 이라는 말을 잘 기억해두세요.  
그리고 `kubectl apply ~` 명령어도 많이 쓰이니, 잘 기억해 두시구요.  

생성한 모든 Pod를 삭제할게요.  
`kubectl delete pod --all`{{execute}}
