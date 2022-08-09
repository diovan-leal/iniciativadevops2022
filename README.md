# INICIATIVA-DEVOPS-2022 [EM CONSTRUÇÃO:)]
Este documento é apenas um key notes particular  referente aos assuntos de docker, kubernetes e terraform abordados na semana da iniciativadeveops2022. Ofertado pelo Fabrício Veronez.

Instalação de ferramentas não será abordado.

#Docker
Após a instalação do docker em seu sistema operacional.

Rode o seguinte comando para verificar a versão instalada.
```
docker version
```
Para mais opções de comandos utilize o helper.

```
docker helper
```
## Hello World do Docker

Num primeiro contato com o docker é bastante comum "consumirmos" uma imagem padrão e bastante utilizada do docker que é a imagem hello-world.
```
docker container run hello-world
```
Será exibido em seu terminal algo assim:

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:53f1bbee2f52c39e41682ee1d388285290c5c8a76cc92b42687eecf38e0af3f0
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
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

Explicando o processo realizado ...

Primeiramente o `docker client` procura localmente a imagem solicitada, de acordo com a primeira linha do <i>output</i> gerado.

```
Unable to find image 'hello-world:latest' locally
```
Não encontrando ele baixa a imagem do [Docker hub](https://hub.docker.com/)

```
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:53f1bbee2f52c39e41682ee1d388285290c5c8a76cc92b42687eecf38e0af3f0
Status: Downloaded newer image for hello-world:latest
```

Neste momento já baixamos uma `imagem` e executamos um `container`.

##Listando as imagens disponíveis localmente.
```
docker image ls
```
Resultado será algo assim:

```
REPOSITORY                                   TAG                  IMAGE ID       CREATED         SIZE
hello-world                                  latest               feb5d9fea6a5   10 months ago   13.3kB
```

##Listando os containers executados e em execução

```
docker container ls -a
```
Resultado será algo assim:

```
CONTAINER ID   IMAGE             COMMAND       NAMES                     CREATED           STATUS                           PORTS                                                                          
fade541f5daa   hello-world       "/hello"      friendly_grothendieck     13 minutes ago    Exited (0) 13 minutes ago             
```

Observe que o nome do container ficou um nome randômico, criado pelo próprio docker, veja coluna NAMES friendly_grothendieck.

Vamos gerar um container com um nome personalizado.

```
docker run --name  my-hello-world hello-world
```

Veja o resultado:

```
CONTAINER ID   IMAGE             COMMAND       NAMES                     CREATED           STATUS                           PORTS                                                                          
6cae2c92417c   hello-world       "/hello"      my-hello-world            7 seconds ago     Exited (0) 7 seconds ago
fade541f5daa   hello-world       "/hello"      friendly_grothendieck     13 minutes ago    Exited (0) 13 minutes ago  
```

No exemplo acima, nosso container executou e morreu.

Agora vamos executar um container de forma interativa usaremos o parâmetrp -it, ou seja acessaramos nosso container para executar comandos.

```
docker container run -it ubuntu /bin/bash
```

Exemplo:

```
root@72569606bdc5:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@72569606bdc5:/# ls tmp/ 
root@72569606bdc5:/# echo 'estamos no container' > tmp/my-file.txt
root@72569606bdc5:/# cat tmp/my-file.txt 
estamos no container
root@72569606bdc5:/# 
```

No exemlo, acessamos o container e criamos o arquivo my-file.txt 

Neste momento nosso container ainda esta em execução, mas quando sairmos do moto interativo nosso container morrerá e junto com ele nosso arquivo recém criado.

Agora vamos manter o `container` em execução utilizando uma combinação de parâmetros -i -d

```
docker run -i -d ubuntu
```

Se executar o comando `docker container ls` verá nosso container em execução.

## Acessando um container em execução.

Utilize o container_id ou o container_name
```
docker container exec -it bd3e21f27979 /bin/bash
```
#Parando um container
#Deletando um container
#Deletando uma imagem
#Criando uma imagem



#Kubernetes
#Terraform
