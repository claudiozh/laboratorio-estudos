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

# Comandos linux

### **Permissão para pasta**

{% code overflow="wrap" %}
```shell
chmod 755 $(find DIRETORIO -type d)
```
{% endcode %}

### Permissão para todos os arquivos dentro de pasta ou subpastas

{% code overflow="wrap" %}
```sh
chmod 644 $(find DIRETORIO -type f)
```
{% endcode %}

### **Mostrar IP usando addr e filtro**

{% code overflow="wrap" %}
```sh
ip addr | grep 192
```
{% endcode %}

### **Exibir tamanho dos arquivos em pastas e subpastas a partir do diretório especificado.**

{% code overflow="wrap" %}
```sh
du -h CAMINHO_DO_DIRETORIO
```
{% endcode %}

### **Permissão para arquivo executável**

{% code overflow="wrap" %}
```sh
sudo chmod +x start.sh
```
{% endcode %}

### **Exibir quantidade de linhas de um arquivo**

{% code overflow="wrap" %}
```sh
cat NOME_ARQUIVO| wc -l
```
{% endcode %}

### **Enviar arquivo ou pasta usando SCP**

{% code overflow="wrap" %}
```shell
scp CAMINHO_ARQUIVO_OU_PASTA usuario@IP:PASTA_DESTINO
```
{% endcode %}

### **Monitorar alterações em um arquivo (Bastante utilizado para monitorar logs)**

{% code overflow="wrap" %}
```sh
tail -f CAMINHO_DO_ARQUIVO
```
{% endcode %}

### **Remover vários arquivos fazendo um filtro**

{% code overflow="wrap" %}
```sh
rm *FILTRO_A_SER_REMOVIDO
```
{% endcode %}

```
rm*.log
```

> Isso iria remover todos os arquvios que terminassem com .log

### **Acessar servidor via SSH e fazer forward ref de porta**

{% code overflow="wrap" %}
```sh
ssh -L 5432:localhost:5432 usuario@ip
```
{% endcode %}
