Horizontal Pod Autoscaler(HPA)를 이용하여 자동으로 Pod의 개수를 조절하는 실습입니다.  
실습 내용은 https://kubernetes.io/ko/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/ 를 기반으로 하였습니다.  


먼저 자원 모니터링을 위한 metrics-server를 준비합니다.  
`kubectl apply -f /root/lab/metrics-server/`{{execute T1}}

바로 적용되지는 않습니다.  
아래 명령어의 결과가 나올 때 까지 조금 기다려주세요.  
`kubectl top node`{{execute T1}}

이제 준비가 됐으면, 다음 명령어를 실행하여 간단한 테스트용 Pod 를 준비합니다.  
`kubectl apply -f https://k8s.io/examples/application/php-apache.yaml`{{execute T1}}

> 혹시 위의 방법이 안되면 아래 명령어로 해주세요.  
`kubectl apply -f php-apache.yaml`{{execute T1}}


이제 hpa를 생성합니다.  
명령어는 다음과 같습니다.  
CPU 사용량을 50%로 유지하기 위해서 Pod의 개수를 1 에서 10 사이로 조정하라는 의미입니다.  
`kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10`{{execute T1}}

잘 만들어졌나 볼까요?  
`kubectl get hpa`{{execute T1}}


이제 부하를 줄 도우미 친구 입니다.  
간단한 sh 명령어를 실행할 pod(load-generator)를 만들어서 반복문을 실행합니다.  
`kubectl run -it --rm load-generator --image=busybox /bin/sh`{{execute T1}}

`while true; do wget -q -O- http://php-apache; done`{{execute T1}}

터미널을 하나 더 열어서 아래 명령어를 실행해 놓고 Pod의 개수가 어떻게 바뀌는지 볼까요?  
`kubectl get pods --watch`{{execute T2}}


어느정도 시간이 지나서, Pod가 늘어나는걸 보셨으면, Terminal1의 반복문을 중지해주세요.  
`Ctrl + c`로 중지하시면 됩니다.  

얼마 있으면 다시 Terminal2에서 Pod가 줄어드는 걸 볼 수 있습니다.

끝~~~
