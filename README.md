# emptybrain
## 1. Docker
### 설치
##
- **명령어**: docker --version
- **출 력**: Docker version 28.5.2, build ecc6942
##
- **명령어**: docker info
- **출 력**: Client:
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
##
- **명령어**: docker run --name hello-test hello-world
- **출 력**: Unable to find image 'hello-world:latest' locally
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
##
  ### 운영 명령 실행
##
  - **명령어**: docker images
  - **출 력**: zsh: no such file or directory: /docker
##
  - **명령어**: docker ps -a
  - **출 력**: CONTAINER ID   IMAGE         COMMAND    CREATED              STATUS                          PORTS     NAMES
52ad68bce410   hello-world   "/hello"   About a minute ago   Exited (0) About a minute ago             hello-test
##
- **명령어**: docker logs hello-test
- **출 력**: Hello from Docker!
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
##
  - **명령어**: docker stats --no-stream
  - **출 력**: CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS
##2. Dockerfile
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
##
### 실행
 - **명령어**: docker build -t angkom-cat:2.0 .
 - **출 력**:
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
##
 - **서버실행**: docker run -d -p 8080:80 --name angkom-server angkom-cat:2.0
- **출 력**: 15fa0dc8e606025ec7e264f92109e72d96195b0ac2c2b1c6a7284cbdf982bef7
##
 - **결과명령어**: docker ps 
 - **결과**: CONTAINER ID   IMAGE            COMMAND                   CREATED         STATUS         PORTS                                     NAMES
15fa0dc8e606   angkom-cat:2.0   "/docker-entrypoint.…"   5 minutes ago   Up 5 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   angkom-server
##
- **cat Dockerfile**: FROM nginx:alpine
COPY ./app/index.html /usr/share/nginx/html/index.html
EXPOSE 80%                                                                      






