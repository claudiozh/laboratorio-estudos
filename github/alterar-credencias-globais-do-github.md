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
   name = {your github username}
   email = {your email}
[credential]
   helper = store --file ~/.git-credentials
```
{% endcode %}

### Crie um arquivo chamado **.git-credentials** em seu diretório principal

```shell=
sudo nano .git-credentials
```

### Adicione a seguinte linha no arquivo e substitua com seus dados do github

{% code overflow="wrap" %}
```plsql
https://NOME_USUARIO:TOKEN_DE_ACESSO@github.com
```
{% endcode %}
