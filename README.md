# **Fundamentos docker** 
  * Para sair do container sem parar o entrepoint que no caso ubuntu o entrepoint e o bash
  * *ctrl + p + q*
```
$ docker container run -ti {ubuntu}
```
###  Para voltar para container novamente.
```
$ docker container attach {ID image or name}
```

### O entrepoint do nginx e o proprio processo.
```
$docker container run -it nginx
```
 
### Rodar o container como dimon.
```
$ docker container run -d nginx
```

### Tbm vai rodar no entrepoint.
```
$ docker container attach {ID Container}
```
```
$ docker container exec -ti {ID Container} ls
bin   docker-entrypoint.d   home   media  proc	sbin  tmp
boot  docker-entrypoint.sh  lib    mnt	  root	srv   usr
dev   etc		    lib64  opt	  run	sys   var
```
```
$ Docker container exec -ti {ID Container} ls /usr/share/nginx/
html
```

```
$ Docker container exec -ti {ID Container} ls /usr/share/nginx/html
    50x.html  index.html
    50x.html  index.html
```    
#
### Acessar o container.
```
$ docker container exec -ti {ID Container} bash
```
```
$ docker container stop {container}
```
```
$ docker container start {container}
```
```
$ docker container restart {container}
```
```
$ docker container inspect {container}
```
```
$ docker container pause {container}  
```
```
$ docker container unpause {container}
```
```
$ docker container logs -f {container}
```
```
$ docker container run -d nginx 
```
### Mostrar todps os recursos que o container esta usando.
```
$ docker container stats {container}
```
```
$ docker container top {container}
```
### Criar container com limite de 128M de memória.
```
$ docker container run -d -m 128M nginx
```
### Criar container com 128 de memória e 1/2 CPU.
```
$ docker container run -d -m 128M --cpus 0.5 nginx
```
```
$ docker container inspect {container}
```
### Update atualiza um contaner ligado nesse caso 20% de 1 Core.
```
$ docker container update --cpus 0.2 {container}
```
### Update atualiza um contaner ligado nesse caso 80% de 1 Core.
```
$ docker container update --cpus 0.8% {container} 
```
### Limitando em 50% de 1 core e 64M de memoria.
```
$ docker container update --cpus 0.5 --memory 64M b76e4
```
### Remove um container, -f força a remoção de container ligado.
```
$ Docker container rm -f {container}
```
### Removendo Container.
```
docker container rmr -f $(docker ps -q)
```

### Mostrar as imagens local.
```
$ docker image ls
```
 #
# **DockerFile**

### Criar um arquivo dentro de um diretorio nome dockerfile.
```
  FROM debian

  LABEL app="Giropops"
  ENV EVERTON="LINDO"

  RUN apt-get update && apt-get install -y stress && apt-get clean

  CMD stress --cpu 1 --vm-bytes 64M --vm 1
```
### (Criando a IMG).
```
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
```
```
$ docker image ls
```
```
$ docker container run -d toskeira:1.0
```
```
$ docker container ls
```
### Nesse caso o logs mostro o stress comando executado pelo CMD no dockerfile.
```
$ docker container logs -f {ID}
```

```
$ docker container stats {ID}
```
#
# **Dia 2: (VOLUMES)**

### Vamos criar um diretorio dentro do /opt.
```
$ sudo mkdir /opt/giropops
```
#### Volume do tipo Bind.

#### Vai montar no diretorio /opt/giropops com destino ao diretorio na raiz do sistema do container /giropops.
```
$ docker container run -it --mount type=bind,src=/opt/giropops,dst=/giropops debian

root@916d2c869731:/# ls
bin   dev  giropops  lib    media  opt	 root  sbin  sys  usr
boot  etc  home      lib64  mnt    proc  run   srv   tmp  var

root@916d2c869731:/# cd giropops/

root@916d2c869731:/giropops# echo > teste.txt

root@916d2c869731:/giropops# ls
teste.txt

root@916d2c869731:/giropops# exit
```
```
$ /opt/giropops$ ls
teste.txt
```
### Outro container dados persiste no volume criado. 
```
$ docker container run -it --mount type=bind,src=/opt/giropops,dst=/giropops debian 

$ root@265ea222ba35:/giropops# ls
teste.txt

root@265ea222ba35:/giropops# touch teste1.txt

root@265ea222ba35 :/# df -h
Filesystem      Size  Used Avail Use% Mounted on
overlay          34G   15G   18G  46% /
tmpfs            64M     0   64M   0% /dev
tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
shm              64M     0   64M   0% /dev/shm
/dev/sda3        34G   15G   18G  46% /giropops
tmpfs           3.9G     0  3.9G   0% /proc/asound
tmpfs           3.9G     0  3.9G   0% /proc/acpi
tmpfs           3.9G     0  3.9G   0% /proc/scsi
tmpfs           3.9G     0  3.9G   0% /sys/firmware
```
### Read only (Somente Leitura).
```
$ docker container run -it --mount type=bind,src=/opt/giropops,dst=/giropops,ro debian
root@b742436810e6:/# ls
    bin   dev  giropops  lib    media  opt	 root  sbin  sys  usr
    boot  etc  home      lib64  mnt    proc  run   srv   tmp  var
    
root@b742436810e6:/# cd giropops/

root@b742436810e6:/giropops# ls

teste.txt  teste1.txt

root@b742436810e6:/giropops# rm teste.txt 
rm: cannot remove 'teste.txt': Read-only file system
```
### Subcomando docker volume.
```
$ docker volume create giropops
giropops
```

```
$ docker volume ls
DRIVER    VOLUME NAME
local     giropops
```
```
$ docker volume inspect giropops
[
    {
        "CreatedAt": "2022-03-02T14:56:09-04:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/giropops/_data",
        "Name": "giropops",
        "Options": {},
        "Scope": "local"
    }
]
```
```
cd /var/lib/docker/volumes ls
giropops  metadata.db
```
```
cd /var/lib/docker/volumes/giropops/_data
```
```
$ docker container run -it --mount type=volume,src=giropops,dst=/giropops debian 
```
```
root@d378ad153293:/# ls
bin   dev  giropops  lib    media  opt	 root  sbin  sys  usr
boot  etc  home      lib64  mnt    proc  run   srv   tmp  var
```
```
root@d378ad153293:/# cd giropops/
```
```
root@d378ad153293:/giropops# ls
teste  teste2
```

