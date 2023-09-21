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

# 🔍 Consultas JSON no postgres

### **Consultando se contem um item em um campo com array json**

```sql
SELECT * FROM {table_name}
WHERE {column_json} @> '[{"name":"Flavio"}]';
```

### **Transformando um campo json em varias linhas**

```sql
SELECT jsonb_array_elements({column_json}) FROM table_name;
```

### Consultando JSONB

{% code overflow="wrap" %}
```sql
SELECT * FROM my_table WHERE event_type = 'updated_kyc_status' and event_data @> '{"updatedKycStatus": {"externalAccountHolderId": "fa54539a-2ba9-4298-b4f3-6145101fdabc"}}'
```
{% endcode %}
