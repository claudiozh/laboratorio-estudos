---
description: >-
  Configure o acesso SSH de forma segura usando chaves públicas/privadas.
  Roteiro para gerar chaves, copiar a chave pública, desabilitar o acesso por
  senha e criar usuário não-root.
---

# 🎉 Configurando acesso SSH seguro com chaves públicas/privadas



## Tutorial: Configurando Acesso SSH com Chaves Públicas/Privadas

Este guia ensina a configurar o acesso SSH seguro usando autenticação por chaves. Ao final, você conseguirá acessar seu servidor sem senha (usando sua chave privada).

***

### ✅ Passo 1: Gerar o Par de Chaves SSH

Abra o terminal da sua máquina local e execute:

```bash
ssh-keygen -t rsa -b 4096 -f ~/.secrets/ssh-keys/tarelo-web
```

* Pressione **Enter** para aceitar o caminho e nome sugerido para os arquivos de chave.
* Será solicitada uma senha para proteger sua chave privada. Digite uma senha segura e pressione **Enter** (essa senha será pedida toda vez que usar essa chave).

> 🔒 **Dica**: As chaves serão salvas:
>
> * **Pública**: `~/.secrets/ssh-keys/tarelo-web.pub`
> * **Privada**: `~/.secrets/ssh-keys/tarelo-web`\
>   Proteja bem o arquivo da chave privada!

***

### ✅ Passo 2: Copiar a Chave Pública para o Servidor

Agora envie sua chave pública para o servidor com o comando abaixo:

```bash
ssh-copy-id -i ~/.secrets/ssh-keys/tarelo-web.pub evocorp@tarelo.site
```

* **Substitua** `evocorp@tarelo.site` pelo usuário e domínio/IP do seu servidor.
* Será solicitada a senha do usuário remoto (**não é a senha da chave**). Digite-a para autorizar a cópia da chave.

> 🔄 **Alternativa**:\
> Caso precise usar outro caminho para a chave pública:
>
> ```bash
> ssh-copy-id -i PATH_DO_ARQUIVO user@ip
> ```

***

### ✅ Passo 3: Conectar ao Servidor Usando a Chave Privada

Agora que a chave pública está no servidor, conecte-se sem precisar digitar a senha do servidor:

```bash
ssh -i ~/.secrets/ssh-keys/tarelo-web evocorp@tarelo.site
```

* Será solicitada a **senha da chave privada** que você definiu no Passo 1.
* Após isso, você estará logado no servidor!

***

### 🔒 Dicas Adicionais de Segurança

* **Desative o login por senha** no SSH no servidor, forçando uso apenas por chave:
  *   Edite o arquivo `/etc/ssh/sshd_config` e altere:

      ```bash
      PasswordAuthentication no
      ```
  *   Em seguida, reinicie o serviço SSH:

      ```bash
      sudo systemctl restart sshd
      ```
*   **Evite usar o usuário root** diretamente. Crie um novo usuário com permissões administrativas (sudo):

    ```bash
    sudo adduser novo_usuario
    sudo usermod -aG sudo novo_usuario
    ```
* **Proteja sua chave privada**:
  * Nunca compartilhe o arquivo `.pem` ou `.key`.
  * Armazene suas chaves em local seguro e faça backup criptografado.

***

### 🚩 Atenção!

* Sempre substitua `user` e `ip` pelos valores corretos do seu servidor.
* Não perca a senha da chave privada! Ela é necessária para autenticação.
