# emptybrain

# AI/SW 개발워크스테이션 

## ㄱ. 프로젝트 개요
ㄱ. **cli / docker / git/github 을 활용한 실행 / 배포 환경 구축**  
ㄴ. **컨테이너 활용 동일한 서버환경 재현**  
ㄷ. **포드매핑의 실행과 검증을 통한 구조적 설계에 대한 이해**  

##
## ㄴ. 실행환경
```bash
os/shell : macOS (Apple Silicon) / zsh
Container Runtime : OrbStack v28.5.2
Git 버전 : Git 2.x
도구 : Terminal (v-code 없이)
```

## ㄷ. 수행체크리스트

-**터미널 기본 조작**  
-**권한 변경 실습**  
-**DOCKER 설치 / 점검**  
-**헬로우월드 실행**    
-**DOCKERFILE 빌드 / 실행**  
-**포트 매핑 접속**  
-**바인드 마운트 반영**  
-**볼륨 영속성**  
-**GIT 설정 / github연동**  

##
# 터미널
## 0. 터미널 기본조작
터미널의 역할 : 컴퓨터에게 효율적으로 내리는 명령
```bash
`pwd` : Print Working Directory / 현재 위치 확인
`ls` : List / 현재 폴더에 있는 파일 목록  
`cd` : Change Directory / 다른 폴더로 이동  
`mkdir` : Make Directory / 새 폴더 만들기  
`touch` : 비어있는 새 파일 만들기  
`rm` : Remove / 파일이나 폴더 삭제  
`cp` : Copy / 복사하기    
`mv` : Move / 이동하기 (이름바꾸기)  
`chmod` : Change Mode / 파일의 권한(읽기/쓰기/실행) 변경
```
## 0. 터미널 조작 로그 

