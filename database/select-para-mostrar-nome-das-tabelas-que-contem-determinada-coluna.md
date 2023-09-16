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

# 🔍 Select para mostrar nome das tabelas que contêm determinada coluna

{% code overflow="wrap" %}
```plsql
select t.table_schema,
       t.table_name
from information_schema.tables t
inner join information_schema.columns c on c.table_name = t.table_name 
                                and c.table_schema = t.table_schema
where c.column_name = 'contato_id'
      and t.table_schema not in ('information_schema', 'pg_catalog')
      and t.table_type = 'BASE TABLE'
order by t.table_schema;
```
{% endcode %}
