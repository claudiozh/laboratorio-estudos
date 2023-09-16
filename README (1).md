# Deploy com docker e prisma para servidor remoto sem usar registrador de imagens

Nesta abordagem de implantação, você aprenderá como fazer o deploy de uma aplicação utilizando Docker e Prisma em um servidor remoto, sem depender de um registrador de imagens.&#x20;

Em vez disso, você utilizará o Docker para empacotar sua aplicação e todas as suas dependências, incluindo o Prisma, em um contêiner. Em seguida, você transferirá esse contêiner para o servidor remoto e o executará lá.

&#x20;Essa abordagem oferece flexibilidade e controle sobre o processo de implantação, permitindo que você implante sua aplicação de forma eficiente e confiável, mesmo sem um registro de imagens.&#x20;

Ao seguir este método, você poderá implantar facilmente sua aplicação Node.js, garantindo que o Prisma e todas as outras dependências estejam configuradas corretamente no servidor remoto.

## Crie seu arquivo de configurações para produção

Este é um arquivo de implantação em várias etapas, onde inicialmente uma máquina mais poderosa é criada para executar o processo de construção da aplicação. Em seguida, uma máquina mais leve é configurada exclusivamente para a criação da imagem Docker.

{% code title="src/Dockerfile.prod" overflow="wrap" fullWidth="false" %}
```docker

FROM node:18.16.0-alpine as builder
# Estágio de construção da aplicação

ENV NODE_ENV build
# Define a variável de ambiente NODE_ENV como "build" para o estágio atual

WORKDIR /home/node/app
# Define o diretório de trabalho dentro do contêiner como "/home/node/app"

COPY . .
# Copia todo o conteúdo do diretório local para o diretório de trabalho no contêiner

RUN npm ci && npm run build && npm prune --production
# Executa os comandos npm no contêiner para instalar as dependências, executar o processo de construção e remover as dependências de desenvolvimento

######## Start a new stage from scratch #######

FROM node:18.16.0-alpine
# Novo estágio baseado em uma imagem Node.js vazia

ENV NODE_ENV production
# Define a variável de ambiente NODE_ENV como "production" para o novo estágio

ENV TZ America/Fortaleza
# Define a variável de ambiente TZ como "America/Fortaleza" para o fuso horário

ENV PORT 3000
# Define a variável de ambiente PORT como 3000 para a porta de execução da aplicação

WORKDIR /home/node/app
# Define o diretório de trabalho dentro do contêiner como "/home/node/app"

# Copia o arquivo binário pré-construído do estágio anterior
COPY --from=builder /home/node/app/prisma ./prisma
COPY --from=builder /home/node/app/dist ./dist
COPY --from=builder /home/node/app/package.json .
COPY --from=builder /home/node/app/package-lock.json .
COPY --from=builder /home/node/app/node_modules/ /home/node/app/node_modules/

RUN apk add --no-cache ca-certificates tzdata && chown -R node:node /home/node/app
# Executa os comandos apk no contêiner para adicionar certificados, configurar o fuso horário e atribuir permissões corretas para o diretório

USER node
# Define o usuário do contêiner como "node"

CMD [ "node", "dist/server" ]
# Comando para executar o arquivo executável da aplicação
```
{% endcode %}

## Crie seu arquivo de omissão de conteúdo

Esse arquivo serve para informar ao docker o que deve ser ignorado na hora do build.

{% code title="src/.dockerignore" %}
```bash
/node_modules
.github
.docker
deploy
```
{% endcode %}

## Crie um arquivo de deploy

Este arquivo contém comandos para executar a construção da imagem, juntamente com a adição da pasta "releases".

{% code title="src/deploy/deploy.sh" %}
```bash

#!/bin/bash

# Definir uma variavel local com o caminho absoluto do projeto
APP_DIR=/home/claudio/projetos/app-nodejs

# Entra dentro da pasta principal do projeto
cd $APP_DIR

# Criar a imagem local da aplicação
docker build -f Dockerfile.prod -t claudiozh/app-nodejs:latest .

# Cria pasta releases dentro do diretório deploy
mkdir -p deploy/realeses

# Salva a imagem na máquina local e cria um .zip da mesma
docker save claudiozh/app-nodejs:latest | gzip > deploy/realeses/app-nodejs.tar.gz
```
{% endcode %}

<details>

<summary>Como executar o arquivo deploy.sh</summary>

Acesse a pasta deploy

```bash
cd deploy
```

Adicionar permissão de execução no arquivo

```bash
sudo chmod +x deploy.sh
```

Execute o arquivo sh

```bash
./deploy.sh
```

</details>

{% hint style="info" %}
Apos a execução do arquivo sua imagem ira ficar disponível dentro da sua pasta "releases".
{% endhint %}

## Como enviar sua imagem para o servidor remoto

Após a disponibilidade da sua imagem, é necessário enviá-la para o servidor. Para otimizar o tráfego e garantir uma transferência eficiente, faremos a conversão da imagem para o formato .tar.gz.

```bash
docker save claudiozh/app-nodejs:latest | gzip > app-nodejs.tar.gz
```

Apos a conversão da imagem, iremos realizar o envio para o servidor via scp.

> Entre dentro da pasta releases
>
> ```bash
> cd deploy/releases
> ```

Para fazer o envio execute o seguinte comando

```bash
scp app-nodejs.tar.gz USER_REMOTE@IP_REMOTE:
```

