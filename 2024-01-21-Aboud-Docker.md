# 도커 다운로드 
도커를 다운로드 받은 후 도커가 잘 다운로드 되었는지 확인하기 위해서 확인!
```
# 도커 버전 확인
>> docker -v
```

[❗️] 도커를 다운로드 받은 후 아래와 같은 메시지가 출력 된다면 도커를 실행 가능하도록 바꾸어 주어야 한다.
 
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

```
# 맥북의 경우, 도커를 실행하는 방법
>> docker run -d -p 80:80 docker/getting-started
```

# **도커 엔진에서 사용하는 기본 단위: 이미지와 컨테이너**

## 도커 이미지
> 이미지는 컨테이너를 생성할 때 필요한 요소

> 이미지는 여러 개의 계층으로 된 바이너리 파일로 존재

> 컨테이너를 생성하고 실행할 때 읽기 전용으로 사용됨

> {저장소 이름}/{이미지 이름}:{태그}


```
# 도커 이미지 검색
>> docker search {이미지명}:{태그}

# 도커 이미지 다운로드
>> docker pull {이미지명}:{태그}

# 로컬에 다운로드 된 도커 이미지를 확인
>> docker images

# 도커 이미지 삭제
>> docker rmi {옵션} {이미지 id}

# 이미지를 사용하여 컨테이너 생성하기
>> docker create {옵션} {이미지명}:{태그}
>> docker create -it python
>> docker create -it ubuntu

# cetos:7 이미지로 mycentos라는 이름의 컨테이너 생성하기
>> docker create --name  mycentos centos:7

# 도커 이미지 생성하기
# 도커 파일이 있는 디렉토리 기준이 상대주소
>> docker bulid -t {이미지명}


```

## 도커 컨테이너
> 이미지로 컨테이너를 생성하면 해당 이미지의 목적에 맞는 파일이 들어있는 파일 시스템과 격리된 시스템 자원 및 네트워크를 사용할 수 있는 독립된 공간이 생성된다.

> 컨테이너는 이미지를 읽기 전용으로 사용하되, 이미지에서 변경된 사항만 컨테이너 계층에 저장하므로 컨테이너에서 무엇을 하든지 원래 이미지는 영향을 받지 않음.

```
# 만들어진 컨테이너 실행하기
>> docker start {컨테이너이름}

# 컨테이너로 접속하기
>> docker attach {컨테이너 이름}

# 도커 실행하고 접속하기(이미지가 없을 경우 다운로드까지)
>> docker run {이미지명}:{태그}
>> docker run —name {컨테이너명} -v ${pwd}:/{경로} -p {포트번호}:{포트번호} {이미지명}

# 사용중인 도커 컨테이너 보기
>> docker ps

# 모든 도커 컨테이너 보기
>> docker ps -a 

# 도커 컨테이너 이름 변경하기
>> docker rename {변경 전 컨테이너명} {변경 할 컨테이너명}

# 동작 중인 컨테이너 재시작
>> docker restart

# 도커 컨테이너를 '종료 후' 빠져나오기
>> exit
>> Ctrl + D

# 도커 컨테이너를 '종료하지 않고' 빠져나오기
>> Ctrl + P, Q

# 모든 컨테이너 중지
>> docker stop $(docker ps -aq)

# 도커 컨테이너 삭제
>> docker rm {컨테이너명}

# 실행 중인 도커 컨테이너 삭제
>> docker rm -f {컨테이너명}

# 모든 컨테이너 삭제
>> docker container prune
```

## 도커 명령어 run, create 차이
> docker run = docker pull, docker create, docker start, docker attach

> docker create = docker pull, docker create 

##  그 밖의 도커 명령어 모음

```
# 도커 엔진의 정보 출력
>> docker info

# 호스트와 바인딩된 포트 확인
>> docker port {컨테이너명}

# 사용되지 않는 모든 도커 요소 삭제(이미지, 컨테이너, 네트워크, 볼륨)
>> docker system prune -a

# 도커 옵션보기
-d 백그라운드 실행
-it 컨테이너 접속시 bash로 CLI 입출력 사용
-p 호스트와 컨테이너 포트 연결
-v 호스트와 컨테이너 디렉토리 연결
```