1. 현재위치확인
```bash
위치: pwd
/Users/loo_cozy9531/cat-study
```
##
2. 테스트 폴더 생성 / 이동 / 빈파일생성 - 파일목록 확인
```bash
*테스트용 폴더 생성: mkdir -p practice/test_folder
*폴더이동: cd practice
*빈파일생성: touch hello_terminal.txt
*파일목록확인(숨김파일포함): ls -la
total 0
drwxr-xr-x  4 loo_cozy9531  loo_cozy9531  128  4 15 22:05 .
drwxr-xr-x  5 loo_cozy9531  loo_cozy9531  160  4 15 22:03 ..
-rw-r--r--  1 loo_cozy9531  loo_cozy9531    0  4 15 22:05 hello_terminal.txt
drwxr-xr-x  2 loo_cozy9531  loo_cozy9531   64  4 15 22:03 test_folder
```
테스트용 폴더 생성 `mkdir -p` 옵션으로  (폴더가 없는경우) 현재 위치에 practice 폴더를 만든 후 그 안에 test_folder를 생성, `touch` 로 빈파일 생성, `li -la`로 숨김파일포함 loo_cozy9531 권한자의 4개 리스트 발견.  
`.`현재폴더(practice) / `..`상위폴더 / `hello_terminal.txt` 일반 빈 파일 / `test_folder` 디렉토리  
##
3. 파일이름 변경/이동/복사
```bash
*파일이름 변경: mv hello_terminal.txt renamed_file.txt
*변경한파일복사: cp renamed_file.txt copy_file.txt
*파일 및 폴더 삭제: rm copy_file.txt
cd ..
rm -rf practice
삭제확인: ls delete_test
ls: delete_test: No such file or directory
```
hello_terminal.txt 이름변경 -> renamed_file.txt 후 복사본 생성 후 뒤로가기로 practice로 이동해 작업폴더, 내부파일을 전부 삭제. 존재하지않는 경로 조회 후 텅 빈 리스트 확인.
##
# 권한변경
## 1. 권환 확인 / 변경
```dash
 *권한확인: ls -l app/index.html
 -rw-r--r--  1 loo_cozy9531  loo_cozy9531  892  4 15 21:23 app/index.html

*권한변경:  chmod 755 app/index.html
ls -l app/index.html

*결과: -rwxr-xr-x  1 loo_cozy9531  loo_cozy9531  892  4 15 21:23 app/index.html
```
소유자 / 그룹 / 나머지 -> 읽기 / 쓰기만 가능한 일반 파일    
변경후  chmod 755 (4+2+1)(4+1)(4+1)  
소유자 / 그룹 / 나무지 -> 소유자는 읽기/쓰기/실행 가능 , 그룹 및 나머지는 읽기/실행만 가능 수정 불가능.
##
# Docker
## 1. 도커 설치 및 기본점검
1. 버전확인
```bash
*버전확인: docker --version
Docker version 28.5.2, build ecc6942
```
현재 시스템 도커 엔진 28.5.2 버전이 성공적으로 서치되어 작동.
##
2. 도커 정보
```bash
*정보: docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/ang.com/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/ang.com/.docker/cli-plugins/docker-compose
```
orbstack 컨텍스트를 기반으로 실행.   
컨테이너 빌드를 위한 Buildx(v0.29.1) / Compose(v2.40.3) 준비되어있음.  
ang.com 컴퓨터 경로 설치.
##
3. 도커 작동 확인
```bash
*실행: docker run --name hello-test hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
58dee6a49ef1: Pull complete 
Digest: sha256:452a468a4bf985040037cb6d5392410206e47db9bf5b7278d281f94d1c2d0931
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
로컬에 없는 hello-world 이미지를 클라우드에서 자동으로 내려받아(Pull), hello-test라는 이름의 컨테이너로  
실행하여 도커의 정상 작동을 확인.

##
  ## 2. 운영 명령 실행
1. 이미지
```bash
*이미지 다운로드/목록확인: docker images
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
angkom-cat    2.0       469aa3b1024f   35 minutes ago   61.6MB
angkom-cat    1.0       0ace6e106e81   2 hours ago      61.6MB
nginx         alpine    cb3fe4a86f76   7 days ago       61.6MB
hello-world   latest    eb84fdc6f2a3   3 weeks ago      5.2kB
ang.com@hwang-angkom-ui-MacBookPro cat-study % \
> docker images
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
angkom-cat    2.0       469aa3b1024f   36 minutes ago   61.6MB
angkom-cat    1.0       0ace6e106e81   2 hours ago      61.6MB
nginx         alpine    cb3fe4a86f76   7 days ago       61.6MB
hello-world   latest    eb84fdc6f2a3   3 weeks ago      5.2kB
ang.com@hwang-angkom-ui-MacBookPro cat-study % 
```
레포지토리에 총 4개의 이미지 존재 . hello-world가 가장 최신이미지 
##
2.  도커 작동 컨테이너 출력
```bash
*작동컨테이너: docker ps -a
 CONTAINER ID   IMAGE         COMMAND    CREATED              STATUS                          PORTS     NAMES
