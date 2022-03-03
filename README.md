# Fundamentos docker 
```
$ docker container run -ti {ubuntu}

```
  - Para sair do container sem parar o entrepoint que no caso ubuntu o entrepoint e o bash
  - ctrl + p + q

###  - Para voltar para container novamente 
```
$ docker container attach {ID image or name}
```

### - O entrepoint do nginx e o proprio processo
```
$docker container run -it nginx
```
 
###  - Rodar o container como dimon 
```
$ docker container run -d nginx
```

###  - Tbm vai rodar no entrepoint
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

###   - Acessar o container 
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
###  - Mostra tudo que o container esta usando de recurso 
```
$ docker container stats {container}
```
```
$ docker container top {container}
```
### - Criar container com limite de 128M de memoria
```
$ docker container run -d -m 128M nginx
```
###  - Cria container com 128 de memoria e 1/2 CPU
```
$ docker container run -d -m 128M --cpus 0.5 nginx
```
```
$ docker container inspect {container}
```
### - Update atualiza um contaner ligado nesse caso 20% de 1 Core
```
$ docker container update --cpus 0.2 {container}
```
### - Update atualiza um contaner ligado nesse caso 80% de 1 Core
```
$ docker container update --cpus 0.8% {container} 
```
### - Limitando em 50% de 1 core e 64M de memoria
```
$ docker container update --cpus 0.5 --memory 64M b76e4
```
###  - Remove um container, -f força a remoção de container ligado
```
$ Docker container rm -f {container}
```
###  - Mostrar as imagens local
```
$ docker image ls
```

# DockerFile

### Criar um arquivo dentro de um diretorio nome dockerfile
```
  FROM debian

  LABEL app="Giropops"
  ENV EVERTON="LINDO"

  RUN apt-get update && apt-get install -y stress && apt-get clean

  CMD stress --cpu 1 --vm-bytes 64M --vm 1
```
### (Criando a IMG)
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
###    Nesse caso o logs mostro o stress comando executado pelo CMD no dockerfile
```
$ docker container logs -f {ID}
```

```
$ docker container stats {ID}
```


# Dia 2: (VOLUMES)

### Vamos criar um diretorio dentro do /opt
```
$ sudo mkdir /opt/giropops
```
### Volume do tipo Bind

### Vai montar no diretorio /opt/giropops com destino ao diretorio na raiz do sistema do container /giropops
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
### Read only (Somente Leitura)
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
### Subcomando docker volume
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

### Arquivos criando anteriormente dentro do diretorio /var/lib/docker/volumes/giropops, no docker.

###  Sair sem matar o container ctrl+p+q
###  tentando remover o container 


```
$ docker volume rm giropops 

 Error response from daemon: remove giropops: volume is in use - [d378ad153293e88e5b72103a2faf28d9887f4878bd97326f97583f5cee1ac6c7, 55e72fa7b85d7cc364eaaa184ae11bf9cf431b2203e54ff1adb3db0b9d9b6bbe]
 ```
### Para remover o volume e necessário remover os container vinculado a esse volume 
```
$ docker container ls -a
```

```
$ docker volume rm giropops
giropops
```
### (cuidado)remover volume que nao esta sendo utilizado
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

### (old + Funciona)
```
$ docker container create -v /opt/giropops/:/giropops --name dbdados centos
```

```
$ docker container ls -a
```

### Criando Banco POSTGRESQL (sistaxe antiga)
```
$ docker run -d -p 5432:5432 --name pgsql1 --volumes-from dbdados -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql
```
 - porta: -p 5432:5432 (localhost/container)
 - nome: do container (--name pgsql1)
 - Volume: --volumes-from dbdados (montar nesse volume)
 - environment: -e (variavel de ambiente "-e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker") 
 - imagem: kamui/postgresql

### Localizando o mounts 
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

### Criando 2 bancos compartilhando mesmo volume
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