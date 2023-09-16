---
description: >-
  Como exemplo de uso vamos criar um novo usuário claudio e liberar acesso
  somente leitura para o banco de dados notas_db
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

# 🕵♂ Criando usuário readonly no postgres

### Criar um novo usuário \[claudio]

{% code overflow="wrap" %}
```plsql
CREATE USER claudio WITH PASSWORD 'minha-senha-segura';
```
{% endcode %}

### **Concedendo acesso para se conectar ao banco \[notas\_db]**

{% code overflow="wrap" %}
```plsql
GRANT CONNECT ON DATABASE notas_db TO claudio;
```
{% endcode %}

### **Concedendo acesso ao schema \[public]**

{% code overflow="wrap" %}
```plsql
GRANT USAGE ON SCHEMA "public" TO claudio;
```
{% endcode %}

### **Concedendo permissão de consultas**

Para uma tabela específica \[categorias]

{% code overflow="wrap" %}
```plsql
GRANT SELECT ON categorias TO claudio;
```
{% endcode %}

Para todas as tabelas de do schema \[public]

{% code overflow="wrap" %}
```plsql
GRANT SELECT ON ALL TABLES IN SCHEMA "public" TO claudio;
```
{% endcode %}

### **Se você deseja conceder acesso à nova tabela automaticamente no futuro, você deve alterar o padrão:**

{% code overflow="wrap" %}
```plsql
ALTER DEFAULT PRIVILEGES IN SCHEMA "public" GRANT SELECT ON TABLES TO claudio;
```
{% endcode %}

### Referências

* [https://tableplus.com/blog/2018/04/postgresql-how-to-create-read-only-user.html](https://tableplus.com/blog/2018/04/postgresql-how-to-create-read-only-user.htmlhttps://tableplus.com/blog/2018/06/postgresql-how-to-create-user.html)
* [https://tableplus.com/blog/2018/06/postgresql-how-to-create-user.html](https://tableplus.com/blog/2018/04/postgresql-how-to-create-read-only-user.htmlhttps://tableplus.com/blog/2018/06/postgresql-how-to-create-user.html)
