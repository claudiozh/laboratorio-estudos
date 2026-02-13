# ⌨️ Comandos docker

Um “cheat sheet” de comandos Docker para o dia a dia.

Use placeholders para adaptar rápido:

* `<CONTAINER_ID>` / `<CONTAINER_NAME>`
* `<IMAGE>` (ex.: `nginx`) e `<IMAGE:TAG>` (ex.: `nginx:alpine`)
* `<HOST_PORT>:<CONTAINER_PORT>` (ex.: `8080:80`)
* `<NETWORK>`
* `<PATH_LOCAL>:<PATH_CONTAINER>`

### Informações e inspeção

<details>

<summary>Ver versão do Docker</summary>

```bash
docker version
```

</details>

<details>

<summary>Listar containers em execução</summary>

```bash
docker ps
```

</details>

<details>

<summary>Listar todos os containers (inclusive parados)</summary>

```bash
docker ps -a
```

</details>

<details>

<summary>Inspecionar detalhes de um container</summary>

```bash
docker inspect <CONTAINER_ID>
```

</details>

<details>

<summary>Ver logs (seguindo) e limitar linhas</summary>

```bash
docker logs -f <CONTAINER_NAME> --tail <N>
```

</details>

### Rodar containers (`docker run`)

#### Exemplos comuns

*   Rodar um container:

    ```bash
    docker run <IMAGE:TAG>
    ```
*   Modo interativo (terminal):

    ```bash
    docker run -it <IMAGE:TAG>
    ```
*   Rodar em background (detached):

    ```bash
    docker run -d <IMAGE:TAG>
    ```
*   Dar nome ao container:

    ```bash
    docker run --name <CONTAINER_NAME> <IMAGE:TAG>
    ```
*   Publicar porta específica:

    ```bash
    docker run -p <HOST_PORT>:<CONTAINER_PORT> <IMAGE:TAG>
    ```
*   Publicar porta aleatória (para todas as `EXPOSE`):

    ```bash
    docker run -P <IMAGE:TAG>
    ```
*   Montar volume:

    ```bash
    docker run -v <PATH_LOCAL>:<PATH_CONTAINER> <IMAGE:TAG>
    ```
*   Usar rede específica:

    ```bash
    docker run --network <NETWORK> <IMAGE:TAG>
    ```
*   Remover o container ao parar:

    ```bash
    docker run --rm <IMAGE:TAG>
    ```

#### Flags mais usadas (resumo)

* `-d`: roda em background.
* `-i`: mantém STDIN aberto.
* `-t`: aloca um pseudo-TTY.
* `-it`: combinação comum para “entrar” no container.
* `--rm`: remove o container ao finalizar.
* `--name`: define nome do container.
* `-p`: mapeia porta host:container.
* `-P`: mapeia portas aleatórias (para portas expostas).
* `-v`: monta volume (host:container).
* `--network`: conecta em uma rede.

### Build de imagens (`docker build`)

```bash
docker build -t <IMAGE:TAG> .
```

Exemplo:

```bash
docker build -t claudiozh/nginx-com-vim:latest .
```

### Iniciar e parar containers

*   Iniciar:

    ```bash
    docker start <CONTAINER_ID>
    ```
*   Iniciar e anexar ao terminal:

    ```bash
    docker start -ai <CONTAINER_ID>
    ```
*   Parar:

    ```bash
    docker stop <CONTAINER_ID>
    ```
*   Parar todos:

    ```bash
    docker stop $(docker ps -q)
    ```

### Remover e limpar

*   Remover container:

    ```bash
    docker rm <CONTAINER_ID>
    ```
*   Remover todos os containers parados:

    ```bash
    docker container prune
    ```
*   Remover imagem:

    ```bash
    docker rmi <IMAGE:TAG>
    ```
*   Listar imagens “dangling” (`<none>`):

    ```bash
    docker image ls -f "dangling=true"
    ```
*   Remover imagens “dangling”:

    ```bash
    docker image rm $(docker image ls -f "dangling=true" -q)
    ```
*   Limpar cache do builder:

    ```bash
    docker builder prune
    ```

{% hint style="warning" %}
Os comandos abaixo são destrutivos. Confirme antes de rodar.
{% endhint %}

*   Remover **todas** as imagens:

    ```bash
    docker rmi -f $(docker images -aq)
    ```
*   Remover **todos** os containers:

    ```bash
    docker rm -f $(docker ps -aq)
    ```

### Docker Hub

*   Login:

    ```bash
    docker login
    ```
*   Push:

    ```bash
    docker push <USER>/<IMAGE:TAG>
    ```
*   Pull:

    ```bash
    docker pull <USER>/<IMAGE:TAG>
    ```

### Rede

*   Ver IP do container (dentro do container):

    ```bash
    hostname -i
    ```
*   Criar rede bridge:

    ```bash
    docker network create --driver bridge <NETWORK>
    ```

### Docker Compose

Se você usa o plugin novo, prefira `docker compose`. Se usa o binário legado, use `docker-compose`.

*   Build:

    ```bash
    docker compose build
    ```
*   Subir:

    ```bash
    docker compose up
    ```
*   Subir em background:

    ```bash
    docker compose up -d
    ```
*   Derrubar:

    ```bash
    docker compose down
    ```

### Manter apenas as N últimas imagens de um repositório

Remove imagens antigas de um repositório, mantendo as mais recentes.

{% code title="Exemplo (ajuste o +5 para o número desejado)" overflow="wrap" %}
```bash
docker images <REPO> --format "{{.ID}}" | tail -n +5 | xargs -r docker rmi
```
{% endcode %}

{% hint style="warning" %}
Revise o output de `docker images <REPO>` antes. A ordem pode variar por ambiente.
{% endhint %}

### Permissões (quando precisa ajustar dono/grupo)

```bash
chown -R 1000:1000 <DIR>/
```
