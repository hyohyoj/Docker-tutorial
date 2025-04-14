# Docker-tutorial

## Docker Build
```
docker build -t <컨테이너명> -f Dockerfile .
```

> push & pull
```
docker push <이미지 이름>
docker pull <이미지 이름>
```

> 개별 실행
```
docker run -d -p 443:8080 -e TZ=Asia/Seoul --restart unless-stopped --name <컨테이너명> <이미지 이름>
```
- -d : 백그라운드 실행
- -p : 포트 설정
- -e : 환경변수 설정 (TZ: timezone)  
- -restart : 재시작 옵션 (no, always, unless-stopped, on-failure[:N])  

> 실행 컨테이너 확인
```
docker ps
```

> 컨테이너 실시간 로그 확인
```
docker logs -f <컨테이너명>
```
컨테이너 명 / 이미지 명 / 컨테이너 ID

> 이미지 목록 확인
```
docker images
```

> 컨테이너 종료
```
docker stop <컨테이너_ID> 또는 <컨테이너명>
```

> 종료된 컨테이너 삭제
```
docker rm $(docker ps --filter status=exited -q)
```

> 미사용 이미지 삭제
```
# 전체 삭제
docker image prune -a
# 개별 삭제
docker rmi <이미지_ID> 또는 <이미지_이름>:<태그>
```


## Docker Compose Build
```
# 전체 컨테이너 
docker compose build
# 개별 컨테이너
docker compose build <서비스명>
```

> push & pull
```
docker compose push
docker compose pull
```
개별 실행 시 서비스명 지정  

> 실행
```
# 전체 컨테이너 
docker compose up -d 
# 개별 컨테이너
docker compose up -d <서비스명>
```


## Docker Swarm
```
docker swarm init --advertise-addr <HOST_IP>
```
manager 노드가 될 서버에서 swarm init

> worker 노드 서버에서 swarm join
```
docker swarm join --token <스웜_토큰> <HOST_IP>:2377
```

> swarm 통신에 필요한 포트 방화벽 오픈
```
sudo ufw allow 2377/tcp
sudo ufw allow 7946/tcp
sudo ufw allow 7946/udp
sudo ufw allow 4789/udp
sudo ufw reload
```
모든 노드에 적용

> 네트워크 생성
```
docker network create --driver overlay --attachable <네트워크명>
```
- --attachable : 컨테이너를 docker run으로 수동 실행할 때도 이 네트워크에 붙일 수 있게 해줌
- bridge : 컨테이너들이 같은 Docker 호스트(=같은 서버) 내에서 통신할 때 사용
  - ex) Docker로 간단한 웹서버와 DB를 하나의 서버에서 돌릴 때 사용
- overlay : 다중 호스트(서버) 간 통신 가능. 서로 다른 물리적인 서버에 있는 컨테이너들이 같은 네트워크에 있는 것처럼 통신 가능
  - ex) 웹서버는 서버 A, DB는 서버 B에 있는데 둘이 통신하게 만들고 싶을 때

> 개별 실행
```
docker service create --name <서비스명> --network <네트워크명> <이미지명>
```

> stack deploy
```
docker stack deploy -c <yaml-file> <스택명>
```
- docker stack : 여러 개의 서비스로 구성된 어플리케이션을 관리하기 위한 도구
- docker-compose.yml 파일에 네트워크 및 배포 상세 설정을 정의할 수 있음   

```
docker stack ls                 # 전체 스택 목록
docker stack ps <스택명>        # 각 서비스의 실제 컨테이너 상태
docker stack rm <스택명>        # 스택 제거
```

> 서비스 단위로 재시작
```
docker service update --force <서비스명>
docker service update --image <이미지명:이미지_버전> <서비스명>
```

> docker secret으로 환경변수 설정
```
# 생성
docker secret create <시크릿명> <환경변수_파일명>
# 삭제
docker secret rm <시크릿명>
```
- secret는 매니저 노드 간에 암호화된 상태로 저장된다. 이런 secret 파일은 컨테이너에 배포된 뒤에도 파일 시스템이 아닌 메모리에 저장되서 컨테이너가 삭제될 때 secret도 삭제된다.
- 컨테이너에 마운트 된 시크릿 데이터는 컨테이너 내부에서 /run/secrets/<시크릿명> 경로를 통해 접근할 수 있다.

## Etc
> exec
```
docker exec -it <컨테이너_ID> sh
```
sh 혹은 /bin/bash 터미널 접근
