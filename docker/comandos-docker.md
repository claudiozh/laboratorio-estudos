---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# ⌨ Comandos docker

### **Comandos relacionados às informações**

<details>

<summary>Exibe a versão do docker que está instalada</summary>

{% code overflow="wrap" %}
```docker
docker version
```
{% endcode %}

</details>

<details>

<summary>Retorna diversas informações sobre o container</summary>

{% code overflow="wrap" %}
```docker
docker inspect ID_CONTAINER
```
{% endcode %}

</details>

<details>

<summary>Exibe todos os containers em execução no momento</summary>

{% code overflow="wrap" %}
```
docker ps 
```
{% endcode %}

</details>

<details>

<summary>Exibe todos os containers, independentemente de estarem em execução ou não</summary>

{% code overflow="wrap" %}
```
docker ps -a
```
{% endcode %}

</details>

<details>

<summary>Exibe todas as image none</summary>

{% code overflow="wrap" %}
```
docker image ls -f 'dangling=true'
```
{% endcode %}

</details>

<details>

<summary>Exibe os logs do container e limita a quantidade de linhas a exibir.</summary>

{% code overflow="wrap" %}
```
docker logs -f NOME_CONTAINER --tail NUMERO_DE_LINHA_LOGS
```
{% endcode %}

</details>

### **Comandos relacionados à execução**

* `docker run NOME_DA_IMAGEM` - cria um container com a respectiva imagem passada como parâmetro.
* `docker run -it NOME_DA_IMAGEM` - entra dentro da imagem buildada.
* `docker run -d -P --name NOME dockersamples/static-site` - ao executar, dá um nome ao container e define uma porta aleatória.
* `docker run -d -p 12345:80 dockersamples/static-site` - define uma porta específica para ser atribuída à porta 80 do container, neste caso 12345.
* `docker run -v "CAMINHO_VOLUME" NOME_DA_IMAGEM` - cria um volume no respectivo caminho do container.
* `docker run -it --name NOME_CONTAINER --network NOME_DA_REDE NOME_IMAGEM` - cria um container especificando seu nome e qual rede deverá ser usada.
* `docker run --rm -it -v $(pwd)/pasta:/volume-container NOME_IMAGEM` - Roda uma imagem e compartilha o volume.
* `--rm` - A flag --rm é para quando a aplicação parar o container ser removido.
* `-d` - A flag -b usada para rodar o container em modo detashed, ou seja ao rodar `docker run -d -p 80:80 nginx` por exemplo, o terminal que você rodar esse comando não ficará preso
* `-t` - A flag -t é para nomear a sua imagem com base no seu dockerfile.
* `docker build -t TAG_DA_SUA_IMAGEM PATH_DOCKERFILE`
  * Ex.: docker build -t claudiozh/nginx-com-vim .

### **Comandos relacionados à inicialização/interrupção**

* `docker start ID_CONTAINER` - inicia o container com id em questão.
* `docker start -a -i ID_CONTAINER` - inicia o container com id em questão e integra os terminais, além de permitir interação entre ambos.
* `docker stop ID_CONTAINER` - interrompe o container com id em questão.
* `docker stop $(docker ps -aq)` - para todos os containers
* `docker builder prune` - limpa o cache do builder

### **Comandos relacionados à remoção**

* `docker rm ID_CONTAINER` - remove o container com id em questão.
* `docker container prune` - remove todos os containers que estão parados.
* `docker rmi NOME_DA_IMAGEM` - remove a imagem passada como parâmetro.
* `docker ps -q` - exibe somente os ids dos containers
* `docker stop $(docker ps -q)` - para todos os containers.
* `docker image rm $(docker image ls -f 'dangling=true' -q)` - remove tosdas as imagens none
* `docker rmi $(docker images -a -q) -f` - remove todas as imagens
* `docker rmi $(docker images --filter "dangling=true" -q --no-trunc)` - remove imagens none
* `docker rm $(docker ps -aq)` remove todos os containers

### **Comandos relacionados ao Docker Hub**

* `docker login` - inicia o processo de login no Docker Hub.
* `docker push NOME_USUARIO/NOME_IMAGEM` - envia a imagem criada para o Docker Hub.
* `docker pull NOME_USUARIO/NOME_IMAGEM` - baixa a imagem desejada do Docker Hub

### **Comandos relacionados à rede**

* `hostname -i` - mostra o ip atribuído ao container pelo docker (funciona apenas dentro do container).
* `docker network create --driver bridge NOME_DA_REDE` - cria uma rede especificando o driver desejado.

### **Comandos relacionados ao docker-compose**

* `docker-compose build` - Realiza o build dos serviços relacionados ao arquivo docker-compose.yml, assim como verifica a sua sintaxe.
* `docker-compose up` - Sobe todos os containers relacionados ao docker-compose, desde que o build já tenha sido executado.
* `docker-compose down` - Para todos os serviços em execução que estejam relacionados ao arquivo docker-compose.yml.

### **Outros**

* `chown -Rf 1000:1000 NOME_DIRETORIO/` - Dá permissão de escrita e leitura dentro do container