#### Arquivos criando anteriormente dentro do diretorio /var/lib/docker/volumes/giropops, no docker.

* Sair sem matar o container *ctrl+p+q*.
* tentando remover o container.

```
$ docker volume rm giropops 
 Error response from daemon: remove giropops: volume is in use - [d378ad153293e88e5b72103a2faf28d9887f4878bd97326f97583f5cee1ac6c7, 55e72fa7b85d7cc364eaaa184ae11bf9cf431b2203e54ff1adb3db0b9d9b6bbe]
 ```
### Para remover o volume e necessário remover os container vinculado a esse volume. 
```
$ docker container ls -a
```
```
$ docker volume rm giropops
giropops
```
### (Cuidado)remover volume que nao esta sendo utilizado.
```
$ docker volume prune
WARNING! This will remove all local volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
```
###  Remove container que não estão sendo utilizado. 
```
$ docker containe prune
```
### Remove as images que não estão sendo utilizado. 
```
$ docker image prune
```
### somente criar o container com comando antigo.
```
$ docker volume create dbdados
```
```
$ docker volume ls
```
### (old + Funciona).
```
$ docker container create -v /opt/giropops/:/giropops --name dbdados centos
```
```
$ docker container ls -a
```
### Criando Banco POSTGRESQL (sistaxe antiga).
```
$ docker run -d -p 5432:5432 --name pgsql1 --volumes-from dbdados -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql
```
 - porta: -p 5432:5432 (localhost/container)
 - nome: do container (--name pgsql1)
 - Volume: --volumes-from dbdados (montar nesse volume)
 - environment: -e (variavel de ambiente "-e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker") 
 - imagem: kamui/postgresql

### Localizando o mounts.
```
$ docker inspect {ID}
/var/lib/docker/volumes/63a575c6c1b3556115f6c4ce038e4fdb2338bc1b2bf3fa4427c25d1a892f59c2/_data
```
### Cenário criando o volume.
```
$ docker volume create dbdados
```
```
$ docker volume ls
DRIVER    VOLUME NAME
local     dbdados
```
### Criando 2 bancos compartilhando mesmo volume.
```
$ docker run -d -p 5432:5432 --name pgsql1  --mount type=volume,src=dbdados,dst=/data -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql
```
```
$ docker run -d -p 5433:5432 --name pgsql2  --mount type=volume,src=dbdados,dst=/data -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql
```
```
$ docker inspect dbdados
[
    {
        "CreatedAt": "2022-03-03T17:35:14-04:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/dbdados/_data",
        "Name": "dbdados",
        "Options": {},
        "Scope": "local"
    }
]
```
```
 #/var/lib/docker/volumes/dbdados/_data# ls
base         pg_ident.conf  pg_snapshots  pg_tblspc    postgresql.conf  server.key
global       pg_multixact   pg_stat       pg_twophase  postmaster.opts
pg_clog      pg_notify      pg_stat_tmp   PG_VERSION   postmaster.pid
pg_hba.conf  pg_serial      pg_subtrans   pg_xlog      server.crt
```
### Levantando um container para BKP de dados ex usado banco de dados.
```
$ docker container run -ti --mount type=volume,src=dbdados,dst=/data --mount type=bind,src=/opt/backupDB,dst=/backup debian tar -cvf /backup/bkp-banco.tar /data
```
```
 #/opt/backupDB# ls
bkp-banco.tar
```
```
/opt/backupDB$ sudo tar -xvf bkp-banco.tar
```
#
# Dia 2 Dockerfile (sepadando a evolução do arquivo Dockerfile por Dir).
### **DockerFile (dir 01)**
```
FROM debian

RUN apt-get update && apt-get install -y apache2
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

LABEL description="Webserver"
LABEL version="1.0.0"

VOLUME /var/www/html/
EXPOSE 80
```
#
### Adicionando Entrypoint, CMD. (dir 02).
```
FROM debian

RUN apt-get update && apt-get install -y apache2
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

LABEL description="Webserver"
LABEL version="1.0.0"

VOLUME /var/www/html/
EXPOSE 80

# Entrypoint processo para executa o apache em primeiro plano, principal processo do container, (modo exec)
ENTRYPOINT ["/usr/sbin/apachectl"]

# CMD passa parametros, pro principal processo do container "/usr/sbin/apachectl" 
CMD ["-D", "FOREGROUND"]
```
### Montando a IMG Build.

```
$ docker image build -t meu_apache:1.0 .
```
### Para não utilizar cache que foi gerado no primeiro build. 
```
$ docker image build -t meu_apache:2.0 . --no-cache
```
### Gerando o container apontando a porta 80 do container para a porta 8080 no meu localhost. 
```
$ docker container run -d -p 8080:80 meu_apache:1.0 

$ docker ps
CONTAINER ID   IMAGE            COMMAND                  CREATED        STATUS        PORTS                                   NAMES
7a41eb807307   meu_apache:1.0   "/usr/sbin/apachectl…"   25 hours ago   Up 25 hours   0.0.0.0:8080->80/tcp, :::8080->80/tcp   infallible_poitras

$ curl localhost:8080
(Mostrar a saida da pagina no apache)
```
#
#### **Dockerfile COPY (dir 03)**
#### Criar o arquivo index.html na raiz do dockerfile (03/index.html).
```
<!DOCTYPE html>
<html>

<head>
  <title>Título do documento</title>
</head>

<body>
  Docker APACHE TESTE
</body>

</html>
```
 ### (COPY index.html /var/www/html/), vai copiar o arquivo index.html para o diretório (/var/www/html/) dentro do container.
