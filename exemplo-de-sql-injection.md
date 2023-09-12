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

# Exemplo de SQL Injection

### Código malicioso

{% code overflow="wrap" %}
```javascript
const express = require('express');
const sqlite3 = require('sqlite3');
const bodyParser = require('body-parser');

const app = express();

const db = new sqlite3.Database(':memory:');

db.serialize(() => {
  db.run(
    'CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, email TEXT, password TEXT)',
  );

  db.run(`INSERT INTO users (name, email, password) VALUES 
    ('admin', 'admin@gmail.com', '123456'),
    ('suporte', 'suporte@gmail.com', '123456')`);
});

app.use(bodyParser.json());

app.post('/login', (req, res) => {
  const { email, password } = req.body;

  // **ALERTA: Este código é vulnerável a SQL Injection.**
  const sql = `SELECT * FROM users WHERE email = '${email}' AND password = '${password}'`;

  db.get(sql, (err, row) => {
    if (err) {
      return res.status(500).send({ message: 'Internal Server Error' });
    }

    if (!row) {
      return res.status(404).send({ message: 'User not found' });
    }

    return res.send(row);
  });
});

app.listen(3000, () => {
  console.log('Servidor iniciado na porta 3000');
});

```
{% endcode %}

Aqui está um exemplo de uma solicitação CURL que explora a vulnerabilidade de SQL Injection:

```bash
curl -X POST http://localhost:3000/login -H "Content-Type: application/json" -d '{
    "email": "admin@gmail.com'--",
    "password": "senahincorreta"
}'
```

Explicação:

* O payload da solicitação CURL contém um email com `'--'` no final, que é uma tentativa de realizar SQL Injection.
* O `'--'` é usado para comentar o restante da consulta SQL original, tornando a condição da senha irrelevante.
* Como resultado, o SQL Injection permite que o invasor acesse a conta 'admin' sem a senha correta.

É importante enfatizar que este é um exemplo de SQL Injection com o propósito de demonstração. Em aplicações reais, é crucial adotar medidas de segurança, como instruções preparadas ou consultas parametrizadas, para evitar vulnerabilidades de SQL Injection.

### Código com solução

Para resolver o problema de SQL Injection em sua aplicação, você deve substituir a construção de consultas SQL concatenadas com instruções preparadas ou consultas parametrizadas. Isso garante que os dados de entrada não sejam interpretados como parte do código SQL e, portanto, não podem ser explorados para injetar SQL malicioso. Vamos atualizar o código para usar instruções preparadas:

```sql
const express = require('express');
const sqlite3 = require('sqlite3');
const bodyParser = require('body-parser');

const app = express();

const db = new sqlite3.Database(':memory:');

db.serialize(() => {
  db.run(
    'CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, email TEXT, password TEXT)',
  );

  db.run(`INSERT INTO users (name, email, password) VALUES 
    ('admin', 'admin@gmail.com', '123456'),
    ('suporte', 'suporte@gmail.com', '123456')`);
});

app.use(bodyParser.json());

app.post('/login', (req, res) => {
  const { email, password } = req.body;

  const sql = 'SELECT * FROM users WHERE email = ? AND password = ?';

  // Usando instruções preparadas para evitar SQL Injection
  db.get(sql, [email, password], (err, row) => {
    if (err) {
      return res.status(500).send({ message: 'Internal Server Error' });
    }

    if (!row) {
      return res.status(404).send({ message: 'User not found' });
    }

    return res.send(row);
  });
});

app.listen(3000, () => {
  console.log('Servidor iniciado na porta 3000');
});

```

Neste código, as consultas SQL agora são preparadas com espaços reservados `?` para os valores das variáveis. Ao passar `[email, password]` como o segundo argumento na função `db.get`, os valores de `email` e `password` são automaticamente inseridos nas posições apropriadas e escapados corretamente. Isso evita a possibilidade de SQL Injection, pois o banco de dados trata os valores como dados, não como parte da consulta SQL.

Este é um método seguro para lidar com consultas SQL em sua aplicação e ajuda a prevenir vulnerabilidades de SQL Injection. Certifique-se de aplicar essa abordagem a todas as consultas SQL sensíveis em seu aplicativo para manter a segurança. Além disso, como mencionado anteriormente, também é importante armazenar senhas de forma segura usando técnicas como o hashing de senhas.