Caso ocorra tudo certo, sua imagem estará disponível na pasta raiz do seu servidor, e agora é necessário descompactar novamente o arquivo para uma imagem utilizável.

```bash
docker load --input app-nodejs.tar.gz
```

Com isso sua imagem já esta disponível para utilização no servidor remoto.&#x20;

## Criando docker-compose.yml na máquina de produção e configurando servidor traefik

Crie uma pasta com o nome do seu projeto

```bash
mkdir minha-aplicacao && cd minha-aplicacao
```

Agora crie um pasta para armazenar os certificados SSL da sua aplicação que serão gerados automaticamente pelo traefik

```bash
mkdir lets-encrypt
```

> Não é necessário criar a pasta com esse mesmo que tem no exemplo, porém caso mude o nome será necessário alterar no docker-compose.yml

Crie o arquivo docker-compose.yml para realizar a orquestração dos seus contêineres, e mapear suas portas.

{% code title="docker-compose.yml" %}
```yaml

version: "3"

services:
  
  application:
    image: claudiozh/app-nodejs:latest  # Imagem do contêiner da aplicação
    container_name: app-nodejs  # Nome do contêiner
    labels:  # Rótulos para configuração do Traefik
      - "traefik.http.routers.my-api.rule=Host(`mydomain.com`)"  # Regra para roteamento do Traefik
      - "traefik.http.services.my-api.loadbalancer.server.port=3000"  # Porta do serviço no contêiner
      - "traefik.http.routers.my-api.tls=true"  # Habilitar TLS
      - "traefik.http.routers.my-api.tls.certresolver=myresolver"  # Resolvedor de certificados
    networks:
      - my-network  # Nome da rede Docker
    user: node  # Usuário do contêiner
    env_file:
      - .env  # Arquivo de variáveis de ambiente
    expose:
      - "3000"  # Porta exposta pela aplicação
  
  my-database:
    image: my-database-image  # Imagem do contêiner do banco de dados
    container_name: my-database  # Nome do contêiner
    ports:
      - 5432:5432  # Mapeamento de porta do host para o contêiner
    env_file:
      - .env  # Arquivo de variáveis de ambiente
    volumes:
      - data:/var/lib/postgres  # Volume para persistência de dados
    networks:
      - my-network  # Nome da rede Docker
  
  reverse-proxy:
    image: traefik:v2.9  # Imagem do contêiner do proxy reverso
    networks:
      - my-network  # Nome da rede Docker
    command:
      - "--api.insecure=true"  # Habilitar API insegura do Traefik
      - "--providers.docker"  # Provedor de configuração do Docker
      - "--entrypoints.web.address=:80"  # Endereço do ponto de entrada HTTP
      - "--entrypoints.websecure.address=:443"  # Endereço do ponto de entrada HTTPS
      - "--entrypoints.web.http.redirections.entryPoint.to=websecure"  # Redirecionamento de HTTP para HTTPS
      - "--entrypoints.web.http.redirections.entryPoint.scheme=https"  # Esquema de redirecionamento para HTTPS
      - "--entrypoints.web.http.redirections.entrypoint.permanent=true"  # Redirecionamento permanente
      - "--certificatesresolvers.myresolver.acme.email=myemail@example.com"  # E-mail para registro no ACME
      - "--certificatesresolvers.myresolver.acme.storage=/opt/lets-encrypt/acme.json"  # Armazenamento do ACME
      - "--certificatesresolvers.myresolver.acme.httpchallenge.entrypoint=web"  # Desafio HTTP para ACME
    ports:
      - "80:80"  # Mapeamento de porta HTTP
      - "443:443"  # Mapeamento de porta HTTPS
      - "8080:8080"  # Porta para painel de controle do Traefik
    volumes:
      - /PATH_PASTA_CRIADO_ARMAZENAR_CERTIFICADOS:/opt/lets-encrypt  # Volume para certificados
      - /var/run/docker.sock:/var/run/docker.sock  # Compartilhamento do socket do Docker

volumes:
  data:  # Volume para persistência de dados do banco de dados

networks:
  my_network:  # Rede Docker para comunicação entre os contêineres
    driver: bridgecompos
```
{% endcode %}



## Explicando docker-compose.yml

1. **application**: Este serviço representa a aplicação que você deseja executar em um contêiner. Ele usa uma imagem específica (claudiozh/app-nodejs:latest) e possui configurações para o Traefik, como rótulos para o roteamento, habilitação de TLS, etc.
2. **my-database**: Este serviço representa o banco de dados que você deseja usar. Ele usa uma imagem específica (my-database-image) e possui configurações de porta, variáveis de ambiente para autenticação e um volume para persistência de dados.
3. **reverse-proxy**: Este serviço é responsável por executar o proxy reverso Traefik. Ele lida com o roteamento de solicitações para a aplicação e lida com o SSL/TLS usando certificados emitidos pelo Let's Encrypt. Também possui um volume para armazenar os certificados e compartilha o socket do Docker para interagir com os contêineres.

Este código genérico pode ser usado como ponto de partida para configurar seus serviços em um ambiente Docker com o Traefik como proxy reverso, facilitando o roteamento e a configuração de SSL/TLS para sua aplicação. Certifique-se de substituir as imagens, nomes de contêiner, variáveis de ambiente e outras configurações específicas de acordo com suas necessidades.