```
FROM debian

RUN apt-get update && apt-get install -y apache2
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

COPY index.html /var/www/html/

LABEL description="Webserver"
LABEL version="1.0.0"

VOLUME /var/www/html/
EXPOSE 80

# Entrypoint processo para executa o apache em primeiro plano, principal processo do container, (modo exec)
ENTRYPOINT ["/usr/sbin/apachectl"]

# CMD passa parametros, pro principal processo do container "/usr/sbin/apachectl" 
CMD ["-D", "FOREGROUND"]
```
### Criando a imagem
```
$ docker image build -t meu_apache:4.0 .
```
### Gerando container.
```
$ docker container run -d -p8001 meu_apache:4.0
```
```
$ docker container ls 
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS                                   NAMES
bcf7e3c5c72d   meu_apache:4.0   "/usr/sbin/apachectl…"   3 seconds ago    Up 2 seconds    0.0.0.0:8001->80/tcp, :::8001->80/tcp   thirsty_kowalevski
aa529875c14e   9f91b0bc57f5     "/usr/sbin/apachectl…"   20 minutes ago   Up 20 minutes   0.0.0.0:8000->80/tcp, :::8000->80/tcp   romantic_kilby
7b8271f78683   meu_apache:3.0   "/usr/sbin/apachectl…"   24 minutes ago   Up 24 minutes   0.0.0.0:8081->80/tcp, :::8081->80/tcp   interesting_haibt
7a41eb807307   meu_apache:1.0   "/usr/sbin/apachectl…"   27 hours ago     Up 27 hours     0.0.0.0:8080->80/tcp, :::8080->80/tcp   infallible_poitras
```
### Testando copia que está no diretorio do dockerfile.
```
$ curl localhost:8001
<!DOCTYPE html>
<html>

<head>
  <title>Título do documento</title>
</head>

<body>
  Docker APACHE TESTE
</body>

```
### Verificando o arquivos no container o diretório.
```
$ docker container exec -it bcf7e3c5c72d bash

root@bcf7e3c5c72d:/# cd var/www/html/           
root@bcf7e3c5c72d:/var/www/html# ls
index.html

```
### Usando a opção ADD no lugar do COPY.

#### COPY 
* -> Copia arquivos e diretórios e adiciona no container
#### ADD 
* -> Copia arquivos e diretórios com 2 funcões a mais, pega arquivos TAR e copia no container descompactado, somente o conteúdo do arquivo, em relação a arquivo remoto, ex: www.meusite.com/conteudo, ele consegue fazer o download do conteudo e adicionar ao diretório do container. 
```
FROM debian

RUN apt-get update && apt-get install -y apache2
RUN chown www-data:www-data /var/lock && chown www-data:www-data /var/run/ && chown www-data:www-data /var/log/
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

ADD index.html /var/www/html/

LABEL description="Webserver"
LABEL version="1.0.0"

# Expecifica o usuário padrão para container 
USER root
# expecifica o diretorio padrão do container
WORKDIR /var/www/html/

VOLUME /var/www/html/
EXPOSE 80

ENTRYPOINT ["/usr/sbin/apachectl"]

CMD ["-D", "FOREGROUND"]
```
### Buildando a imagem.
```
$ docker image build -t meu_apache:08 .
```
```
$ docker run -d -p 8005:80 meu_apache:08

$ docker container ps
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS         PORTS                                   NAMES
f4015ee49dcb   meu_apache:08   "/usr/sbin/apachectl…"   5 seconds ago   Up 5 seconds   0.0.0.0:8005->80/tcp, :::8005->80/tcp   musing_cannon

$ docker exec -it  f4015ee49dcb bash
root@f4015ee49dcb:/var/www/html#
```
#
#### **Dockerfile COPY (dir 04)**
```
FROM python:3

WORKDIR /usr/src/app

COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD [ "python", "./main" ]
```
### main.py.
```
def somalista(numeros):
    soma = 0
    for i in numeros:
        soma = soma + i
    return soma

print(somalista([1,3,5,7,9]))

```
```
$ docker image build -t meu_python:1.0 .
....
$ docker image ls
.....
$ docker run -it meu_python:1.0
25
```
#
### MultiStage.
 A utilização do multistage é utilizado para reduzir o tamanho da imagem final.<p> Por exemplo, para uma aplicação GO, precisaríamos utilizar a imagem GOLANG para realizar o build da imagem, essa imagem possui em torno de 800MB, logo minha imagem final teria os 800MB mais o tamanho da minha aplicação GO.<p> Para contornar isso, poderíamos utilizar a imagem GOLANG apenas para gerar o executável da aplicação GO e depois copia-lo para uma imagem menor como a imagem ALPINE. <p>OBS: A utilização do MULTISTAGE depende muito de como funciona sua aplicação, pois é necessário copiar os arquivos para a outra imagem, com exemplo o APACHE seria bem difícil de fazer. Apenas como comparação, utilizando o multistage a imagem final ficou com 7MB e sem o multistage a imagem final ficou com 800MB.

#### criar um arquivo dentro do dir "meu_go.go".
```
package main
import "fmt"

func main() {
        fmt.Println("Giropops Strigus Girus")
}

```
### dockerfile.
```
FROM golang

WORKDIR /app
ADD . /app
RUN go build -o meugo

ENTRYPOINT ./meugo
```

