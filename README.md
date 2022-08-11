# INICIATIVA-DEVOPS
Este documento é apenas um <b>key notes</b> particular  referente aos assuntos de `docker`, `kubernetes` e `terraform` abordados na semana da iniciativadeveops2022. Ofertado por [Fabricio Veronez](https://www.youtube.com/user/fabricioveronez).
<hr>

## CONTÉUDO
### [DOCKER](#DOCKER)
### KUBERNETES
### TERRAFORM
<hr>

# DOCKER
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

Se executar o comando `docker container ls` vera nosso container em execução.

## Acessando um container em execução.

Utilize o container_id ou o container_name
```
docker container exec -it bd3e21f27979 /bin/bash
```

## Parando um container em execução

```
docker container stop afec6e8f006f
```
## Deletando um container

```
docker container rm 74dee8cb3579
```

## Deletando uma imagem

```
docker image rm df5de72bdb3b

--------Result---------------------------------------------------------------------------
docker image rm df5de72bdb3b
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:34fea4f31bf187bc915536831fd0afc9d214755bf700b5cdb1336c82516d154e
Deleted: sha256:df5de72bdb3b711aba4eca685b1f42c722cc8a1837ed3fbd548a9282af2d836d
```

> Ao remover uma imagem é possível que o docker informe que não conseguiu deletar devido haver containers associados a imagem.
> ``` 
> docker image rm df5de72bdb3b
> Error response from daemon: conflict: unable to delete df5de72bdb3b (cannot be forced) - image is being used by running container afec6e8f006f
> ```
> Antes da remoção da imagem remova primeiramente os container associados.
> Identifique o id da sua imagem e então filtre os containers gerados apartir desta imagem. Veja exemplo abaixo.

## Filtrando containers oriundos de uma imagem

```
docker container ls -a --filter ancestor=df5de72bdb3b
CONTAINER ID   IMAGE     COMMAND                CREATED          STATUS                        PORTS     NAMES
afec6e8f006f   ubuntu    "bash"                 22 minutes ago   Exited (137) 14 minutes ago             compassionate_ellis
26da763f504e   ubuntu    "-i -d"                22 minutes ago   Created                                 festive_allen
2bdf097e2567   ubuntu    "-i -t /bin/bash"      22 minutes ago   Created                                 laughing_ramanujan
39b398c43080   ubuntu    "-i -t /bin/bash"      23 minutes ago   Created                                 quizzical_dijkstra
bd3e21f27979   ubuntu    "bash"                 23 hours ago     Exited (137) 20 hours ago               hungry_kapitsa
7a1b916b3dee   ubuntu    "/bin/bash /bin/cat"   23 hours ago     Created                                 relaxed_mendeleev
77b061fd4644   ubuntu    "/bin/bash cat"        23 hours ago     Created                                 lucid_rubin
36b7cf860d27   ubuntu    "/bin/bash"            23 hours ago     Exited (0) 23 hours ago                 determined_goldstine
de021e9f5af9   ubuntu    "bash"                 23 hours ago     Exited (0) 23 hours ago                 blissful_mayer
72569606bdc5   ubuntu    "/bin/bash"            23 hours ago     Exited (0) 23 hours ago                 nice_bose
97a9ff625e95   ubuntu    "/bin/bash"            4 days ago       Exited (0) 4 days ago                   amazing_bhabha
e1efc3ee468e   ubuntu    "/bin/bash"            5 days ago       Exited (127) 5 days ago                 condescending_ptolemy
3fdc2da7741f   ubuntu    "/bin/bash"            5 days ago       Exited (0) 5 days ago                   thirsty_jepsen

```

## Retornando apenas o `container id` 

```
docker container ls -a -q --filter ancestor=df5de72bdb3b
afec6e8f006f
26da763f504e
2bdf097e2567
39b398c43080
bd3e21f27979
7a1b916b3dee
77b061fd4644
36b7cf860d27
de021e9f5af9
72569606bdc5
97a9ff625e95
e1efc3ee468e
3fdc2da7741f
```

## Removendo todos os containers associados a uma imagem

```
docker container rm $(docker container ls -a -q --filter ancestor=df5de72bdb3b)
afec6e8f006f
26da763f504e
2bdf097e2567
39b398c43080
bd3e21f27979
7a1b916b3dee
77b061fd4644
36b7cf860d27
de021e9f5af9
72569606bdc5
97a9ff625e95
e1efc3ee468e
3fdc2da7741f
```
## Criando nossas próprias imagens
Vamos criar uma imagem a partir do ubuntu com o curl já instalado para isso criaremos uma arquivo chamado Dockerfile, este arquivo contêm as instruções para a criação de uma imagem.

>#### Dockerfile
>Um Dockerfile é um documento de texto que contém todos os comandos que um usuário pode chamar na linha de comando para montar uma imagem

Conteúdo do Dockerfile

```
FROM ubuntu
RUN apt update && \
    apt install curl --yes
```
A instrução `FROM` indica a imagem de origem, neste caso o ubuntu e `RUN` os comandos que serão executados atualização do ubuntu `apt update` e instalação do curl `apt install curl`.

Com o arquivo Dockerfile criado já podemos criar nossa imagem, mas primeiramente e não obrigatoriamente crie uma conta no [Docker hub](https://hub.docker.com/) para podermos subir nossa imagem e utiliza-la em qualquer lugar.

Com a conta já criada faça o login com o comando:
```
docker login
```
Com o login do docker no terminal realizado, no diretório onde se encontra o Dockerfile informe os comandos conforme exemplo, não esqueça o contexto o ponto (.):
Exemplo : `docker build -t <seu-namespace>/<nome-da-imagem>:<tag> .`
```
docker build -t dleal/my-ubuntu-curl:v1 .
```

Após a conclusão consulta sua imagem localmente pelo comando `docker image ls`.

## Publicando nossa imaagem

```
docker push dleal/my-ubuntu-curl:v1
```
Progresso do envio da imagem
```
docker push dleal/my-ubuntu-curl:v1
The push refers to repository [docker.io/dleal/my-ubuntu-curl]
5e1665c860f4: Pushing [==========================>                        ]  22.59MB/42.62MB
629d9dbab5ed: Mounted from dleal/ubuntu-curl 
```

Após subir a v1 da nossa imagem é uma boa prática gerarmos a `latest` para execute os seguintes comandos.

```
docker tag dleal/my-ubuntu-curl:v1 dleal/my-ubuntu-curl:latest
```
Se consultar as imagens agora haverá a :v1 e a :latest

Subindo a imagem :latest.

```
docker push dleal/my-ubuntu-curl:latest
```
## Removendo nossa imagem recem criada

```
docker image rm -f 3c003731cf87
```
*Foi usado o parâmetro -f (force) porque a imagem tava sendo referẽnciado em mais de um repositório o da v1 e da latest

## Utilizando nossa imagem

```
docker run -d -i dleal/my-ubuntu-curl:v1
```

#Kubernetes

#Terraform