52ad68bce410   hello-world   "/hello"   About a minute ago   Exited (0) About a minute ago             hello-test
```
도커에서 어떤 컨테이너가 작동 하고 있는지 (전부) 명령 ->  
hello-world 이미지로 hello-test라는 이름의 컨테이너를 만들고 그 안의 프로그램을 실행.  
hello-world는 "Hello from Docker!"라는 문구만 출력하고 바로 종료되도록 설계된 프로그램.  
Exited (0) -> 에러 없이 마무리하고 종료.
##
3. 도커 모든 텍스트 출력 기록
```bash
*hello-test 도커기록: docker logs hello-test

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
사용자의 명령이 도커 엔진에 전달 -> 내 컴퓨터에 없는 이미지를 도커허브(인터넷)에서 가져옴 ->  
이미지를 실행 가능한 컨테이너로 변신 -> 컨테이너 안에서 나온 글자를 사용자의 터미널로 출력  
추후 에러(Exited (1))시 로그(텍스트기록) 를 보고 원인을 찾을 수도 있음  
##
4. 도커 상태 확인
```bash
실시간 모니터링 작업 관리: docker stats --no-stream
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS
```
도커의 현재 상태만 보여주고 종료 -> 현재 시제로 가동 중인 컨테이너가 없는 상태.
## 3. 컨테이너 실행 실습
1. Ubuntu 실행
```bash
*실행: docker run -it ubuntu bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
689b91d88a0f: Pull complete 
Digest: sha256:84e77dee7d1bc93fb029a45e3c6cb9d8aa4831ccfcc7103d36e876938d28895b
Status: Downloaded newer image for ubuntu:latest
```
우분투 리눅스 운영체제 진입. 우분투 안에서 명령어를 주고받을 수 있는 통로 오픈.
##
2. 우분투 내부 파일 목록
```bash
*리눅스폴더: root@72a0cb392452:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
```
`bin` / `sbin` 컴퓨터 실행시 가장 기본적인 명령어 도구    
`etc` 시스템 각종 설정파일 모음  
`root` 시스템 최고 관리자의 개인폴더  
`home` 일반 사용자들의 개인폴더  
`tmp` 임시 파일 저장 및 삭제 (연습장)
##
3. 메아리
```bash
다시 보여주기 기능: root@001b56f9fffb:/# echo meow        
meow
```
야옹을 외쳤더니 야옹을 했다.
##
4. 컨테이너 유지
```bash
유지하며 나오기 : `ctrl + p + Q`
*유지확인: docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
001b56f9fffb   ubuntu    "bash"    8 minutes ago    Up 8 minutes              laughing_diffie
5f3d76d6719b   ubuntu    "bash"    15 minutes ago   Up 15 minutes             serene_cerf
72a0cb392452   ubuntu    "bash"    16 minutes ago   Up 16 minutes             infallible_williamson
loo_cozy9531@c5r3s7 ~ % 
```
잠시 나왔지만 컨테이너들은 여전히 실행 유지
##
5. 컨테이너 `exec` 새로운 통로로 들어가서 빠져나오기
```bash
*새로운 문으로 들어와서: docker exec -it laughing_diffie bash
*원래 문으로 나가기 : root@001b56f9fffb:/# exit
*확인 : loo_cozy9531@c5r3s7 ~ % docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
001b56f9fffb   ubuntu    "bash"    25 minutes ago   Up 25 minutes             laughing_diffie
5f3d76d6719b   ubuntu    "bash"    31 minutes ago   Up 31 minutes             serene_cerf
72a0cb392452   ubuntu    "bash"    32 minutes ago   Up 32 minutes             infallible_williamson
```
`exec`로 laughing_diffie bash 컨테이너에서 다시 들어갔다가 나갔지만 실행 유지 중
##
6. 컨테이너 `attach` 다시 들어가서 빠져나오기
```bash
*직접 연결된 기존 문으로 들어가서: docker attach laughing_diffie
bash: docker: command not found
*똑같은 문으로 나가기: root@001b56f9fffb:/# exit
* laughing_diffie 존재확인: docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
5f3d76d6719b   ubuntu    "bash"    36 minutes ago   Up 36 minutes             serene_cerf
72a0cb392452   ubuntu    "bash"    37 minutes ago   Up 37 minutes             infallible_williamson

*나머지 퇴근 명령: docker stop serene_cerf infallible_williamson
*docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
`attach`으로 들어가서 빠져나왔을 때 laughing_diffie 존재 사멸 및 나머지 serene_cerf / infallible_williamson 도 도커 멈춤 명령으로 종료 -> 도커 현재상태로 유지된 컨테이너 없음 확인.
##
# Dockerfile 기반 커스텀 이미지 제작
## 1. 웹 서버 베이스 이미지 활용
html파일을 웹 화면으로 출력하기 위해서는 웹 서버 프로그램이 필요하며  
가볍고 대중적인 `nginx:alpine`  이미지 베이스 선택
```bash
 - **HTML**:
cat <<EOF > app/index.html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>고양이 아저씨의 비밀 서버</title>
</head>
<body style="background-color: pink; text-align: center; padding-top: 50px; font-family: sans-serif;">
    <h1 style="color: #333; font-size: 3em;">😽 앙큼한 고양이 아저씨의 최종 비밀 서버 😽</h1>
    <div style="background: white; display: inline-block; padding: 20px; border-radius: 20px; box-shadow: 10px 10px 0px #ffb6c1;">
        <h2 style="color: #ff69b4;">도커가 갸르릉거리며 잘 돌아가고 있다냥!</h2>
        <p style="font-size: 24px; color: #555;">
            "우울과 야옹은 한끗차이, <span style="color: #ff1493; font-weight: bold; font-size: 30px;">야옹!</span>"
        </p>
    </div>
    <p style="margin-top: 30px; color: #888;">우리 집사, 이제 진짜 도커 박사다냥! 🐾</p>
</body>
</html>
EOF
```
### [1] 기존 베이스 이미지 : `nginx:alpine` 
html파일을 웹 화면으로 출력하기 위해서는 웹 서버 프로그램이 필요하며  
가볍고 대중적인 `nginx:alpine`  이미지 베이스 선택  
### [2] 커스텀포인트
핑크색 배경 : 그 때 시점 기준 화사한 색상으로 어두운 나를 밝히는게 매우 필수.  
특수 문자와 문구 :
고양이아저씨라는 내성적이며 독특하고 음침하지만 귀여운 개발자 이미지에 적합함.  
추후 대머리 아저씨 얼굴 이모티콘을 추가하고 싶은 마음입니다.   

##
## 2. 빌드 / 실행
1. 빌드
```bash
*건축 : docker build -t angkom-cat:2.0 .
 [+] Building 1.1s (7/7) FINISHED                                docker:orbstack
 => [internal] load build definition from Dockerfile                       0.0s
 => => transferring dockerfile: 119B                                       0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine            1.0s
 => [internal] load .dockerignore                                          0.0s
 => => transferring context: 2B                                            0.0s
 => [internal] load build context                                          0.0s
 => => transferring context: 961B                                          0.0s
 => CACHED [1/2] FROM docker.io/library/nginx:alpine@sha256:582c496ccf79d  0.0s
 => [2/2] COPY ./app/index.html /usr/share/nginx/html/index.html           0.0s
 => exporting to image                                                     0.0s
 => => exporting layers                                                    0.0s
 => => writing image sha256:469aa3b1024f47c8971c4d77485905728878a882d37af  0.0s
 => => naming to docker.io/library/angkom-cat:2.0                          0.0s
```
angkom-cat:2.0 커스텀 이미지 생성 완료.  
캐시기능을 사용해 1.1초로 건축 완료.
##
2. 실행 / 확인
```bash
*서버실행 (백그라운드/포트 포워딩):  docker run -d -p 8080:80 --name angkom-server angkom-cat:2.0
15fa0dc8e606025ec7e264f92109e72d96195b0ac2c2b1c6a7284cbdf982bef7

