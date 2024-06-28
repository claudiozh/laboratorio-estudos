# 🌟 Select para trazer todas as colunas da base de dados

```sql
SELECT 
    column_name 
FROM 
    information_schema.columns 
WHERE 
    table_schema = 'public' -- Substitua 'public' pelo nome do seu esquema, se for diferente
GROUP BY 
    column_name

```
