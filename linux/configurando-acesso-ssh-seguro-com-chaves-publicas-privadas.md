---
description: >-
  Configure o acesso SSH de forma segura usando chaves públicas/privadas.
  Roteiro para gerar chaves, copiar a chave pública, desabilitar o acesso por
  senha e criar usuário não-root.
---

# Configurando acesso SSH seguro com chaves públicas/privadas

## Roteiro para configurar o acesso SSH utilizando chaves públicas/privadas:

1. Abra o terminal na máquina local
2. Execute o comando `ssh-keygen` para gerar as chaves SSH. Pressione Enter para aceitar o local e o nome padrão do arquivo de chave.
3. Será solicitada uma senha para proteger a chave privada. Digite uma senha segura e pressione Enter. (Você precisará dessa senha ao fazer login via SSH no servidor pela primeira vez.)
4. As chaves pública e privada foram geradas e estão localizadas no diretório `~/.ssh/`.
5. Agora, copie a chave pública para o servidor usando o comando `ssh-copy-id user@ip`. Substitua "user" pelo nome do usuário do servidor e "ip" pelo endereço IP ou nome de domínio do servidor.
   * Se você desejar especificar o arquivo da chave pública a ser enviado, execute o comando `ssh-copy-id -i PATH_DO_ARQUIVO user@ip`.
   * Será solicitada a senha do usuário do servidor. Digite-a e pressione Enter para concluir a cópia da chave pública.
6. Agora, você pode fazer login no servidor via SSH sem a necessidade de senha usando a chave privada correspondente.
   * Execute o comando `ssh user@ip` para fazer login no servidor. Substitua "user" pelo nome do usuário do servidor e "ip" pelo endereço IP ou nome de domínio do servidor.
   * Digite a senha que você definiu ao gerar as chaves SSH e pressione Enter. Agora você está conectado ao servidor.

Dicas adicionais:

* Para aumentar a segurança, você pode configurar o servidor para desativar o acesso SSH com senha e permitir apenas autenticação baseada em chaves públicas.
* É recomendado criar um novo usuário no servidor para evitar o uso do usuário root, pois isso aumenta a segurança do sistema.
* Certifique-se de proteger sua chave privada, pois ela concede acesso ao servidor. Mantenha-a em um local seguro e não a compartilhe com pessoas não autorizadas.

Lembre-se de substituir os placeholders "user" e "ip" pelos valores corretos, de acordo com sua configuração específica.

\