```
$ docker image build -t meu_go:1.0 .
....
$ docker image ls
.....
$ docker run -it meu_go:1.0
Giropops Strigus Girus

```
#
## **DOCKER HUB**
### Enviando uma imagem local para repositório de imgs do docker.
```
$ Docker image ls

#escolha sua imagem, apois execute esse comando abaixo.

$ docker image tag 0a66b9b7b0c1 evertonstm/meu_apache:1.0.0

# imagem já com a tag correta
$ docker image ls| grep evertonstm
evertonstm/meu_apache   1.0.0     0a66b9b7b0c1   2 hours ago         252MB

# logar no docker hub
$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: evertonstm
Password: 

$ docker push evertonstm/meu_apache:1.0.0
The push refers to repository [docker.io/evertonstm/meu_apache]
0d34571c1aaa: Pushed 
31cbd343fd41: Pushed 
89fda00479fc: Mounted from library/python 
1.0.0: digest: sha256:71c34493854b5f65c8fa894da101e795a04767720b28077acf2aa8d2b9165aff size: 948
```
## Implantar um servidor Registry.
### Use um comando como o seguinte para iniciar o contêiner do registro:
```
 docker run -d -p 5000:5000 --restart=always --name registry registry:2

```
#### Marque a imagem como localhost:5000/meu_apache:1.0.0. Isso cria uma tag adicional para a imagem existente.<p> Quando a primeira parte da tag é um nome de host e uma porta, o Docker interpreta isso como o local de um registro ao enviar.

```
$ docker image tag b9079391a49d localhost:5000/meu_apache:1.0.0

$ docker image push localhost:5000/meu_apache:1.0.0

$ docker run -d -p 8080:80 localhost:5000/meu_apache:1.0.0

```

### Como verificar as imagens.
```
$ curl localhost:5000/v2/_catalog
{"repositories":["meu_apache"]}

$ curl localhost:5000/v2/meu_apache/tags/list
{"name":"meu_apache","tags":["1.0.0"]}

```
#
### HEALTHCHECK (usando o dockerfile do dir 03).
* O healtcheck checará a cada 5 minutos se a URL localhost está respondendo, caso ela falhe "||" então o container será encerrado com o código 1.
```
HEALTCHECK --interval 5m --timeout=3s \
    CMD curl --retry-max-time 2 -f http://localhost/ || exit 1
```
```
FROM debian

RUN apt-get update && apt-get install -y apache2 curl
RUN chown www-data:www-data /var/lock && chown www-data:www-data /var/run/ && chown www-data:www-data /var/log/
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

ADD index.html /var/www/html/

# O healtcheck checará a cada 5 minutos se a URL localhost está respondendo, caso ela falhe "||" então o container será encerrado com o código 1
HEALTHCHECK --interval=1m --timeout=3s \
  CMD curl -f http://localhost/ || exit 1

LABEL description="Webserver"
LABEL version="1.0.0"

USER root

WORKDIR /var/www/html/

VOLUME /var/www/html/
EXPOSE 80

ENTRYPOINT ["/usr/sbin/apachectl"]

CMD ["-D", "FOREGROUND"]
```
### Atenção: o comando utilizado para o healthcheck deve existir na imagem.
#
## Editar container (Criar uma imagem de container a partir do container).
```
$ docker container run -it ubuntu

root@15ef4275b6e2:/# apt-get update && apt-get install -y nano curl vim

root@15ef4275b6e2:/# cat /etc/issue
Ubuntu 20.04.4 LTS \n \l

root@15ef4275b6e2:/# echo "TESTE DISTRO" > /etc/issue

root@15ef4275b6e2:/# cat /etc/issue
TESTE DISTRO

```
### Saindo do container com CRTL+P+Q, para continuar em execução. 

