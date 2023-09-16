---
description: Instalar Docker e Docker Compose no Ubuntu 20.04
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Instalação docker

### **Passo 1: Instale o Docker**

Digite o seguinte comando para instalar o Docker no seu sistema:

```bash
sudo apt install docker.io
```

Isso instalará o Docker e suas dependências.

**Passo 2: Inicie e habilite o serviço Docker**

Para iniciar o serviço Docker e garantir que ele seja iniciado automaticamente no boot, use os seguintes comandos:

```bash
sudo systemctl enable --now docker
```

Isso iniciará o serviço Docker e o habilitará para inicialização automática.

**Passo 3: Adicione seu usuário ao grupo Docker**

Para executar comandos Docker sem a necessidade de usar `sudo`, adicione seu usuário atual ao grupo Docker. Substitua "SOMEUSERNAME" pelo seu nome de usuário:

```bash
sudo usermod -aG docker SOMEUSERNAME
```

Certifique-se de fazer logout e login novamente ou reiniciar o sistema para que as alterações entrem em vigor.

**Passo 4: Verifique a instalação do Docker**

Para verificar se o Docker foi instalado corretamente e verificar sua versão, execute o seguinte comando:

```bash
docker --version
```

Isso deve exibir a versão do Docker que foi instalada no seu sistema.

**Passo 5: Instale o Docker Compose**

Digite os seguintes comandos para baixar e instalar o Docker Compose:

{% code overflow="wrap" %}
```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.25.5/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```
{% endcode %}

```sh
sudo chmod +x /usr/local/bin/docker-compose
```

Isso baixará o Docker Compose e o tornará executável.

**Passo 6: Verifique a instalação do Docker Compose**

Para verificar se o Docker Compose foi instalado corretamente e verificar sua versão, execute o seguinte comando:

```bash
docker-compose --version
```

Isso deve exibir a versão do Docker Compose que foi instalada no seu sistema.

Agora você instalou com sucesso o Docker e o Docker Compose no seu sistema Linux e está pronto para começar a usar essas poderosas ferramentas para criar e gerenciar contêineres.