*실행상태확인*: docker ps 
CONTAINER ID   IMAGE            COMMAND                   CREATED         STATUS         PORTS                                     NAMES
15fa0dc8e606   angkom-cat:2.0   "/docker-entrypoint.…"   5 minutes ago   Up 5 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   angkom-server
```
서버를 컨테이너가 실행되어도 터미널이 멈추지 않고 다음 명령어를 입력할 수 있는 상태에서 내 컴퓨터와 컨테이너의 문을 연결 시켜 실행.  
컨테이너 고유 ID 생성.  
8080번 포트와 컨테이너 안 80번 포트를 연결. 8080번 주소 (비밀번호) 입력시 컨테이너 안 80번 문으로 입장 가능.
##
3. cat Dockerfile 설계도 
```bash
설계도: cat Dockerfile
FROM nginx:alpine
COPY ./app/index.html /usr/share/nginx/html/index.html
EXPOSE 80
```
nginx 베이스에 커스텀 HTML을 얹고 80번 포트를 개방하도록 설계.                                              
##
4.최종결과물: http://localhost:8080/
<img width="882" height="528" alt="스크린샷 2026-04-15 오전 2 49 39" src="https://github.com/user-attachments/assets/0424b78a-a3ea-45c1-93fc-d1e982c6c0f9" />
##
5. 포트 매핑 접속
```bash
*서버접속확인: docker logs angkom-server
docker-entrypoint.sh: Configuration complete; ready for start up
2026/04/14 17:44:42 [notice] 1#1: using the "epoll" event method
2026/04/14 17:44:42 [notice] 1#1: nginx/1.29.8
2026/04/14 17:44:42 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0) 
2026/04/14 17:44:42 [notice] 1#1: OS: Linux 6.17.8-orbstack-00308-g8f9c941121b1
2026/04/14 17:44:42 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 20480:1048576
2026/04/14 17:44:42 [notice] 1#1: start worker processes
2026/04/14 17:44:42 [notice] 1#1: start worker process 30
2026/04/14 17:44:42 [notice] 1#1: start worker process 31
2026/04/14 17:44:42 [notice] 1#1: start worker process 32
2026/04/14 17:44:42 [notice] 1#1: start worker process 33
2026/04/14 17:44:42 [notice] 1#1: start worker process 34
2026/04/14 17:44:42 [notice] 1#1: start worker process 35
2026/04/14 17:44:42 [notice] 1#1: start worker process 36
2026/04/14 17:44:42 [notice] 1#1: start worker process 37
2026/04/14 17:44:42 [notice] 1#1: start worker process 38
2026/04/14 17:44:42 [notice] 1#1: start worker process 39
2026/04/14 17:44:42 [notice] 1#1: start worker process 40
192.168.215.1 - - [14/Apr/2026:17:45:08 +0000] "GET / HTTP/1.1" 200 891 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36" "-"
```
웹서버가 오류없이 정상적으로 시작 -> 외부에서 서버의 대문으로 접속 요청 ->  
서버가 성공코드로 응답 "GET / HTTP/1.1" 200  -> 맥북 크롬 브라우저를 통해 페이지 출력 성공
##
# 바인드마운트 / 볼륨
## 1. 바인드마운트
1. 변경
```bash
*호스트변경: docker run -d -p 8081:80 -v $(pwd)/app:/usr/share/nginx/html --name bind-test nginx:alpine

nginx:alpine
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
d8ad8cd72600: Already exists 
d00d1920ebfb: Already exists 
e951149bd236: Already exists 
bd72d396bf04: Already exists 
b35c5a7eacad: Already exists 
31f48cb0d775: Already exists 
c2b302928bf4: Already exists 
97577b80aa16: Already exists 
Digest: sha256:582c496ccf79d8aa6f8203a79d32aaf7ffd8b13362c60a701a2f9ac64886c93d
Status: Downloaded newer image for nginx:alpine
707c15a2973a32ae7b99aeb3694c353ba55a7a6e2ea8e7cde839eb321997e56c
```
웹서버 이미지를 빌려서 bind-test 이름으로 실행 ->  
백그라운드에서 컨테이너랑 연결 8081번 문으로 내폴더에서 컨테이너 폴더로 실시간 연결.
##
2. 수정
```bash
*멘트수정: sed -i '' 's/우울과 야옹은 한끗차이/원래 코딩 시작하면 성격이 나빠지나요? 그래도 감사함미다.../g' ./app/index.html

curl http://localhost:8081 | grep "감사함미다"
 % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   941  100   941    0     0  37709      0 --:--:-- --:--:-- --:--:-- 39208
            "원래 코딩 시작하면 성격이 나빠지나요? 그래도 감사함미다..., <span style="color: #ff1493; font-weight: bold; font-size: 30px;">야옹!</span>"
