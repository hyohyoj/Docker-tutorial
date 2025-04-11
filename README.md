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