### Container foi commitado sem a TAG e sem nome:
```
$ docker commit -m "Ubuntu com Vim, Curl, nano" 15ef4275b6e2
sha256:2ab2f2f779b7b9f792934336d797a5c67dbd6816968a7c66d2c6ddc533003c84

$ docker image ls
REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE
<none>                            <none>    2ab2f2f779b7   6 seconds ago    185MB
meu_apache                        1.0.0     46b1a886c531   38 minutes ago   253MB
```
### Editando a TAG.
```
$ docker image tag 2ab2f2f779b7 ubuntu_vim_curl:1.0

$ docker image ls
REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE
ubuntu_vim_curl                   1.0       2ab2f2f779b7   3 minutes ago    185MB
meu_apache                        1.0.0     46b1a886c531   42 minutes ago   253MB
```
### Executando o container.
```
$ docker container run -ti ubuntu_vim_curl:1.0 
root@41e1ed80db18:/# cat /etc/issue
TESTE DISTRO
root@41e1ed80db18:/# nano 
.dockerenv  dev/        lib/        libx32/     opt/        run/        sys/        var/        
bin/        etc/        lib32/      media/      proc/       sbin/       tmp/        
boot/       home/       lib64/      mnt/        root/       srv/        usr/        
root@41e1ed80db18:/# vim  
vim        vim.basic  vimdiff    vimtutor   
root@41e1ed80db18:/# vim 
```
#### **OBs: o Ideal e sempre criar por dockerfile**
#
## **DIA 03 (Docker MACHINE)**
(https://github.com/docker/machine/releases) <p>
Docker-machine é utilizado criar uma VM remota para rodar o docker, pode ser utilizado para criar a VM na AWS, Digital Ocean, Virtual Box, Hyper-V entre outros.<p>
Utilize o docker-machine para gerenciar um host remoto com o Docker
```
curl -L https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine && chmod +x /tmp/docker-machine && sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
```
~~~
$ docker-machine --version
docker-machine version 0.16.2, build bd45ab13
~~~
~~~
$ docker-machine create --driver virtualbox giropops
Creating CA: /home/everton/.docker/machine/certs/ca.pem
Creating client certificate: /home/everton/.docker/machine/certs/cert.pem
Running pre-create checks...
(giropops) Image cache directory does not exist, creating it at /home/everton/.docker/machine/cache...
(giropops) No default Boot2Docker ISO found locally, downloading the latest release...
(giropops) Latest release for github.com/boot2docker/boot2docker is v19.03.12
(giropops) Downloading /home/everton/.docker/machine/cache/boot2docker.iso from https://github.com/boot2docker/boot2docker/releases/download/v19.03.12/boot2docker.iso...
(giropops) 0%....10%....20%....30%....40%....50%....60%....70%....80%....90%....100%
Creating machine...
(giropops) Copying /home/everton/.docker/machine/cache/boot2docker.iso to /home/everton/.docker/machine/machines/giropops/boot2docker.iso...
(giropops) Creating VirtualBox VM...
(giropops) Creating SSH key...
(giropops) Starting the VM...
(giropops) Check network to re-create if needed...
(giropops) Found a new host-only adapter: "vboxnet0"
Error creating machine: Error in driver during machine creation: Error setting up host only network on machine start: /usr/bin/VBoxManage hostonlyif ipconfig vboxnet0 --ip 192.168.99.1 --netmask 255.255.255.0 failed:
VBoxManage: error: Code E_ACCESSDENIED (0x80070005) - Access denied (extended info not available)
VBoxManage: error: Context: "EnableStaticIPConfig(Bstr(pszIp).raw(), Bstr(pszNetmask).raw())" at line 242 of file VBoxManageHostonly.cpp
~~~
### VBoxManage: error:
* Existem agora 2 soluções óbvias, uma seria mudar a maneira como o docker cria sua máquina para que ela se encaixe no “novo” espaço de endereço que o VirtualBox:
~~~
$ docker-machine create --driver virtualbox --virtualbox-memory "2048" --virtualbox-hostonly-cidr 192.168.99.1/21 default
~~~
* Também podemos resolver isso do outro lado do problema, que é mudar o comportamento do VirtualBox. Para isso precisamos criar o arquivo networks.conf em /etc/vbox. No network.confs podemos dizer ao VirtualBox quais redes estamos permitindo:
~~~
sudo mkdir /etc/vbox
sudo vi /etc/vbox/networks.conf
 
cat /etc/vbox/networks.conf
* 10.0.0.0/8 192.168.99.0/16
* 2001::/64
~~~
### Acessando container via SSH.
~~~
$ docker-machine ls
NAME       ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER      ERRORS
giropops   -        virtualbox   Running   tcp://192.168.99.101:2376           v19.03.12   
$ docker-machine ssh giropops
   ( '>')
  /) TC (\   Core is distributed with ABSOLUTELY NO WARRANTY.
 (/-_--_-\)           www.tinycorelinux.net

docker@giropops:~$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
docker@giropops:~$ cat /etc/issue                                                                        
Core Linux
docker@giropops:~$ logout
~~~
~~~
$ docker-machine env giropops

export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.101:2376"
export DOCKER_CERT_PATH="/home/everton/.docker/machine/machines/giropops"
export DOCKER_MACHINE_NAME="giropops"
# Run this command to configure your shell: 
# eval $(docker-machine env giropops)
~~~
* Tras algumas variaveis.
~~~
$ docker ps
~~~
* *eval $(docker-machine env {VM})* Executa as 4 variaveis apos isso ja vai conectar no docker do virtualbox s
~~~
$ eval $(docker-machine env giropops)
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
~~~
~~~
$ docker container run -d -p8080:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
f7a1c6dad281: Pull complete 
4d3e1d15534c: Pull complete 
9ebb164bd1d8: Pull complete 
59baa8b00c3c: Pull complete 
a41ae70ab6b4: Pull complete 
e3908122b958: Pull complete 
Digest: sha256:1c13bc6de5dfca749c377974146ac05256791ca2fe1979fc8e8278bf0121d285
Status: Downloaded newer image for nginx:latest
a147657f66973f681a8cd0b5199cd29ecb8938d6149bcba66ef5cd4b47ba4e26

### Como rodei a variável eval note que estou no meu usuário rodando comando na minha MV###
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                  NAMES
a147657f6697   nginx     "/docker-entrypoint.…"   5 seconds ago   Up 5 seconds   0.0.0.0:8080->80/tcp   crazy_montalcini
~~~
* Acessando o endereço do NGINX pelo terminal ou acessar nesse caso pelo navegador no ip entrega a MV na sua criação "192.168.99.101:8080"
~~~
$ curl 192.168.99.101:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
~~~
~~~
$ docker-machine ssh giropops
   ( '>')
  /) TC (\   Core is distributed with ABSOLUTELY NO WARRANTY.
 (/-_--_-\)           www.tinycorelinux.net

docker@giropops:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                  NAMES
a147657f6697   nginx     "/docker-entrypoint.…"   5 seconds ago   Up 5 seconds   0.0.0.0:8080->80/tcp   crazy_montalcini
~~~
* STOP/LS/STATUS/START 
~~~
$ docker-machine stop giropops
Stopping "giropops"...
Machine "giropops" was stopped.

$ docker-machine stop giropops
Stopping "giropops"...
Machine "giropops" was stopped.

$ docker-machine ls giropops
NAME       ACTIVE   DRIVER       STATE     URL   SWARM   DOCKER    ERRORS
giropops   -        virtualbox   Stopped                 Unknown   

$ docker-machine status giropops
Stopped

$ docker-machine start giropops
Starting "giropops"...
(giropops) Check network to re-create if needed...
(giropops) Waiting for an IP...
Machine "giropops" was started.
Waiting for SSH to be available...
Detecting the provisioner...
Started machines may have new IP addresses. You may need to re-run the `docker-machine env` command.

$ docker env giropops
docker: 'env' is not a docker command.
See 'docker --help'

$ docker-machine env giropops
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.101:2376"
export DOCKER_CERT_PATH="/home/everton/.docker/machine/machines/giropops"
export DOCKER_MACHINE_NAME="giropops"
# Run this command to configure your shell: 
# eval $(docker-machine env giropops)
~~~
* Removendo o *Eval*.
~~~
$ docker-machine env giropops
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.101:2376"
export DOCKER_CERT_PATH="/home/everton/.docker/machine/machines/giropops"
export DOCKER_MACHINE_NAME="giropops"
# Run this command to configure your shell: 
# eval $(docker-machine env giropops)

$ eval $(docker-machine env -u)
~~~

* Removendo MV
~~~
$ docker-machine rm giropops
About to remove giropops
WARNING: This action will delete both local reference and remote instance.
Are you sure? (y/n): y
Successfully removed giropops
~~~
#
## Docker Swarm.
```
docker-machine create --driver virtualbox mv01

docker-machine create --driver virtualbox mv02

docker-machine create --driver virtualbox mv03
```
~~~
$ docker-machine ls
NAME   ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER      ERRORS
mv01   -        virtualbox   Running   tcp://192.168.99.100:2376           v19.03.12   
mv02   -        virtualbox   Running   tcp://192.168.99.101:2376           v19.03.12   
mv03   -        virtualbox   Running   tcp://192.168.99.102:2376           v19.03.12   
~~~
~~~
$ docker swarm init --advertise-addr 192.168.100.100
Swarm initialized: current node (56v42fcp0z98yqpopkq576m7q) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token {Hash GERADO PELO SWARM} 192.168.100.100:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
~~~
* Guardei no arquivo swarm
~~~ 
$ echo docker swarm join --token {Hash GERADO PELO SWARM} 192.168.100.100:2377 > swarm
~~~
~~~
$ docker-machine ssh mv01
   ( '>')
  /) TC (\   Core is distributed with ABSOLUTELY NO WARRANTY.
 (/-_--_-\)           www.tinycorelinux.net

docker@mv01:~$ docker swarm join --token {Hash GERADO PELO SWARM} 192.168.100.100:2377
This node joined a swarm as a worker.
docker@mv01:~$ exit                                                                         
~~~
~~~
$ docker-machine ssh mv02
   ( '>')
  /) TC (\   Core is distributed with ABSOLUTELY NO WARRANTY.
 (/-_--_-\)           www.tinycorelinux.net

docker@mv02:~$ docker swarm join --token {Hash GERADO PELO SWARM} 192.168.100.100:2377
This node joined a swarm as a worker.
docker@mv02:~$ exit                                                                                                                                                                                                                    
logout
~~~
~~~
$ docker-machine ssh mv03
   ( '>')
  /) TC (\   Core is distributed with ABSOLUTELY NO WARRANTY.
 (/-_--_-\)           www.tinycorelinux.net

docker@mv03:~$ docker swarm join --token {Hash GERADO PELO SWARM} 192.168.100.100:2377
This node joined a swarm as a worker.
docker@mv03:~$ exit                                                                                                                                                                                                                          
logout
~~~

~~~
$ docker-machine ls
NAME   ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER      ERRORS
mv01   -        virtualbox   Running   tcp://192.168.99.100:2376           v19.03.12   
mv02   -        virtualbox   Running   tcp://192.168.99.101:2376           v19.03.12   
mv03   -        virtualbox   Running   tcp://192.168.99.102:2376           v19.03.12   
~~~
~~~
$ docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
j6feplwyq9i7vjurmj00oa9k9 *   vm01                Ready               Active              Leader              19.03.12
6qdaot0t39x3f6rkprbdhnhp5     vm02                Ready               Active                                  19.03.12
rwb3z1m27x8fl9xl3wnf7ksg3     vm03                Ready               Active                                  19.03.12
~~~
* Promovendo o VM02 para manager (*MANAGER STATUS/Reachable*)
~~~
docker@vm01:~$ docker node promote vm02                                                      
Node vm02 promoted to a manager in the swarm.

docker@vm01:~$ docker node ls                                                                
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
j6feplwyq9i7vjurmj00oa9k9 *   vm01                Ready               Active              Leader              19.03.12
6qdaot0t39x3f6rkprbdhnhp5     vm02                Ready               Active              Reachable           19.03.12
rwb3z1m27x8fl9xl3wnf7ksg3     vm03                Ready               Active                                  19.03.12
~~~
* Promovendo o VM03 para manager (*MANAGER STATUS/Reachable*)
~~~
docker@vm01:~$ docker node promote vm03                                                      
Node vm03 promoted to a manager in the swarm.
docker@vm01:~$ docker node ls                                                                
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
j6feplwyq9i7vjurmj00oa9k9 *   vm01                Ready               Active              Leader              19.03.12
6qdaot0t39x3f6rkprbdhnhp5     vm02                Ready               Active              Reachable           19.03.12
rwb3z1m27x8fl9xl3wnf7ksg3     vm03                Ready               Active              Reachable           19.03.12
~~~
* Removendo o VM03 de manager (*MANAGER STATUS*)
~~~
docker@vm01:~$ docker node demote vm03                                                       
Manager vm03 demoted in the swarm.
docker@vm01:~$ docker node ls                                                                
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
j6feplwyq9i7vjurmj00oa9k9 *   vm01                Ready               Active              Leader              19.03.12
6qdaot0t39x3f6rkprbdhnhp5     vm02                Ready               Active              Reachable           19.03.12
rwb3z1m27x8fl9xl3wnf7ksg3     vm03                Ready               Active                                  19.03.12
~~~
#### Recuperando o Token de worker gerado pelo comando docker swarm init
~~~
$ docker swarm join-token worker                                                
To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-28g9f5cy9560snsp7rocj5pvbgd09s7ocqm1ol3u4k2vlwu2vg-8yp4apqbtv8qbcz5xtqp8sl2i 192.168.99.102:2377
~~~
#### Recuperando o Token de manager gerado pelo comando docker swarm init.<p> Nesse caso ao adicionar um nó com esse token ja entra no cluster como manager. 
~~~
$ docker swarm join-token manager                                               
To add a manager to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-28g9f5cy9560snsp7rocj5pvbgd09s7ocqm1ol3u4k2vlwu2vg-8oms09ledop7ahrhbqfiojwy7 192.168.99.102:2377
~~~
#### Comando *--rotate* gera um novo token para novos nós a ser adicionado. <p> Para os nós com token antigo não faz diferença, a partir desse momento o token antigo que gerou o cluste atual para de funcionar.
~~~
docker@vm01:~$ docker swarm join-token --rotate worker                                       
Successfully rotated worker join token.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-28g9f5cy9560snsp7rocj5pvbgd09s7ocqm1ol3u4k2vlwu2vg-a6w2e5sdmjp8uz56fvne6a8ko 192.168.99.102:2377
~~~
~~~
docker@vm01:~$ docker swarm join-token --rotate manager                                      
Successfully rotated manager join token.

To add a manager to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-28g9f5cy9560snsp7rocj5pvbgd09s7ocqm1ol3u4k2vlwu2vg-bvqwiehzw1i9fw53lwfnnfjqk 192.168.99.102:2377

~~~
* Criando um serviço Webserver Nginx com 3 replicas.
~~~
docker@vm01:~$ docker service create --name webserver --replicas 3 -p 8081:80 nginx          
kkfsq5j2hzmkdbvpak804idqx
overall progress: 3 out of 3 tasks 
1/3: running   
2/3: running   
3/3: running   
verify: Service converged 

docker@vm01:~$ docker service ps webserver                                                   
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
a2gwpqfmreoq        webserver.1         nginx:latest        vm02                Running             Running 35 seconds ago                       
t4hh3vb98m8g        webserver.2         nginx:latest        vm03                Running             Running 35 seconds ago                       
qs7jvbsmln7z        webserver.3         nginx:latest        vm01                Running             Running 32 seconds ago                       
~~~
* Pausando o VM02 para não receber mais serviços.
~~~
docker@vm01:~$ docker node update --availability pause vm02                                  
vm02

docker@vm01:~$ docker inspect vm02                                                           
[
    {
        "ID": "6qdaot0t39x3f6rkprbdhnhp5",
        "Version": {
            "Index": 52
        },
        "CreatedAt": "2022-03-09T16:13:34.138567217Z",
        "UpdatedAt": "2022-03-09T17:40:57.992035341Z",
        "Spec": {
            "Labels": {},
            "Role": "manager",
    ------- EM PAUSA ---------------      
            "Availability": "pause"
    --------------------------------        
        },
        "Description": {
            "Hostname": "vm02",
            "Platform": {
                "Architecture": "x86_64",
                "OS": "linux"
            },
  .............................
]
~~~
* Escalando 10 serviços(Note que no VM02 não receberá mais serviços, somente o que já estava escalado anteriormente).
~~~
docker@vm01:~$ docker service scale webserver=10                                             
webserver scaled to 10
overall progress: 10 out of 10 tasks 
1/10: running   
2/10: running   
3/10: running   
4/10: running   
5/10: running   
6/10: running   
7/10: running   
8/10: running   
9/10: running   
10/10: running   
verify: Service converged 
~~~
#### (Note que no VM02 não receberá mais serviços, somente o que ja estava escalado anteriormente).
~~~
docker@vm01:~$ docker service ps webserver                                                   
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
------------------------------------------------------------------------------------------------------------------------
a2gwpqfmreoq        webserver.1         nginx:latest        vm02                Running             Running 19 minutes ago                       
------------------------------------------------------------------------------------------------------------------------
t4hh3vb98m8g        webserver.2         nginx:latest        vm03                Running             Running 19 minutes ago                       
qs7jvbsmln7z        webserver.3         nginx:latest        vm01                Running             Running 19 minutes ago                       
9vspmu4f842l        webserver.4         nginx:latest        vm03                Running             Running 25 seconds ago                       
at12l9f1l0jf        webserver.5         nginx:latest        vm01                Running             Running 25 seconds ago                       
vwwn3lone6f2        webserver.6         nginx:latest        vm03                Running             Running 25 seconds ago                       
hdnmefpup1yv        webserver.7         nginx:latest        vm01                Running             Running 25 seconds ago                       
hekshfcev083        webserver.8         nginx:latest        vm03                Running             Running 25 seconds ago                       
uxgylr92d5c2        webserver.9         nginx:latest        vm01                Running             Running 25 seconds ago                       
vz5yn3eqezwr        webserver.10        nginx:latest        vm03                Running             Running 25 seconds ago               
~~~
~~~
docker@vm01:~$ docker service scale webserver=3                                              
webserver scaled to 3
overall progress: 3 out of 3 tasks 
1/3: running   
2/3: running   
3/3: running   
verify: Service converged 
~~~
~~~
docker@vm01:~$ docker service ps webserver                                                   
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
a2gwpqfmreoq        webserver.1         nginx:latest        vm02                Running             Running 20 minutes ago                       
t4hh3vb98m8g        webserver.2         nginx:latest        vm03                Running             Running 20 minutes ago                       
qs7jvbsmln7z        webserver.3         nginx:latest        vm01                Running             Running 20 minutes ago                       
~~~
* Ativando novamente o no VM02 e scalando novamente.
~~~
docker@vm01:~$ docker node update --availability active vm02                                 
vm02

docker@vm01:~$ docker service scale webserver=10                                             
webserver scaled to 10
overall progress: 10 out of 10 tasks 
1/10: running   
2/10: running   
3/10: running   
4/10: running   
5/10: running   
6/10: running   
7/10: running   
8/10: running   
9/10: running   
10/10: running   
verify: Service converged 

docker@vm01:~$ docker service ps webserver                                                   
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
a2gwpqfmreoq        webserver.1         nginx:latest        vm02                Running             Running 20 minutes ago                       
t4hh3vb98m8g        webserver.2         nginx:latest        vm03                Running             Running 20 minutes ago                       
qs7jvbsmln7z        webserver.3         nginx:latest        vm01                Running             Running 20 minutes ago                       
xz215hmmjeeu        webserver.4         nginx:latest        vm03                Running             Running 10 seconds ago                       
h2oq3802jhqz        webserver.5         nginx:latest        vm01                Running             Running 10 seconds ago                       
muu2oihspz2u        webserver.6         nginx:latest        vm01                Running             Running 10 seconds ago                       
u2y3utsqf509        webserver.7         nginx:latest        vm02                Running             Running 10 seconds ago                       
ah87imvof11s        webserver.8         nginx:latest        vm03                Running             Running 10 seconds ago                       
weo0y8ab71sd        webserver.9         nginx:latest        vm01                Running             Running 10 seconds ago                       
d97buissgkog        webserver.10        nginx:latest        vm02                Running             Running 10 seconds ago                       
~~~
* Usando a tag dain para manutenção do nó (os serviços foram alocado para outros nós).
~~~
docker@vm01:~$ docker node update --availability drain vm02                                  
vm02
docker@vm01:~$ docker service ps webserver                                                   
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE                     ERROR               PORTS
------------------------------------------------------------------------------------------------------------------------------
ujbb8icy0yii        webserver.1         nginx:latest        vm03                Running             Running less than a second ago                        
a2gwpqfmreoq         \_ webserver.1     nginx:latest        vm02                Shutdown            Shutdown less than a second ago 
-------------------------------------------------------------------------------------------------------------------------------         t4hh3vb98m8g        webserver.2         nginx:latest        vm03                Running             Running 21 minutes ago                                
qs7jvbsmln7z        webserver.3         nginx:latest        vm01                Running             Running 21 minutes ago                                
xz215hmmjeeu        webserver.4         nginx:latest        vm03                Running             Running about a minute ago                            
h2oq3802jhqz        webserver.5         nginx:latest        vm01                Running             Running about a minute ago                            
muu2oihspz2u        webserver.6         nginx:latest        vm01                Running             Running about a minute ago                            
iw0drzfua6ht        webserver.7         nginx:latest        vm03                Running             Running less than a second ago          
u2y3utsqf509         \_ webserver.7     nginx:latest        vm02                Shutdown            Shutdown less than a second ago  
--------------------------------------------------------------------------------------------------------------------------------
ah87imvof11s        webserver.8         nginx:latest        vm03                Running             Running about a minute ago                            
weo0y8ab71sd        webserver.9         nginx:latest        vm01                Running             Running about a minute ago                            
l7uqw3dk8e3f        webserver.10        nginx:latest        vm01                Running             Running less than a second ago 
--------------------------------------------------------------------------------------------------------------------------------                       
d97buissgkog         \_ webserver.10    nginx:latest        vm02                Shutdown            Shutdown less than a second ago     
~~~
* Note que o serviço não volta ao nó, por segurança, escale uma quandidade menor. 
~~~
docker@vm01:~$ docker service ps webserver                                                   
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE                ERROR               PORTS
ujbb8icy0yii        webserver.1         nginx:latest        vm03                Running             Running 14 seconds ago                           
a2gwpqfmreoq         \_ webserver.1     nginx:latest        vm02                Shutdown            Shutdown 15 seconds ago                          
t4hh3vb98m8g        webserver.2         nginx:latest        vm03                Running             Running 21 minutes ago                           
qs7jvbsmln7z        webserver.3         nginx:latest        vm01                Running             Running 21 minutes ago                           
xz215hmmjeeu        webserver.4         nginx:latest        vm03                Running             Running about a minute ago                       
h2oq3802jhqz        webserver.5         nginx:latest        vm01                Running             Running about a minute ago                       
muu2oihspz2u        webserver.6         nginx:latest        vm01                Running             Running about a minute ago                       
iw0drzfua6ht        webserver.7         nginx:latest        vm03                Running             Running 14 seconds ago                           
u2y3utsqf509         \_ webserver.7     nginx:latest        vm02                Shutdown            Shutdown 15 seconds ago                          
ah87imvof11s        webserver.8         nginx:latest        vm03                Running             Running about a minute ago                       
weo0y8ab71sd        webserver.9         nginx:latest        vm01                Running             Running about a minute ago                       
l7uqw3dk8e3f        webserver.10        nginx:latest        vm01                Running             Running 15 seconds ago                           
d97buissgkog         \_ webserver.10    nginx:latest        vm02                Shutdown            Shutdown 15 seconds ago                                       
~~~
~~~         
docker@vm01:~$ docker service scale webserver=1                                                                                                                                              
webserver scaled to 1
overall progress: 1 out of 1 tasks 
1/1: running   [==================================================>] 
verify: Service converged 
docker@vm01:~$ docker service ps webserver                                                                                                                                                   
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
ujbb8icy0yii        webserver.1         nginx:latest        vm03                Running             Running 6 minutes ago                        
a2gwpqfmreoq         \_ webserver.1     nginx:latest        vm02                Shutdown            Shutdown 6 minutes ago  
~~~
~~~                     
docker@vm01:~$ docker service scale webserver=10                                                                                                                                             
webserver scaled to 10
overall progress: 10 out of 10 tasks 
1/10: running   [==================================================>] 
2/10: running   [==================================================>] 
3/10: running   [==================================================>] 
4/10: running   [==================================================>] 
5/10: running   [==================================================>] 
6/10: running   [==================================================>] 
7/10: running   [==================================================>] 
8/10: running   [==================================================>] 
9/10: running   [==================================================>] 
10/10: running   [==================================================>] 
verify: Service converged 
docker@vm01:~$ docker service ps webserver                                                                                                                                                   
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
ujbb8icy0yii        webserver.1         nginx:latest        vm03                Running             Running 7 minutes ago                        
a2gwpqfmreoq         \_ webserver.1     nginx:latest        vm02                Shutdown            Shutdown 7 minutes ago                       
3x0ppapeqn1a        webserver.2         nginx:latest        vm01                Running             Running 8 seconds ago                        
ono5wvmgjr02        webserver.3         nginx:latest        vm03                Running             Running 9 seconds ago                        
qe81ll9220ec        webserver.4         nginx:latest        vm02                Running             Running 8 seconds ago                        
l5gxlpu5cfk6        webserver.5         nginx:latest        vm02                Running             Running 8 seconds ago                        
q4zlskhbtywc        webserver.6         nginx:latest        vm02                Running             Running 8 seconds ago                        
8wbhdbjslqvj        webserver.7         nginx:latest        vm01                Running             Running 8 seconds ago                        
fe3brscuxmoy        webserver.8         nginx:latest        vm01                Running             Running 8 seconds ago                        
qfijkou84wm8        webserver.9         nginx:latest        vm03                Running             Running 9 seconds ago                        
r1yak2j0uuzz        webserver.10        nginx:latest        vm01                Running             Running 8 seconds ago                        
docker@vm01:~$                                                                                                                          ~~~
                                                  

