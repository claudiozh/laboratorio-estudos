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

# 🎒 Curso docker

### Desvantagens das máquinas virtuais <a href="#desvantagens-das-maquinas-virtuais" id="desvantagens-das-maquinas-virtuais"></a>

* Toda máquina virtual precisa de um sistema operacional.
* Desperdicio de recursos da máquina.
* Configuração e atualização dos sistemas operacionais de cada máquina.
* Custo de luz e rede, e o seu alto tempo de ociosidade.

### Containers <a href="#containers" id="containers"></a>

Um container funcionará junto do nosso sistema operacional base, e conterá a nossa aplicação, ou seja, a aplicação será executada dentro dele. Criamos um container para cada aplicação, e esses containers vão dividir as funcionalidades do sistema operacional:

![](https://i.imgur.com/3NfrGnB.png)

Não temos mais um sistema operacional para cada aplicação, já que agora as aplicações estão dividindo o mesmo sistema operacional, que está em cima do nosso hardware. Os próprios containers terão a lógica que se encarregará dessa divisão.

Assim, com somente um sistema operacional, reduzimos os custos de manutenção e de infraestrutura como um todo.

### Vantagens de um container <a href="#vantagens-de-um-container" id="vantagens-de-um-container"></a>

Por não possuir um sistema operacional, o container é muito mais leve e não possui o custo de manter múltiplos sistemas operacionais, já que só teremos um sistema operacional, que será dividido entre os containers.

### Necessidade do uso de containers <a href="#necessidade-do-uso-de-containers" id="necessidade-do-uso-de-containers"></a>

Mas por que precisamos dos containers, não podemos simplesmente instalar as aplicações no nosso próprio sistema operacional? Até por que já fazemos isso, já que no nosso sistema operacional temos um editor de texto, terminal, navegador, etc.

No caso das nossas aplicações, essa abordagem pode ter alguns problemas. Por exemplo, se dois aplicativos precisarem utilizar a mesma porta de rede? Precisaremos de algo para isolar uma aplicação da outra. E se uma aplicação consumir toda a CPU, a ponto de prejudicar o funcionamento das outras aplicações? Isso acontece se não limitarmos a aplicação. Outro problema que pode ocorrer é cada aplicação precisar de uma versão específica de uma linguagem, por exemplo, uma aplicação precisa do Java 7 e outra do Java 8. Além disso, uma aplicação pode acabar congelando todo o sistema. Por isso é bom ter essa separação das aplicações, isolar uma da outra, e isso pode ser feito com os **containers**.

Com os containers, conseguimos limitar o consumo de CPU das aplicações, melhorando o controle sobre o uso de cada recurso do nosso sistema (CPU, rede, etc). Também temos uma facilidade maior em trabalhar com versões específicas de linguagens/bibliotecas, além de ter uma agilidade maior na hora de criar e subir containers, já que eles são mais leves que as máquinas virtuais.

### Docker, Inc. <a href="#docker-inc" id="docker-inc"></a>

Primeiramente, devemos falar sobre a **Docker**, Inc., que no início era chamada de dotCloud. A dotCloud era uma empresa de **PaaS** (Platform as a Service), sendo responsável pela hospedagem da nossa aplicação, levantando o servidor, configurando-o, liberando portas, etc, fazendo tudo o que é necessário para subir a nossa aplicação. Outros exemplos de empresas de **PaaS** são o **Heroku**, **Microsoft Azure** e **Google Cloud Platform**.

Inicialmente, para prover a parte de infraestrutura, a dotCloud utilizava o Amazon Web Services (AWS), serviço que nos disponibiliza máquinas virtuais e físicas para trabalharmos. E para hospedar uma aplicação, sabemos que precisamos do sistema operacional, mas a dotCloud introduziu o conceito de containers na hora de subir uma aplicação, dando origem ao Docker, tecnologia utilizada para baratear o custo de hospedar várias aplicações em uma mesma máquina.

Ou seja, quando a dotCloud criou o Docker, sua intenção era economizar os gastos da empresa, subindo várias aplicações em containers, em um mesmo hardware do AWS, e com o passar do tempo a empresa percebeu que tinham muitos desenvolvedores interessados na tecnologia que ela havia criado, a tecnologia que permite a criação de containers, que faz o intermédio entre eles e o sistema operacional, o Docker.

### As tecnologias do Docker <a href="#as-tecnologias-do-docker" id="as-tecnologias-do-docker"></a>

O **Docker** nada mais é do que uma coleção de tecnologias para facilitar o deploy e a execução das nossas aplicações. A sua principal tecnologia é a **Docker Engine**, a plataforma que segura os containers, fazendo o intermédio entre o sistema operacional.

Outras tecnologias do Docker que facilitam a nossa vida e que veremos neste curso são o **Docker Compose**, um jeito fácil de definir e orquestrar múltiplos containers; o **Docker Swarm**, uma ferramenta para colocar múltiplos docker engines para trabalharem juntos em um cluster; o **Docker Hub**, um repositório com mais de 250 mil imagens diferentes para os nossos containers; e a **Docker Machine**, uma ferramenta que nos permite gerenciar o Docker em um host virtual.

### Volumes <a href="#volumes" id="volumes"></a>

Quando escrevemos em um container, assim que ele for removido, os dados também serão. Mas podemos criar um local especial dentro dele, e especificamos que esse local será o nosso volume de dados.

Quando criamos um volume de dados, o que estamos fazendo é apontá-lo para uma pequena pasta no Docker Host. Então, quando criamos um volume, criamos uma pasta dentro do container, e o que escrevermos dentro dessa pasta na verdade estaremos escrevendo do Docker Host.

Isso faz com que não percamos os nossos dados, pois o container até pode ser removido, mas a pasta no Docker Host ficará intacta.

Um volume fica salvo no **Docker Host**. Ou seja, fica salvo no computador onde a Docker Engine está rodando.

### Montando o Dockerfile <a href="#montando-o-dockerfile" id="montando-o-dockerfile"></a>

> Geralmente, montamos as nossas imagens a partir de uma imagem já existente. Nós podemos criar uma imagem do zero, mas a prática de utilizar uma imagem como base e adicionar nela o que quisermos é mais comum. Para dizer a imagem-base que queremos, utilizamos a palavra FROM mais o nome da imagem.

Como o nosso projeto precisa do Node.js, vamos utilizar a sua imagem:

* Exemplo Dockerfile

```
FROM node:latest
MAINTAINER Claudio Rodrigo
COPY . /app
WORKDIR /app
RUN npm install
ENTRYPOINT npm start
EXPOSE 3000
```

* Criando imagem com base no Dockerfile

> Para criar a imagem, precisamos fazer o seu build através do comando docker build, comando utilizado para buildar uma imagem a partir de um Dockerfile. Para configurar esse comando, passamos o nome do Dockerfile através da flag -f:

```
docker build -f Dockerfile -t claudiorodrigo/node .
```

### Criando um container a partir da nossa imagem <a href="#criando-um-container-a-partir-da-nossa-imagem" id="criando-um-container-a-partir-da-nossa-imagem"></a>

Agora que já temos a imagem criada, podemos criar um container a partir dela:

```
docker run -d -p 8080:3000 claudiorodrigo/node
```

### Redes com Docker <a href="#redes-com-docker" id="redes-com-docker"></a>

A boa notícia é que no Docker, por padrão, já existe uma default network. Isso significa que, quando criamos os nossos containers, por padrão eles funcionam na mesma rede:

![](https://i.imgur.com/kFFNaNk.png)

Para verificar isso, vamos subir um container com Ubuntu:

{% code overflow="wrap" %}
```sh
docker run -it ubuntu
```
{% endcode %}

Em outro terminal, vamos verificar o id desse container através do comando docker ps, e com ele em mãos, vamos passá-lo para o comando docker inspect. Na saída desse comando, em NetworkSettings, vemos que o container está na rede padrão bridge, rede em que ficam todos os containers que criamos.

Voltando ao terminal do container, se executarmos o comando hostname -i vemos o IP atribuído a ele pela rede local do Docker:

{% code overflow="wrap" %}
```sh
hostname -i
```
{% endcode %}

Então, dentro dessa rede local, os containers podem se comunicar através desses IPs. Para comprovar isso, vamos deixar esse container rodando e criar um novo:

{% code overflow="wrap" %}
```sh
docker run -it ubuntu
```
{% endcode %}

E vamos verificar o seu IP:

{% code overflow="wrap" %}
```sh
hostname -i
```
{% endcode %}

Agora, no primeiro container, vamos instalar o pacote iputils-ping para podermos executar o comando ping para verificar a comunicação entre os containers:\`\`

{% code overflow="wrap" %}
```sh
apt-get update && apt-get install iputils-ping
```
{% endcode %}

Após o término da instalação, executamos o comando ping, passando para ele o IP do segundo container. Para interromper o comando, utilizamos o atalho CTRL + C:

{% code overflow="wrap" %}
```sh
ping ENDERECO_IP
```
{% endcode %}

Assim, podemos ver que os containers estão conseguindo se comunicar entre eles.

### Comunicação entre containers utilizando os seus nomes <a href="#comunicacao-entre-containers-utilizando-os-seus-nomes" id="comunicacao-entre-containers-utilizando-os-seus-nomes"></a>

Então, o Docker criar uma rede virtual, em que todos os containers fazem parte dela, com os IPs automaticamente atribuídos. Mas quando os IPs são atribuídos, cada hora em que subirmos um container, ele irá receber um IP novo, que será determinado pelo Docker. Logo, se não sabemos qual o IP que será atribuído, isso não é muito útil quando queremos fazer a comunicação entre os containers. Por exemplo, podemos querer colocar dentro do aplicativo o endereço exato do banco de dados, e para saber exatamente o endereço do banco de dados, devemos configurar um nome para aquele container.

Mas nomear um container nós já sabemos, basta adicionar o --name, passando o nome que queremos na hora da criação do container, certo? Apesar de conseguirmos dar um nome a um container, a rede do Docker não permite com que atribuamos um hostname a um container, diferentemente de quando criamos a nossa própria rede.

Na rede padrão do Docker, só podemos realizar a comunicação utilizando IPs, mas se criarmos a nossa própria rede, podemos “batizar” os nossos containers, e realizar a comunicação entre eles utilizando os seus nomes:

![](https://i.imgur.com/xE49BSI.png)

Isso não pode ser feito na rede padrão do Docker, somente quando criamos a nossa própria rede.

### Criando a nossa própria rede do Docker <a href="#criando-a-nossa-propria-rede-do-docker" id="criando-a-nossa-propria-rede-do-docker"></a>

Então, vamos criar a nossa própria rede, através do comando docker network create, mas não é só isso, para esse comando também precisamos dizer qual driver vamos utilizar. Para o padrão que vimos, de ter uma nuvem e os containers compartilhando a rede, devemos utilizar o driver de bridge.

Especificamos o driver através do --driver e após isso nós dizemos o nome da rede. Um exemplo do comando é o seguinte:

{% code overflow="wrap" %}
```sh
docker network create --driver bridge minha-rede
```
{% endcode %}

Agora, quando criamos um container, ao invés de deixarmos ele ser associado à rede padrão do Docker, atrelamos à rede que acabamos de criar, através da flag --network. Vamos aproveitar e nomear o container:

{% code overflow="wrap" %}
```sh
docker run -it --name meu-container-de-ubuntu --network minha-rede ubuntu
```
{% endcode %}

Agora, se executarmos o comando docker inspect meu-container-de-ubuntu, podemos ver em NetworkSettings o container está na rede minha-rede. E para testar a comunicação entre os containers na nossa rede, vamos abrir outro terminal e criar um segundo container:

{% code overflow="wrap" %}
```sh
docker run -it --name segundo-ubuntu --network minha-rede ubuntu
```
{% endcode %}

Agora, no segundo-ubuntu, instalamos o ping e testamos a comunicação com o meu-container-de-ubuntu:

{% code overflow="wrap" %}
```sh
ping meu-container-de-ubuntu
```
{% endcode %}

Conseguimos realizar a comunicação entre os containers utilizando somente os seus nomes. É como se o Docker Host, o ambiente que está rodando os containers, criasse uma rede local chamada minha-rede, e o nome do container será utilizado como se fosse um hostname.

Mas lembrando que só conseguimos fazer isso em redes próprias, redes que criamos, isso não é possível na rede padrão dos containers.

### Funcionamento das aplicações em geral <a href="#funcionamento-das-aplicacoes-em-geral" id="funcionamento-das-aplicacoes-em-geral"></a>

Na vida real, sabemos que a aplicação é maior que somente dois containers, geralmente temos dois, três ou mais containers para segurar o tráfego da aplicação, distribuindo a carga. Além disso, temos que colocar todos esses containers para se comunicar com o banco de dados em um outro container, mas quanto maior a aplicação, devemos ter mais de um container para o banco também.

E claro, se temos três aplicações rodando, não podemos ter três endereços diferentes, então nesses casos utilizamos um Load Balancer em um outro container, para fazer a distribuição de carga quando tivermos muitos acessos. Ele recebe as requisições e distribui para uma das aplicações, e ele também é muito utilizado para servir os arquivos estáticos, como imagens, arquivos CSS e JavaScript. Assim, a nossa aplicação controla somente a lógica, as regras de negócio, com os arquivos estáticos ficando a cargo do Load Balancer:

![](https://i.imgur.com/c15FC1V.png)

Se formos seguir esse diagrama, teríamos que criar cinco containers na mão, e claro, cada container com configurações e flags diferentes, além de termos que nos preocupar com a ordem em que vamos subi-los.

### Docker Compose <a href="#docker-compose" id="docker-compose"></a>

Ao invés de subir todos esses containers na mão, o que vamos fazer é utilizar uma tecnologia aliada do Docker, chamada Docker Compose, feito para nos auxiliar a orquestrar melhor múltiplos containers. Ele funciona seguindo um arquivo de texto YAML (extensão .yml), e nele nós descrevemos tudo o que queremos que aconteça para subir a nossa aplicação, todo o nosso processo de build, isto é, subir o banco, os containers das aplicações, etc.

Assim, não precisamos ficar executando muitos comandos no terminal sem necessidade. E esse será o foco desta aula, montar uma aplicação na estrutura descrita anteriormente na imagem, que é uma situação comum no nosso dia-a-dia.

> Exemplo docker-compose

{% code overflow="wrap" %}
```yaml
version: '3'
services:
    nginx:
        build:
            dockerfile: ./docker/nginx.dockerfile
            context: .
        image: douglasq/nginx
        container_name: nginx
        ports:
            - "80:80"
        networks: 
            - production-network
        depends_on: 
            - "node1"
            - "node2"
            - "node3"

    mongodb:
        image: mongo
        networks: 
            - production-network

    node1:
        build:
            dockerfile: ./docker/alura-books.dockerfile
            context: .
        image: douglasq/alura-books
        container_name: alura-books-1
        ports:
            - "3000"
        networks: 
            - production-network
        depends_on:
            - "mongodb"

    node2:
        build:
            dockerfile: ./docker/alura-books.dockerfile
            context: .
        image: douglasq/alura-books
        container_name: alura-books-2
        ports:
            - "3000"
        networks: 
            - production-network
        depends_on:
            - "mongodb"

    node3:
        build:
            dockerfile: ./docker/alura-books.dockerfile
            context: .
        image: douglasq/alura-books
        container_name: alura-books-3
        ports:
            - "3000"
        networks: 
            - production-network
        depends_on:
            - "mongodb"

networks: 
    production-network:
        driver: bridge
```
{% endcode %}

### Instalando Docker Compose

O Docker Compose não é instalado por padrão no Linux, então você deve instalá-lo por fora. Para tal, baixe-o na sua versão mais atual, que pode ser visualizada no seu GitHub, executando o comando abaixo:

{% code overflow="wrap" %}
```shell
sudo curl -L https://github.com/docker/compose/releases/download/1.15.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```
{% endcode %}

Após isso, dê permissão de execução para o docker-compose:

{% code overflow="wrap" %}
```shell
sudo chmod +x /usr/local/bin/docker-compose
```
{% endcode %}
