이제 도커 볼륨(Volume)을 이용해서 데이터를 유지하는 방법을 알아보겠습니다.

우리 샘플 애플리케이션(Todo List Manager)는 SQLite database를 사용하고 있습니다.  
데이터는 `/etc/todos/todo.db` 에 파일로 저장이 되고 있구요.

이제 도커 볼륨을 이용해서 데이터가 저장되는 위치를 호스트의 경로로 바꿔보겠습니다.

먼저 도커 볼륨을 하나 생성합니다.
`docker volume create todo-db`{{execute}}  

생성된 볼륨을 확인하려면 아래 명령어를 사용하면 됩니다.
`docker volume list`{{execute}}

그리고, 볼륨의 더 자세한 정보를 알아보려면 아래 명령어를 사용하면 됩니다.
`docker volume inspect todo-db`{{execute}}  
`Mountpoint`가 바로 실제 데이터가 저장되는 위치입니다.

이제 방금 생성한 볼륨을 우리 애플리케이션의 데이터 저장경로로 마운트해서 실행해 보겠습니다.
`docker run --detach --publish 3000:3000 --volume todo-db:/etc/todos --name my-todo-manager rogallo/101-todo-app:1.0.0`{{copy}}  
여러분은 여러분의 이미지를 도커허브에서 받아와서 실행해보세요 ( rogallo -> 여러분의 계정 )

`--volume todo-db:/etc/todos`에서 콜론(:)앞은 volume의 이름을, 뒤는 마운트할 컨테이너의 경로를 적어줍니다.

`Display 3000`탭을 클릭해서 애플리케이션으로 접속하고 오늘 할 일을 몇 개 적어볼까요?

그리고, 컨테이너를 삭제합니다.
`docker rm --force my-todo-manager`{{execute}}
> 컨테이너는 생성할때 --name 옵션으로 이름을 정하면, 이후에 이 이름을 이용할 수 있습니다.

이전 같으면 방금 저장한 할 일이 모두 사라지고 없겠죠?

이제 다시한번 같은 명령어로 실행해 볼까요?
`docker run --detach --publish 3000:3000 --volume todo-db:/etc/todos --name my-todo-manager rogallo/101-todo-app:1.0.0`{{copy}}  


어떤가요?
오늘 할 일 목록이 그대로 남아있나요?

이제, 좀 제대로 된 애플리케이션이 된 것 같네요.... (ง˙∇˙)ว
