도커 네트워크를 이용해서 아래 그림과 같이 멀티 컨테이너 애플리케이션을 구성해 보겠습니다. 우리 애플리케이션에 Database(MySQL)를 연결해서 서비스하도록 합니다.



<img src="./assets/multi-app-architecture.png" alt="ToDo List App." />



먼저 `docker network create`명령으로 bridge network을 하나 생성하겠습니다.  
`docker network create todo-app`{{exec}}

> 생성된 `network`는 `docker network inspect [OPTIONS] NETWORK [NETWORK...]` 명령어를 이용하여 상세 내용을 확인할 수 있습니다.




그리고, 생성한 네트워크를 이용해서 `mysql`을 실행합니다.

```
docker run -d \
    --network todo-app --network-alias mysql \
    --volume todo-mysql-data:/var/lib/mysql \
    --env MYSQL_ROOT_PASSWORD=secret \
    --env MYSQL_DATABASE=todos \
    --env LANG=C.UTF-8 \
    --name my-mysql \
    mysql:5.7 \
    --character-set-server=utf8mb4 \
    --collation-server=utf8mb4_unicode_ci
```{{exec}}


이전에 배운 `volume`도 사용하네요.
데이터의 영속성을 위해서 데이터는 볼륨에 저장하도록 구성했습니다.
`docker volume create`명령으로 생성하지 않아도, 없는경우엔 도커가 알아서 생성해줍니다. ◟(ˊᗨˋ)◞  

이제 mysql에 로그인해서 데이터베이스가 잘 생성됐나 봅시다.  
`docker exec -it my-mysql mysql -p`{{exec}}

Password는 `secret`{{copy}} 입니다.  

`mysql>` 프롬프트가 표시되면, 정상적으로 로그인 된겁니다.  

이제 mysql 명령어로 database를 조회해볼까요?  
`SHOW DATABASES;`{{exec}}  
`todos`라는 database가 보이시나요?  

이제 mysql 에서 나갈게요.  
`exit;`{{exec}}


이번에는 우리의 샘플 애플리케이션을 mysql과 연계해서 실행해 보겠습니다.
````
docker run -dp 3000:3000 \
  --network todo-app \
  --env MYSQL_HOST=mysql \
  --env MYSQL_USER=root \
  --env MYSQL_PASSWORD=secret \
  --env MYSQL_DB=todos \
  --name my-todo-manager \
  [USER-NAME]/101-todo-app:1.0.0
```{{copy}}  
> [USER-NAME] 에는 여러분의 정보로 채워넣어 주세요.

`--network`옵션으로 mysql과 동일한 네트워크로 설정했고, `--env`를 이용해서 mysql 연계에 필요한 환경변수들을 설정해 주었습니다.

우리 애플리케이션의 로그를 한 번 볼까요?  
`docker logs my-todo-manager`{{exec}}  
mysql 과 잘 연결됐다는 로그가 보이시나요?

이제 아래 링크를 통해서 우리 애플리케이션 화면으로 이동해보세요.  
그리고, 오늘 할 일을 또 입력해 보시구요.

[ToDo List Manager]({{TRAFFIC_HOST1_3000}})

<img src="./assets/todo-list-sample3.png" alt="ToDo List App." />

자, 이제 다시 mysql 로 로그인해서 table에 잘 저장되어 있나 확인해 보겠습니다.  
`docker exec -it my-mysql mysql -p todos`{{exec}}

`secret`{{copy}}

`mysql>`프롬프트가 나오면 아래 쿼리문으로 조회해보세요.  
`select * from todo_items;`{{exec}}  
화면에서 입력한 오늘의 할 일이 todo_items table에 잘 저장되어 있나요?

`exit;`{{exec}} 명령으로 나와주시구요.

여기까지 실습을 마치겠습니다.  ˘◡˘
