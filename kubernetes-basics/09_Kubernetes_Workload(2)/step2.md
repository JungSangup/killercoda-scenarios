좀 더 나가서, 이번엔 **Deployment** 입니다.
먼저 YAML파일을 만들어보겠습니다. (~/lab 디렉토리 아래의 파일을 사용하셔도 됩니다.)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: my-nginx
    tier: frontend
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
  selector:
    matchLabels:
      app: my-nginx
  template:
    metadata:
      labels:
        app: my-nginx
      name: my-nginx
    spec:
      containers:
      - image: nginx:1.19.3
        name: my-nginx
        ports:
        - containerPort: 80
```

> 파일명은 nginx-deployment.yaml로 합니다.

어디서 많이 본 형식인데... 하시면 맞습니다.
ReplicaSet과 유사해요. (거의 동일)

---

일단 한번 생성해볼게요.

```bash
ubuntu@ip-10-0-1-14:~$ kubectl apply -f nginx-deployment.yaml
deployment.apps/nginx-deployment created
```

> **명령어** : `kubectl apply -f nginx-deployment.yaml`

이번엔 새로운 명령어 `kubectl get all`을 해볼까요?

```bash
ubuntu@ip-10-0-1-14:~$ kubectl get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-596ff98864-7rcc5   1/1     Running   0          51s
pod/nginx-deployment-596ff98864-8gzpg   1/1     Running   0          51s
pod/nginx-deployment-596ff98864-ccgxd   1/1     Running   0          51s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5d11h

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   3/3     3            3           51s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-596ff98864   3         3         3       51s
```

> **명령어** : `kubectl get all`

오~ 다 나오네요... ٩(ˊᗜˋ*)و

---

Pod들을 Label까지 같이 보려면 아래와 같이 하면 됩니다.

```bash
ubuntu@ip-10-0-1-14:~$ kubectl get po --show-labels
NAME                                READY   STATUS    RESTARTS   AGE    LABELS
nginx-deployment-596ff98864-7rcc5   1/1     Running   0          113s   app=my-nginx,pod-template-hash=596ff98864
nginx-deployment-596ff98864-8gzpg   1/1     Running   0          113s   app=my-nginx,pod-template-hash=596ff98864
nginx-deployment-596ff98864-ccgxd   1/1     Running   0          113s   app=my-nginx,pod-template-hash=596ff98864
```

> **명령어** : `kubectl get po --show-labels`


이제 Pod 하나를 삭제(delete)해 볼까요?

```bash
ubuntu@ip-10-0-1-14:~$ kubectl delete po nginx-deployment-596ff98864-7rcc5
pod "nginx-deployment-596ff98864-7rcc5" deleted
```

> **명령어** : `kubectl delete po [POD-NAME]`
> [POD-NAME] 에는 앞에서 조회된 POD 중 하나의 이름을 넣어주세요.

그리고, 다시 조회를 해보면...

```bash
ubuntu@ip-10-0-1-14:~$ kubectl get po --show-labels
NAME                                READY   STATUS    RESTARTS   AGE    LABELS
nginx-deployment-596ff98864-8gzpg   1/1     Running   0          3m9s   app=my-nginx,pod-template-hash=596ff98864
nginx-deployment-596ff98864-ccgxd   1/1     Running   0          3m9s   app=my-nginx,pod-template-hash=596ff98864
nginx-deployment-596ff98864-8sqsn   1/1     Running   0          11s    app=my-nginx,pod-template-hash=596ff98864
```

> **명령어** : `kubectl get po --show-labels`

새롭게 하나의 POD가 생성된 걸 볼 수 있습니다. ReplicaSet이 자기 역할을 다하고 있는 듯 하네요~ 이제 든든합니다.

---

이번엔 scale in/out 방법을 알아보겠습니다. (replicas를 조정)
명령형 커맨드 방식으로는 이렇게 할 수 있습니다.

```bash
ubuntu@ip-10-0-1-14:~$ kubectl scale deployment nginx-deployment --replicas=5
deployment.apps/nginx-deployment scaled
```

> **명령어** : `kubectl scale deployment nginx-deployment --replicas=5`

조회결과도 보겠습니다.

```bash
ubuntu@ip-10-0-1-14:~$ kubectl get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-596ff98864-6m6nz   1/1     Running   0          41s
pod/nginx-deployment-596ff98864-8gzpg   1/1     Running   0          8m50s
pod/nginx-deployment-596ff98864-8sqsn   1/1     Running   0          5m52s
pod/nginx-deployment-596ff98864-ccgxd   1/1     Running   0          8m50s
pod/nginx-deployment-596ff98864-sh7sh   1/1     Running   0          41s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5d11h

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   5/5     5            5           8m50s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-596ff98864   5         5         5       8m50s
```

> **명령어** : `kubectl get all`

명령형 커맨드에서 지정한 대로 Pod의 개수가 다섯개가 되었습니다. 새롭게 생성된 두 개의 Pod를 볼 수 있습니다.

---

`kubectl edit deployment nginx-deployment` 명령어로 생성된 리소스를 수정할 수도 있습니다.
마치 vi editor를 이용하여 YAML파일을 수정하는 것과 동일합니다.
한 번 해보세요.

`replicas`를 2로 바꾸고 저장후 빠져나옵니다. (`:wq`)

조회결과는 아래와 같습니다.

```bash
ubuntu@ip-10-0-1-14:~/mspt2/hands_on_files$ kubectl get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-596ff98864-8gzpg   1/1     Running   0          16m
pod/nginx-deployment-596ff98864-8sqsn   1/1     Running   0          13m

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5d11h

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   2/2     2            2           16m

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-596ff98864   2         2         2       16m
```

> **명령어** : `kubectl get all`

---

그리고, 마지막으로 선언형 방법을 적용하려면 처음 사용된 yaml파일을 수정해주시면 됩니다.
vi editor를 이용하여 `.spec.replicas`부분을 수정하면 됩니다. (4로 변경)

그리고, 마법의 주문 `kubectl apply`를 하는거죠.

```bash
ubuntu@ip-10-0-1-14:~$ kubectl apply -f nginx-deployment.yaml
deployment.apps/nginx-deployment configured
```

> **명령어** : `kubectl apply -f nginx-deployment.yaml`

조회결과는 아래와 같습니다.

```bash
ubuntu@ip-10-0-1-14:~$ kubectl get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-596ff98864-74x29   1/1     Running   0          63s
pod/nginx-deployment-596ff98864-8gzpg   1/1     Running   0          21m
pod/nginx-deployment-596ff98864-8sqsn   1/1     Running   0          18m
pod/nginx-deployment-596ff98864-tsqfk   1/1     Running   0          63s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5d11h

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   4/4     4            4           21m

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-596ff98864   4         4         4       21m
```

> **명령어** : `kubectl get all`