```
터미널에서 수정할 내용 탐색 후 백업 파일 없이 바로 수정 및 변경 멘트로 대체  
-> 터미널에서 웹사이트에 접속해 즉각적으로 데이터를 가져와 이미지 빌드 없이 파일 수정 후 멘트 변경
##
3. 결과: http://localhost:8081
<img width="876" height="526" alt="스크린샷 2026-04-15 오전 3 33 24" src="https://github.com/user-attachments/assets/08605886-771a-4895-8f2c-18774ee305dd" />
##

## 2. 볼륨 영속성
```dash
*생성: docker volume create cat-snack-storage
cat-snack-storage

*연결 : docker run --name writer-test -v cat-snack-storage:/data alpine sh -c "echo ' 고양이 간식: 츄르 100개' > /data/list.txt"

*삭제: docker rm -f writer-test
writer-test

*검증: docker run --rm -v cat-snack-storage:/data alpine cat /data/list.txt
고양이 간식: 츄르 100개
```
[1] 도커 전용 안전 구역에 'cat-snack-storage' 데이터 보관함 생성    
[2] writer-test 볼륨 임시 컨테이너 실행해 볼륨 안에 메모 작성    
[3] writer-test 컨테이너 강제삭제  
[4] 새 컨테이너 생성해 똑같은 볼륨 연결 -> 예전 메모 그대로 활성화
##

# GIT설정 및 연동
## 1. 깃허브
```bash
*사용자정보: git config --global --list
user.name=yaongcatman
user.email=loo_cozy@naver.com
<img width="519" height="48" alt="Screenshot 2026-04-16 at 7 04 41 AM" src="https://github.com/user-attachments/assets/2ea75630-8d66-42e9-afd7-0cfba9c8b46a" />
```
https://github.com/yaongcatman/emptybrain

##
# 과제 목표

[1] 절대경로와 상대경로  
**절대경로**:
파일의 정확한 전체주소. 항상 같은 곳을 가리킨다.  
언제: (개인) 개인 컴퓨터의 시스템 설정 파일이나, 중요한 데이터베이스 위치를 정할 때
왜: 현재 터미널에서 어떤 폴더에 있든 상관없이, 항상 똑같은 파일을 찾기 위해서 (도커 볼륨)


**상대경로**: 현재 내가 있는 위치를 기준으로 한 주소이며 갈 수 있는 경로(방향)을 향한다.  
언제: (단체) 개인이 만든 프로젝트 폴더 안에 있는 파일을 서로 연결할 때  
왜: 폴더 전체를 클라우드나 원격 온라인에 공유했을 때 유연. 상대적인 위치는 변하지 않는다.  
##
[2] 파일권한과 숫자표기  
8권한을 부여하는 것
파일권한 : R(읽기) 4 / W(쓰기) 2/ X(실행) 1
7(421) 모든권한
5(41) 읽기 / 실행
4(4) 읽기

**-rwxr-xr-x (755)**
**-rw-r--r-- (644)**
- : 파일
rw- (주인): 읽기 / 쓰기
r - - (그룹): 읽기  
r - - (나머지) : 읽기
##
[3] 도커 커스텀 프로젝트 디렉토리 및 이미지 제작  
1.프로젝트디렉토리  
```bash
cat-study/              # 최상위 프로젝트 폴더 (Root)
├── app/                # 소스 코드 전용 폴더
│   └── index.html      # '앙큼한 고양이' HTML 파일
└── Dockerfile          # 이미지를 만들기 위한 설계도 (레시피)
```
-  cat-study 폴더 안에 app / Dockerfile 같은 위치 생성.   
`build` 실행시 상대경로로 오류 확률 낮음.  
-  index.html을 app 폴더 안에 보관.  
관리 편의성
##
2. 이미지 제작  
웹서버 이미지를 생성 그 안에 사용자가 커스텀한 index.html를 넣고 `build` / `run`  
나만의 전용 서버 환경을 만들어 복사해 똑같이 볼 수 있게 만드는 것.  

-  이미지: 그림 속 풍경    
1. `build` 설계한 이미지를 만드는 과정  
2. 이미지`run` 실행 -> 움직이는 컨테이너  
3. 빌드된 이미지는 수정 불가 -> 코드 수정 후 재빌드 -> 새로운 이미지 생성.  

-  컨테이너: 실제 풍경  
1. 이미 만들어진 이미지 `build` 불필요  
2. `run` 하나의 이미지로 똑같은 컨테이너 동시 생성.  
3.  컨테이너 실행 중 수정 가능 (컨테이너 삭제시 변경된 내용 소멸)
   
##
[4] 포드매핑이 필요한 이유  
컨테이너는 공유하지 않은 개인적인 독립된 공간 (외부에서 접근 불가능)  
컨테이너의 방화벽으로 외부접속차단  
포트와 호스트 네트워크는 완전 분리

서버 문짝 오픈 -> `-p 8081:80` 같은 호스트링크를 만들어 80번으로 갈 수 있는 외부와 내부 중간의 연결 통로 생성.   
->`localhost:8081` 브라우저에 입력한 신호 수락 및 전달  
-> 설계한 건축물 네트워크에 실현가능
-> 호스트의 포트번호를 다른 번호로 여러개 지정시 겹치지 않고 운영가능. 

-  만약 호스트 포트가 이미 사용 중이라 포트 매핑이 실패한다면?
1. `docker ps` 으로 현재 해당 포트를 실행 중인 컨테이너 확인  
2. `docker stop (컨테이너이름)`으로 멈춤  
3. 1/2 단계 실행시 발견 못할시 `lsof -i : 해당포트번호` 로 다른 시스템이 포트 점유하고 있는지 진단.  
4. `kill -9 [아까 찾은 PID번호]`로 죽이기.

##
[5] 영속데이터  
컨테이너가 아닌 기존 컴퓨터에 데이터베이스 비밀방을 생성. 그 안에 데이터 보존.    
내 방(컨테이너)을 통째로 부셔도 내 방안의 물건들이 그대로 유지되어 있는 것.  
어떻게?  
새로운 컨테이너 생성시 비밀 데이터베이스 연결 -> 이전 방의 물건들이 그대로 유지 보존.  
컨테이너는 지워져도 데이터는 보존되는 것.  

**EX**  
```bash
명령어 `nano docker-compose.yml`  
cat <<EOF > docker-compose.yml
services:
  my-cat:
    image: my-custom-image
    ports:
      - "8081:80"
    volumes:
      - cat-data:/usr/share/nginx/html

