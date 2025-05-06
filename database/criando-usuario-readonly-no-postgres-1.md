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

# 😎 Criando usuário readonly no postgres

### 🙍 Criando usuário readonly no postgres

Como exemplo de uso vamos criar um novo usuário **flavio** e liberar acesso somente leitura para o banco de dados **notas\_db**

### **Criar um novo usuário \[flavio]**

```plsql
CREATE USER claudio WITH PASSWORD 'minha-senha-segura';
```

### Concedendo acesso para se conectar ao banco \[notas\_db]

```plsql
GRANT CONNECT ON DATABASE notas_db TO claudio;
```

### **Concedendo acesso ao schema \[public]**

```plsql
GRANT USAGE ON SCHEMA "public" TO claudio;
```

## **Concedendo permissão de consultas**

**Para uma tabela específica categorias**

```plsql
GRANT SELECT ON categorias TO claudio;
```

**Para todas as tabelas de do schema \[public]**

```plsql
GRANT SELECT ON ALL TABLES IN SCHEMA "public" TO claudio;
```

**Se você deseja conceder acesso à nova tabela automaticamente no futuro, você deve alterar o padrão:**

```plsql
ALTER DEFAULT PRIVILEGES IN SCHEMA "public" GRANT SELECT ON TABLES TO claudio;
```

**Referências**

* [https://tableplus.com/blog/2018/04/postgresql-how-to-create-read-only-user.html](https://tableplus.com/blog/2018/04/postgresql-how-to-create-read-only-user.htmlhttps://tableplus.com/blog/2018/06/postgresql-how-to-create-user.html)
* [https://tableplus.com/blog/2018/06/postgresql-how-to-create-user.html](https://tableplus.com/blog/2018/04/postgresql-how-to-create-read-only-user.htmlhttps://tableplus.com/blog/2018/06/postgresql-how-to-create-user.html)
