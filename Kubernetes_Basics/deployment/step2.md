
혹시 이전 실습에서 경로를 왔다갔다 하셨으면 실습실로 다시 이동하시구요.  
`cd /root/lab`{{execute T1}}


이번엔 RollingUpdate 입니다.

Terminal도 깨끗이 정리하고 하죠.  
`clear`{{execute T1}}

`clear`{{execute T2}}

테스트를 위한 애플리케이션을 준비하구요.  
`kubectl apply -f nginx-rollingupdate.yaml`{{execute T1}}

`kubectl get all`{{execute T1}}

생성된 Deployment의 정보를 보고 현재 실행된 이미지를 확인해봅니다.  
`kubectl describe deployment nginx-deployment | grep -i image`{{execute T1}}

버젼을 변경하구요.  
`sed -i 's/image: nginx:1.18/image: nginx:1.19/g' nginx-rollingupdate.yaml`{{execute T1}}

확인할 명령어를 실행한 후에  
`kubectl get pods --watch`{{execute T2}}

업데이트를 합니다.  
`kubectl apply -f nginx-rollingupdate.yaml`{{execute T1}}

deployment에 잘 반영이 됐나요?  
`kubectl describe deployment nginx-deployment | grep -i image`{{execute T1}}

새로 생성된 Pod의 정보도 확인해봅니다.  
`kubectl describe pod [POD-NAME] | grep -i image`


앞에서와 마찬가지로 롤백도 해보세요..
자세한 설명은 생략합니다.  

`kubectl rollout history deployment nginx-deployment`{{execute T1}}

`kubectl get pods --watch`{{execute T2}}

`kubectl rollout undo deployment nginx-deployment --to-revision=1`{{execute T1}}

`kubectl describe pod [POD-NAME] | grep -i image`

`kubectl delete -f nginx-rollingupdate.yaml`{{execute T1}}
