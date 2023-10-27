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

# 🆔 Alterar credencias globais do github

### Execute o comando abaixo para abrir o arquivo de configurações do github

{% code overflow="wrap" %}
```sh
sudo nano \.gitconfig
```
{% endcode %}

### Altere o arquivo substituindo com suas informações do github

{% code overflow="wrap" %}
```sh
[user]
   name = {YOUR_GITHUB_USERNAME}
   email = {YOUR_GITHUB_EMAIL}
[credential]
   helper = store --file ~/.git-credentials
```
{% endcode %}

### Crie um arquivo em seu diretório principal

{% code title=".git-credentials.sh" %}
```bash
sudo nano .git-credentials
```
{% endcode %}

### Adicionar permissão no arquivo

```bash
chmod +x .git-credentials.sh
```

### Adicione a seguinte linha no arquivo e substitua com seus dados do github

{% code overflow="wrap" %}
```plsql
https://YOUR_GITHUB_USERNAME:ACCESS_TOKEN@github.com
```
{% endcode %}

### Crie um scrip para fazer a troca entre credencias entre contas

<pre class="language-bash" data-title="change-github-credentials.sh" data-overflow="wrap"><code class="lang-bash"><strong>
</strong><strong>WORK_URL="https://NOME_USER:ACCESS_TOKEN@github.com"
</strong>
PERSONAL_URL="https://NOME_USER:ACCESS_TOKEN@github.com"

PATH_FILE_GITHUB_CREDENTIALS="/home/claudio/.git-credentials"

if grep -q "$WORK_URL" "$PATH_FILE_GITHUB_CREDENTIALS"; then
  git config --global user.name "NOME_USER_PERSONAL"
  git config --global user.email "EMAIL_USER_PERSONAL"
  sed -i "s|$WORK_URL|$PERSONAL_URL|g" "$PATH_FILE_GITHUB_CREDENTIALS"
  echo "ALTERADO PARA CREDENCIAIS PESSOAIS"
else
  git config --global user.name "NOME_USER_WORK"
  git config --global user.email "NOME_USER_WORK"
  sed -i "s|$PERSONAL_URL|$WORK_URL|g" "$PATH_FILE_GITHUB_CREDENTIALS"
echo "ALTERADO PARA CREDENCIAIS DA EMPRESA"
fi

</code></pre>

### Adicionar permissão no arquivo

```bash
chmod +x .change-github-credentials.sh
```

### Mover o scrip para um diretório de execução do linux

```bash
mv /usr/local/bin change-github-credentials
```

Pronto agora é só executar o scrip para realizar a troca entre credenciais