volumes:
  cat-data:
EOF
```
저장 (ctrl+O / Enter) -> 나가기 (ctrl+X) -> 실행 `docker-compose up -d` 

##
[6] GIT과 GITHUB  
깃 : (로컬) 소프트웨어 / 개인적인 저장공간 
깃허브 : (원격/클라우드) 웹사이트 / 저장한 것을 공유하는 공간  

##
[7] 문제해결방법  
**문제발생**:
Dockerfile 빌드 후 컨테이너를 실행  
-> 브라우저에서 접속했을 때 '사이트에 연결할 수 없음' 에러가 발생.
**가설**:   
컨테이너는 잘 돌아가고 있지만, 내 컴퓨터의 포트와 컨테이너의 포트가 연결되지 않았을 것이다. (포트 매핑 문제)  
**확인**:  
docker ps 명령어를 입력해 포트 설정을 확인->  
컨테이너 내부 포트(80)만 표시되고 호스트 포트(8081)와 연결된 표시(0.0.0.0:8081->80/tcp)가 없는 것을 발견  
**조치**:  
기존 컨테이너를 정지(docker stop) 및 삭제(docker rm)한 후, 포트 매핑 다시 실행->   
`docker run -d -p 8081:80 --name my-cat-server my-custom-image` ->  
localhost:8081 접속 시 메인 화면이 정상적으로 출력되는 것을 확인.






















