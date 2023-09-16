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

# 📦 Backup e restauração no Postgres

### Backup usando pg\_dumpall

```shell=

# entrando no diretorio do postgres e usando usuario postgres
cd /var/lib/PATH_DOS_DADOS_DO_POSTGRES && su postgres

# realizando backup full
pg_dumpall -v -f filename-backup.sql

# verificando tamanho do arquivo
du -h filename-backup.sql

# gerando md5 do arquivo
md5sum filename-backup.sql

```

### Restore usando pg\_dumpall

{% code overflow="wrap" %}
```plsql

# copiando para maquina de destino
scp {user}@{new_host}:/var/lib/pgsql/9.6/filename-backup.sql .

# checando tamanho 
du -h filename-backup.sql

# checando hash md5 
md5sum filename-backup.sql

# restore full
psql -U postgres -f filename-database.sql
```
{% endcode %}

### Como fazer o backup PostgreSQL pgdump

```shell=
pg_dump --host localhost --port 5432 --username provapp --format tar --file fileName.backup --verbose NomeDoBanco
```

* **--host localhost**: define o local onde o banco se encontra, pode ser localmente ou externamente em outra rede;
* **--port 5432**: define a porta utilizada, nesse caso a padrão postgres 5432;
* **--username** postgres: define qual é o usuário utilizado na comunicação;
* **--format tar**: o tipo de compressão do arquivo gerado;
* **--file fileName.backup**: define com qual nome e caminho completo do arquivo que será gerado;
* **NomeDoBanco**: por último vai o nome do banco que se estará exportando, atenção neste ponto, pois é case sensitive, ou seja ele considera letras maiúsculas e minúsculas.
* **--verbose** Modo informações detalhadas

### Como fazer o restore PostgreSQL

{% code overflow="wrap" %}
```plsql
pg_restore --host localhost --port 5432 --username postgres --dbname nomeDoBanco --verboser fileName.backup
```
{% endcode %}

* **--host localhost**: define o local onde o banco se encontra, pode ser localmente ou externamente em outra rede.
* **--port 5432**: é definida a porta utilizada, nesse caso a padrão postgres 5432.
* **--username postgres**: define qual é o usuário utilizado na comunicação.
* **--dbname nomeDoBanco**: o nome do banco que se estará exportando, atenção neste ponto, pois é case sensitive, ou seja ele considera letras maiúsculas e minúsculas.
* **fileName.backup**: por último, vai o caminho completo do arquivo que deseja restaurar.
* **--verbose** Modo informações detalhadas

### Referências

* https://blog.tecnospeed.com.br/backup-e-restore-postgresql/
* https://www.postgresqltutorial.com/postgresql-administration/postgresql-restore-database/
