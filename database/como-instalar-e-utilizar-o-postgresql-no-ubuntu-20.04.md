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

# 🐘 Como Instalar e Utilizar o PostgreSQL no Ubuntu 20.04

<figure><img src="https://www.digitalocean.com/_next/static/media/intro-to-cloud.d49bc5f7.jpeg" alt=""><figcaption></figcaption></figure>

#### [Introdução](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#introducao)

Sistemas de gerenciamento de banco de dados relacionados são um componente fundamental de muitos sites e aplicativos. Eles fornecem uma maneira estruturada de armazenar, organizar e acessar informações.

O [PostgreSQL](https://www.postgresql.org/) ou Postgres é um sistema de gerenciamento de banco de dados relacionados que fornece uma implementação da linguagem estruturada [SQL](https://en.wikipedia.org/wiki/SQL). Ele está em conformidade com as normas e possui muitos recursos avançados, como transações confiáveis e concorrência sem trava de leitura.

Este guia demonstra como instalar o Postgres em um servidor Ubuntu 20.04. Ele também fornece algumas instruções para a administração geral de banco de dados.

### [Pré-requisitos](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#pre-requisitos)

Para acompanhar este tutorial, você precisará de um servidor Ubuntu 20.04 que tenha sido configurado seguindo nosso guia [Configuração Inicial do Servidor para Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04). Após concluir este tutorial pré-requisito, seu servidor deve ter um usuário não-**root** com permissões sudo e um firewall básico.

### [Passo 1 — Instalando o PostgreSQL](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#passo-1-instalando-o-postgresql)

Os repositórios padrão do Ubuntu contêm pacotes Postgres, para que você possa instalar esses usando o sistema de empacotamento `apt`.

Se você não tiver feito isso recentemente, atualize o índice de pacotes local do seu servidor:

{% code overflow="wrap" %}
```sh
sudo apt update
```
{% endcode %}

Então, instale o pacote Postgres jutamente com um pacote `-contrib` que adiciona alguns serviços e funcionalidade adicionais:

{% code overflow="wrap" %}
```sh
sudo apt install postgresql postgresql-contrib
```
{% endcode %}

Agora que o software está instalado, podemos examinar como ele funciona e como ele pode ser diferente de outros sistemas similares de gerenciamento de banco de dados que você possa ter usado.

### [Passo 2 — Usando as Roles PostgreSQL e Bancos de Dados](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#passo-2-usando-as-roles-postgresql-e-bancos-de-dados)

Por padrão, o Postgres usa um conceito chamado “roles” para lidar com a autenticação e autorização. Essas são, de certa forma, semelhantes a contas regulares estilo Unix, mas o Postgres não distingue entre os usuários e os grupos e, ao invés disso, prefere o termo “role” mais flexível.

Após a instalação, o Postgres é configurado para usar a autenticação _ident_, o que significa que ele associa os roles com uma conta do sistema Unix/Linux que combine. Se um role existe no Postgres, um nome de usuário Unix/Linux com o mesmo nome é capaz de fazer login como aquele role.

O procedimento de instalação criou uma conta de usuário chamada **postgres** que está associada ao role padrão do Postgres. Para utilizar o Postgres, você pode logar naquela conta.

Existem algumas maneiras de utilizar essa conta para acessar o Postgres.

#### [Mudando para a Conta postgres](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#mudando-para-a-conta-postgres)

Mude para a conta **postgres** no seu servidor digitando:

{% code overflow="wrap" %}
```sh
sudo -i -u postgres
```
{% endcode %}

Agora você pode acessar o prompt do Postgres imediatamente digitando:

{% code overflow="wrap" %}
```sql
psql
```
{% endcode %}

A partir daí, você está livre para interagir com o sistema de gerenciamento de banco de dados quando necessário.

Saia do prompt do PostgreSQL digitando:

{% code overflow="wrap" %}
```plsql
\q
```
{% endcode %}

Isso irá trazer você de volta ao prompt de comando do Linux `postgres`.

#### [Acessando um Prompt do Postgres Sem Mudar de Contas](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#acessando-um-prompt-do-postgres-sem-mudar-de-contas)

Você também pode executar o comando que você quiser com a conta **postgres** diretamente com o `sudo`.

Por exemplo, no último exemplo, você foi instruído a ir ao prompt do Postgres trocando primeiramente para o usuário **postgres** e então executando o `psql` para abrir o prompt do Postgres. Você poderia fazer isso em um passo executando o comando único `psql` como usuário **postgres** com `sudo`, dessa forma:

{% code overflow="wrap" %}
```sh
sudo -u postgres psql
```
{% endcode %}

Isso irá logar você diretamente no Postgres sem o shell `bash` intermediário.

Novamente, você pode sair da sessão interativa Postgres digitando:

```sh
\q
```

Muitas formas de uso requerem mais de um role Postgres. Leia para aprender a configurá-los.

### [Passo 3 — Criando um Novo Role](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#passo-3-criando-um-novo-role)

Atualmente, você tem o role **postgres** configurado no banco de dados. Você pode criar novos roles na linha de comando com o comando `createrole`. A flag `--interactive` irá te solicitar o nome do novo role e também perguntar se ele deveria ter permissões de superusuário.

Se você estiver logado com a conta **postgres**, crie um novo usuário digitando:

{% code overflow="wrap" %}
```plsql
createuser --interactive
```
{% endcode %}

Se, ao invés disso, você preferir usar o `sudo` para cada comando sem mudar da sua conta usual, digite:

{% code overflow="wrap" %}
```sh
sudo -u postgres createuser --interactive
```
{% endcode %}

O script irá te solicitar algumas escolhas e, com base nas suas respostas, executar os comandos corretos do Postgres para criar um usuário nas suas especificações.

```
OutputEnter name of role to add: sammy
Shall the new role be a superuser? (y/n) y
```

Você pode ter mais controle passando algumas bandeiras adicionais. Verifique as opções olhando para a página `man`:

{% code overflow="wrap" %}
```sh
man createuser
```
{% endcode %}

Sua instalação do Postgres agora tem um novo usuário, mas você ainda não adicionou nenhum banco de dados. A próxima seção descreve este processo.

### [Passo 4 — Criando um Novo Banco de Dados](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#passo-4-criando-um-novo-banco-de-dados)

Outra suposição que o sistema de autenticação do Postgres faz por padrão é que para qualquer role usado para logar, esse role terá um banco de dados com o mesmo nome que ele pode acessar.

Isso significa que, se o usuário que você criou na última seção for chamado de **sammy**, essa role tentará se conectar a um banco de dados também denominado “sammy”, por padrão. Você pode criar o banco de dados apropriado com o comando `createdb`.

Se você estiver logado com a conta **postgres**, você digitaria algo como:

{% code overflow="wrap" %}
```plsql
createdb sammy
```
{% endcode %}

Se, ao invés disso, você preferir usar o `sudo` para cada comando sem mudar da sua conta usual, você digitaria:

```
sudo -u postgres createdb sammy
```

Esta flexibilidade proporciona vários caminhos para a criação de bancos de dados conforme necessário.

### [Passo 5 — Abrindo um Prompt do Postgres com o Novo Role](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#passo-5-abrindo-um-prompt-do-postgres-com-o-novo-role)

Para logar com a autenticação baseada no `ident`, você precisará de um usuário Linux com o mesmo nome que seu role e banco de dados do Postgres.

Se você não tiver um usuário do Linux que combine disponível, você pode criar um com o comando `adduser`. Você terá que fazer isso através da sua conta **não-root** com privilégios `sudo` (ou seja, não logado como o usuário **postgres**):

{% code overflow="wrap" %}
```sh
sudo adduser sammy
```
{% endcode %}

Uma vez que essa nova conta estiver disponível, você pode ou mudar e se conectar ao banco de dados digitando:

{% code overflow="wrap" %}
```sh
sudo -i -u sammy
psql
```
{% endcode %}

Ou você pode fazer isso em linha:

```
sudo -u sammy psql
```

Este comando irá logar você automaticamente, supondo que todos os componentes tenham sido configurados corretamente.

Se você quiser que seu usuário se conecte a um banco de dados diferente, você pode fazer isso especificando o banco de dados dessa forma:

{% code overflow="wrap" %}
```plsql
psql -d postgres
```
{% endcode %}

Uma vez logado, você pode verificar sua informação de conexão atual digitando:

```
\conninfo
```

{% code overflow="wrap" %}
```plsql
OutputYou are connected to database "sammy" as user "sammy" via socket in "/var/run/postgresql" at port "5432".
```
{% endcode %}

Isso é útil se você estiver se conectando a bancos de dados fora do padrão ou com usuários que não sejam padrão.

### [Passo 6 — Criando e Deletando Tabelas](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#passo-6-criando-e-deletando-tabelas)

Agora que você sabe se conectar ao sistema de banco de dados do PostgreSQL, você pode aprender algumas tarefas básicas de gerenciamento do Postgres.

A sintaxe básica para criar tabelas é a seguinte:

{% code overflow="wrap" %}
```plsql
CREATE TABLE table_name (
    column_name1 col_type (field_length) column_constraints,
    column_name2 col_type (field_length),
    column_name3 col_type (field_length)
);
```
{% endcode %}

Como você pode ver, esses comandos dão à tabela um nome e então definem as colunas, além do tipo de coluna e o comprimento máximo dos dados do campo. Você também pode adicionar de modo opcional tabelas de restrições para cada coluna.

Você pode aprender mais sobre [como criar e gerenciar tabelas no Postgres](https://digitalocean.com/community/articles/how-to-create-remove-manage-tables-in-postgresql-on-a-cloud-server) aqui.

Para fins demonstrativos, crie a seguinte tabela:

{% code overflow="wrap" %}
```plsql
CREATE TABLE playground (
    equip_id serial PRIMARY KEY,
    type varchar (50) NOT NULL,
    color varchar (25) NOT NULL,
    location varchar(25) check (location in ('north', 'south', 'west', 'east', 'northeast', 'southeast', 'southwest', 'northwest')),
    install_date date
);
```
{% endcode %}

Estes comandos criarão uma tabela que faz um inventário de equipamentos de playground. A primeira coluna irá armazenar os números de ID do equipamento do tipo `serial`, que é um número inteiro de incremento automático. Esta coluna também tem a restrição de `PRIMARY KEY`, o que significa que os valores dentro dela devem ser únicos e não nulos.

As duas linhas seguintes criam colunas para `type` e `color` do equipamento, respectivamente, nenhum dos quais pode ser nulo. A linha seguinte a estas cria uma coluna `location` bem como uma restrição que exige que o valor seja um de oito valores possíveis. A última linha cria uma coluna de `data` que registra a data na qual você instalou o equipamento.

Para duas das colunas (`equip_id` e `install_date`), o comando não especifica um comprimento do campo. A razão para isso é que alguns tipos de dados não requerem uma definição de comprimento, pois o comprimento ou o formato está implícito.

Você pode ver sua nova tabela digitando:

```
\d
```

{% code overflow="wrap" %}
```
Output                  List of relations
 Schema |          Name           |   Type   | Owner
--------+-------------------------+----------+-------
 public | playground              | table    | sammy
 public | playground_equip_id_seq | sequence | sammy
(2 rows)
```
{% endcode %}

Sua tabela do playground está aqui, mas também há algo chamado `playground_equip_id_seq` que é do tipo `sequence`. Esta é uma representação do tipo `serial` que você deu à sua coluna `equip_id` Isso mantém o rastro do próximo número na sequência e é criado automaticamente para colunas deste tipo.

Se você quiser ver apenas a tabela sem a sequência, você pode digitar:

```
\dt
```

{% code overflow="wrap" %}
```
Output          List of relations
 Schema |    Name    | Type  | Owner
--------+------------+-------+-------
 public | playground | table | sammy
(1 row)
```
{% endcode %}

Com uma tabela pronta, vamos utilizá-la para praticar o gerenciamento de dados.

### [Passo 7 — Adicionando, Consultando e Deletando Dados em uma Tabela](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#passo-7-adicionando-consultando-e-deletando-dados-em-uma-tabela)

Agora que você tem uma tabela. você pode inserir alguns dados nela. Como um exemplo, adicione um escorregador ou slide e um balanço ou swing chamando a tabela que você deseja adicionar, nomeando as colunas e, depois, fornecendo dados para cada coluna, desta forma:

{% code overflow="wrap" %}
```plsql
INSERT INTO playground (type, color, location, install_date) VALUES ('slide', 'blue', 'south', '2017-04-28');
INSERT INTO playground (type, color, location, install_date) VALUES ('swing', 'yellow', 'northwest', '2018-08-16');

```
{% endcode %}

Você deve tomar cuidado ao introduzir os dados para evitar alguns problemas comuns. Por exemplo, não envolva os nomes das colunas em aspas, mas os valores de coluna que você digitar precisam de aspas.

Outra coisa para ficar atento é não digitar um valor para a coluna `equip_id`. Isso acontece porque isso é gerado automaticamente sempre quando você adicionar uma nova linha à tabela.

Recupere a informação que você adicionou digitando:

{% code overflow="wrap" %}
```plsql
SELECT * FROM playground;
```
{% endcode %}

```
Output equip_id | type  | color  | location  | install_date
----------+-------+--------+-----------+--------------
        1 | slide | blue   | south     | 2017-04-28
        2 | swing | yellow | northwest | 2018-08-16
(2 rows)
```

Aqui, você pode ver que seu `equip_id` foi preenchido com sucesso e que todos os seus outros dados foram organizados corretamente.

Se o slide no playground falhar e você tiver que removê-lo, você também pode remover a linha da sua tabela digitando:

{% code overflow="wrap" %}
```plsql
DELETE FROM playground WHERE type = 'slide';
```
{% endcode %}

Consulte a tabela novamente:

{% code overflow="wrap" %}
```plsql
SELECT * FROM playground;
```
{% endcode %}

{% code overflow="wrap" %}
```
Output equip_id | type  | color  | location  | install_date
----------+-------+--------+-----------+--------------
        2 | swing | yellow | northwest | 2018-08-16
(1 row)
```
{% endcode %}

Observe que a linha do `slide` não faz mais parte da tabela.

### [Passo 8 — Adicionando e Deletando Colunas de uma Tabela](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#passo-8-adicionando-e-deletando-colunas-de-uma-tabela)

Após criar uma tabela, você pode modificá-la adicionando ou removendo colunas. Adicione uma coluna para mostrar a última visita de manutenção para cada peça de equipamento digitando:

{% code overflow="wrap" %}
```plsql
ALTER TABLE playground ADD last_maint date;
```
{% endcode %}

Se você vir a informação da sua tabela novamente, você verá que a nova coluna foi adicionada mas nenhum dado foi adicionado:

{% code overflow="wrap" %}
```plsql
SELECT * FROM playground;
```
{% endcode %}

{% code overflow="wrap" %}
```
Output equip_id | type  | color  | location  | install_date | last_maint
----------+-------+--------+-----------+--------------+------------
        2 | swing | yellow | northwest | 2018-08-16   |
(1 row)
```
{% endcode %}

Se você descobrir que sua equipe de trabalho usa uma ferramenta separada para acompanhar o histórico de manutenção, você pode deletar da coluna digitando:

{% code overflow="wrap" %}
```plsql
ALTER TABLE playground DROP last_maint;
```
{% endcode %}

Isso apaga a coluna `last_maint` e quaisquer valores encontrados nela, mas deixa todos os outros dados intactos.

### [Passo 9 — Atualizando os Dados em uma Tabela](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#passo-9-atualizando-os-dados-em-uma-tabela)

Até agora, você aprendeu a adicionar registros a uma tabela e como deletá-los, mas este tutorial ainda não cobriu como modificar os itens existentes.

Você pode atualizar os valores de um item existente consultando o registro que você quiser e definindo a coluna para o valor que você deseja usar. É possível consultar pelo registro `swing` (isto corresponderá a _cada_ swing na sua tabela) e alterar sua cor para `red`. Isso pode ser útil se você realizou uma pintura com o swing.

{% code overflow="wrap" %}
```plsql
UPDATE playground SET color = 'red' WHERE type = 'swing';
```
{% endcode %}

Você pode verificar se a operação foi bem sucedida consultando os dados novamente:

{% code overflow="wrap" %}
```plsql
SELECT * FROM playground;
```
{% endcode %}

{% code overflow="wrap" %}
```plsql
Output equip_id | type  | color | location  | install_date
----------+-------+-------+-----------+--------------
        2 | swing | red   | northwest | 2018-08-16
(1 row)
```
{% endcode %}

Como você pode ver, o slide agora está registrado como red (vermelho).

### [Conclusão](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-pt#conclusao)

Você agora está com o PostgreSQL configurado no seu servidor Ubuntu 20.04. Se você quiser aprender mais sobre o Postgres e como utilizá-lo, encorajamos você a verificar os seguintes guias:

* [Uma comparação com sistemas de gerenciamento de banco de dados relacionados](https://www.digitalocean.com/community/tutorials/sqlite-vs-mysql-vs-postgresql-a-comparison-of-relational-database-management-systems)
* [Pratique a execução de consultas com o SQL](https://www.digitalocean.com/community/tutorials/introduction-to-queries-postgresql)
