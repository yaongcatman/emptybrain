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
 - **폴더**:
   mkdir -p ~/cat-study/app && cd ~/cat-study/app
   echo '<html><body style="background:pink; text-align:center;">
<h1>😽 앙큼한 고양이 아저씨의 진짜 최종 비밀 서버 😽</h1><p>도커가 갸르릉거리며 잘 돌아가고 있다냥.우울과 야옹은 한끗차이 야-옹</p></body></html>' > index.html
printf "FROM nginx:alpine\nCOPY ./app/index.html /usr/share/nginx/html/index.html\nEXPOSE 80" > Dockerfile

##
### 실행
 - **명령어**: docker build -t angkom-cat:1.0 .
 - **출 력**:
 [+] Building 28.1s (7/7) FINISHED                               docker:orbstack
 => [internal] load build definition from Dockerfile                       0.0s
 => => transferring dockerfile: 119B                                       0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine            3.2s
 => [internal] load .dockerignore                                          0.0s
 => => transferring context: 2B                                            0.0s
 => [internal] load build context                                          0.0s
 => => transferring context: 325B                                          0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:582c496ccf79d8aa6f8  24.7s
 => => resolve docker.io/library/nginx:alpine@sha256:582c496ccf79d8aa6f82  0.0s
 => => sha256:582c496ccf79d8aa6f8203a79d32aaf7ffd8b1336 10.33kB / 10.33kB  0.0s
 => => sha256:f4b8d628b9ede61753c6936bb998c8b14a51a6c7941 2.50kB / 2.50kB  0.0s
 => => sha256:e951149bd236796617ff2989c86839ebf37a90f5c2bdaf4 629B / 629B  0.9s
 => => sha256:cb3fe4a86f7647b754c92bd801b0fb36571ae56bd 12.34kB / 12.34kB  0.0s
 => => sha256:d8ad8cd72600f46cc068e16c39046ebc76526e41051 4.20MB / 4.20MB  8.9s
 => => sha256:d00d1920ebfbb5a7394d3350a8269ddd68bffa53de6 1.89MB / 1.89MB  4.1s
 => => sha256:bd72d396bf040006366cd28298e7b8a5b77b93b99288db5 956B / 956B  1.5s
 => => sha256:b35c5a7eacad97265ca4d4fdb9022888281f22b1b7ef337 403B / 403B  2.1s
 => => sha256:31f48cb0d7750fc5ba9cca2ddf614408aecb4ab666d 1.21kB / 1.21kB  2.7s
 => => sha256:c2b302928bf4afffdc98a4dc1747f7920b0252e85e1 1.40kB / 1.40kB  3.3s
 => => sha256:97577b80aa169f5ffee53dd2d8cedc40dff9f6be 19.72MB / 19.72MB  24.3s
 => => extracting sha256:d8ad8cd72600f46cc068e16c39046ebc76526e41051f43a8  0.1s
 => => extracting sha256:d00d1920ebfbb5a7394d3350a8269ddd68bffa53de6d9553  0.0s
 => => extracting sha256:e951149bd236796617ff2989c86839ebf37a90f5c2bdaf4e  0.0s
 => => extracting sha256:bd72d396bf040006366cd28298e7b8a5b77b93b99288db5d  0.0s
 => => extracting sha256:b35c5a7eacad97265ca4d4fdb9022888281f22b1b7ef337c  0.0s
 => => extracting sha256:31f48cb0d7750fc5ba9cca2ddf614408aecb4ab666d70c68  0.0s
 => => extracting sha256:c2b302928bf4afffdc98a4dc1747f7920b0252e85e1cdc37  0.0s
 => => extracting sha256:97577b80aa169f5ffee53dd2d8cedc40dff9f6bef6ded9d7  0.2s
 => [2/2] COPY ./app/index.html /usr/share/nginx/html/index.html           0.1s
 => exporting to image                                                     0.0s
 => => exporting layers                                                    0.0s
 => => writing image sha256:0ace6e106e81e2940ef763b5a505db829aca80011c63f  0.0s
 => => naming to docker.io/library/angkom-cat:1.0                          0.0s
##
 - **서버실행**: docker run -d -p 8080:80 --name angkom-server angkom-cat:1.0
- **출 력**: 165a6ed569b020bb386c00a8d235e4fe4756942405ffa6de71849480734aadba
##







