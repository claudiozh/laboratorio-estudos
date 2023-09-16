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

# ➕ Insert em uma tabela A, com dados de uma tabela B

{% code overflow="wrap" %}
```plsql
insert
    into
    tabelaA (nome, descricao)
select
    tb.nome,
    tb.descricao
from
    tabelaB tb;
```
{% endcode %}
