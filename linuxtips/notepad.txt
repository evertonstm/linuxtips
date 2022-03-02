### Fundamentos

$ docker container run -ti {ubuntu}
  - Para sair do container sem parar o entrepoint que no caso ubuntu o entrepoint e o bash
  - ctrl + p + q

$ docker container attach {ID imagen or name}
  - Para voltar para container novamente 
  
$docker container run -it nginx
  - o entrepoint do nginx e o proprio processo

$ docker container run -d nginx
  rodar o container como dimon 

$ docker container attach 17ee80775751
  - tbm vai rodar no entrepoint

$ docker container exec -ti 17ee80775751 ls
    bin   docker-entrypoint.d   home   media  proc	sbin  tmp
    boot  docker-entrypoint.sh  lib    mnt	  root	srv   usr
    dev   etc		    lib64  opt	  run	sys   var

$ docker container exec -ti 17ee80775751 ls /usr/share/nginx/
    html

$ docker container exec -ti 17ee80775751 ls /usr/share/nginx/html
    50x.html  index.html
    50x.html  index.html
    
$ docker container exec -ti 17ee80775751 bash
   - acessar o container 

$ docker container stop {container}

$ docker container start {container}

$ docker container restart {container}

$ docker container inspect {container}

$ docker container pause {container}  

$ docker container unpause {container}


$ docker container logs -f {container}

$ docker container run -d nginx 

$ docker container stats {container}
  - Mostra tudo que o container esta usando de recurso 

$ docker container top {container}
  - 
$ docker container run -d -m 128M nginx
  - criar container com limite de 128M de memoria

$ docker container run -d -m 128M --cpus 0.5 nginx
  - cria container com 128 de memoria e 1/2 CPU

$ docker container inspect {container}

$ docker container update --cpus 0.2 {container}
 - update atualiza um contaner ligado nesse caso 20% de 1 Core

$ docker container update --cpus 0.8% {container} 
 - update atualiza um contaner ligado nesse caso 80% de 1 Core

$ docker container update --cpus 0.5 --memory 64M b76e4
 - limitando em 50% de 1 core e 64M de memoria

$ docker container rm -f {container}
  - remove um container, -f força a remoção de container ligado

$ docker image ls
  - Mostrar as imagens local


#####DockerFile

criar um arquivo dentro de um diretorio nome dockerfile
  FROM debian

  LABEL app="Giropops"
  ENV EVERTON="LINDO"

  RUN apt-get update && apt-get install -y stress && apt-get clean

  CMD stress --cpu 1 --vm-bytes 64M --vm 1

(Criando a IMG)
$ docker image build -t toskeira:1.0 .   
      Sending build context to Docker daemon  2.048kB
      Step 1/5 : FROM debian
      latest: Pulling from library/debian
      e4d61adff207: Pull complete 
      Digest: sha256:10b622c6cf6daa0a295be74c0e412ed20e10f91ae4c6f3ce6ff0c9c04f77cbf6
      Status: Downloaded newer image for debian:latest
      ---> d40157244907
      Step 2/5 : LABEL app="Giropops"
      ---> Running in 605d4ea0015e
      Removing intermediate container 605d4ea0015e
      ---> 706a54f9953e
      Step 3/5 : ENV EVERTON="LINDO"
      ---> Running in ebb5a69839b5
      Removing intermediate container ebb5a69839b5
      ---> d34ee911e464
      Step 4/5 : RUN apt-get update && apt-get install -y stress && apt-get clean
      ---> Running in 214415ebd20e
      Get:1 http://security.debian.org/debian-security bullseye-security InRelease [44.1 kB]
      Get:2 http://deb.debian.org/debian bullseye InRelease [116 kB]
      Get:3 http://security.debian.org/debian-security bullseye-security/main amd64 Packages [121 kB]
      Get:4 http://deb.debian.org/debian bullseye-updates InRelease [39.4 kB]
      Get:5 http://deb.debian.org/debian bullseye/main amd64 Packages [8183 kB]
      Get:6 http://deb.debian.org/debian bullseye-updates/main amd64 Packages [2596 B]
      Fetched 8506 kB in 3s (3223 kB/s)
      Reading package lists...
      Reading package lists...
      Building dependency tree...
      Reading state information...
      The following NEW packages will be installed:
        stress
      0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
      Need to get 22.0 kB of archives.
      After this operation, 55.3 kB of additional disk space will be used.
      Get:1 http://deb.debian.org/debian bullseye/main amd64 stress amd64 1.0.4-7 [22.0 kB]
      debconf: delaying package configuration, since apt-utils is not installed
      Fetched 22.0 kB in 0s (97.2 kB/s)
      Selecting previously unselected package stress.
      (Reading database ... 6653 files and directories currently installed.)
      Preparing to unpack .../stress_1.0.4-7_amd64.deb ...
      Unpacking stress (1.0.4-7) ...
      Setting up stress (1.0.4-7) ...
      Removing intermediate container 214415ebd20e
      ---> ba332ce632d6
      Step 5/5 : CMD stress --cpu 1 --vm-bytes 64M --vm 1
      ---> Running in e2ddddb879fe
      Removing intermediate container e2ddddb879fe
      ---> 0fcffdebec90
      Successfully built 0fcffdebec90
      Successfully tagged toskeira:1.0


$ docker image ls

$ docker container run -d toskeira:1.0

$ docker container ls

$ docker container logs -f {ID}
    nesse caso o logs mostro o stress comando executado pelo CMD no dockerfile

$ docker container stats {ID}