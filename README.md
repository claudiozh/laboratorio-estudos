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

# Como organizar e estruturar projetos com NodeJS

No node temos muita liberdade para construir nossa API REST da forma que desejarmos, quem está começando não sabe ao certo como organizar.

Talvez em projetos pequenos você não perceba os problemas que isso pode te causar, projetos maiores onde visa escalonar, provavelmente terá problemas com essa organização para manter o código com um alto acoplamento trazendo falta de reutilização de código, falta de estabilidade...

Por isso nesse artigo vamos falar sobre uma estrutura que irá ajudar a trazer uma melhor sustentabilidade e escalonabilidade para sua aplicação.

## Estrutura de pastas  <a href="#019d" id="019d"></a>

> Lembrando que cada projeto tem suas peculiaridades, então basta você/equipe decidir o que é melhor para adicionar ou remover no seu projeto visando sempre o equilibrio entre agilidade e qualidade na entrega do produto.

## Controllers e os fardos que elas carregam <a href="#f8ff" id="f8ff"></a>

A prática de atribuir bastantes regras de negócio nos controllers express.js é de fácil visualização, pois na cabeça de quem está começando tudo é uma coisa só.

E com isso uma controller que começou tendo _300 linhas_ de código com o passar do tempo ela pode ficar com mais de _1000 linhas_. Pois você estará implementando **querys builders, regras de negócios, chamada para serviços externos e muito mais…**

Com isso para _testar, reaproveitar código e dar manutenção_ que deveria ser um trabalho mais fácil acaba se tornando muito complicado.

<figure><img src="https://miro.medium.com/v2/resize:fit:560/1*qpELXZNQgMX63di9rZZ76Q.png" alt="" height="394" width="700"><figcaption></figcaption></figure>

Acima segue um _exemplo ruim_ de uma controller onde atribuimos todas as responsabilidades em uma única camada, vamos adotar _princípio da **responsabilidade única do SOLID** para fazer a separação**.**_

> “Uma classe deve ter **um**, **e apenas um**, motivo para ser modificada”

## Ao separar as controllers das regras de negócio o que ganhamos? <a href="#d6ab" id="d6ab"></a>

Reutilização de código em outras classes, fazer testes únitarios e de integração ficará mais claro o que de fato você terá que testar em cada um, fácil manutenção já que iremos granularizar nossas responsabilidades…

<figure><img src="https://miro.medium.com/v2/resize:fit:560/1*lCUJfaV5c8XShyte3qRFvQ.png" alt="" height="396" width="700"><figcaption></figcaption></figure>

## **Controllers** <a href="#dc70" id="dc70"></a>

O controle deve se preocupar em aceitar a solicitação, repassar para o serviço de domínio correto processe a solicitação e entregue a resposta ao cliente.

## Services <a href="#1fda" id="1fda"></a>

Essa camada é um design pattern que ajuda a abstrair suas regras de negócio, deixando sua controller mais limpa e com a responsabilidade única.

Um outro ponto importante que a medida que cresce sua aplicação você tende a reutilizar os códigos já implementados nesta camada. Imagine que você tem **três controllers** que faz uso de um service e você precisa alterar alguma parte do código, obviamente você vai utilizar **somente a função no service** para alterar, entretanto se não tivessemos essa camada? Teriamos sair procurando no nosso projeto **todos os lugares que faz o uso daquele trecho de código.**

Para fazer testes unitários será muito mais fácil, sem necessidade de fazer uma request a um endpoint.

## Repositories <a href="#b050" id="b050"></a>

Ter querys sql no código de uma service isso torna um código grande e ilegível, por isso atribuimos aos repositories o trabalho de ser uma camada de acesso e interação com as entidades do banco de dados.

Temos dois pontos que podemos utilizar para falar da utilização de um repository, _**centralizar regras de recuperação e persistência de dados** e_\
_**abstrair a utilização de ORMs possibilitando a troca por outros ORMs**,_ mas vamos falar a verdade é **muito dificil de um projeto ficar trocando de ORM**.

<figure><img src="https://miro.medium.com/v2/resize:fit:454/1*WpfYs-nYsbVZhpX4aZoFJA.png" alt="" height="412" width="568"><figcaption></figcaption></figure>

> Vale ressaltar que no mundo ideal levariamos a risca o **S** de SOLID e com isso cada classe teria sua responsabilidade única, mas sabemos que não temos tanto tempo para fazer isso no dia-a-dia por isso nem sempre vai ser necessário ter uma classe no repo, pois o metodo utilizado pode ser somente um **findAll().** Isso vai da avaliação do programador para saber se é necessário a utilização dessa camada.

## Subscribers (Pub/Sub) <a href="#3b85" id="3b85"></a>

Quando se tem uma aplicação onde ela utiliza serviços de terceiros e geralmente fazemos uso na camada de controller junto com a regras de negócio com o tempo a aplicação crescendo é muito provável que iremos acrescentar mais linhas de códigos de serviços externos.

Abordagem de utilizar um serviço de terceiro de forma imperativo não é a melhor opção para esse caso, por isso é bom trabalhar com eventos sendo emitidos para cada subscriber depois que uma ação for executada na camada da service.

<figure><img src="https://miro.medium.com/v2/resize:fit:560/1*eCZUBWMA6Z_JDGcieVFt8w.png" alt="" height="396" width="700"><figcaption></figcaption></figure>

## Jobs <a href="#d9e4" id="d9e4"></a>

Essa camada é criada para armazenar tarefas agendadas que precisam ser feitas automaticamente em um certo intervalo de tempo. Como nossa regra de negócios está centralizada em um serviço isso facilita a utilização em um cron.

Devido a forma que o node funciona é melhor evitar a utilização de formas primitivas para agendar uma tarefa e com isso você ganha um controle melhor dos retornos da ação executada.

## Models <a href="#a7a2" id="a7a2"></a>

Determina a estrutura lógica que representa uma entidade do banco de dados e da forma na qual os dados podem ser manipulados e organizados.

## Database <a href="#a239" id="a239"></a>

Onde organizamos nossas **migrations** para que a equipe tenha o controle do versionamento do banco de dados e dos **seeders** para popular nosso banco com dados inserido pelo desenvolvedor, para mais informação sobre essa parte é só ir nesse artigo abaixo:

[Migrations e Seeders no SequelizeJSVamos começar com um overviewmedium.com](https://medium.com/@diomalta/migrations-e-seeders-no-sequelizejs-67ba3571ed0e?source=post\_page-----4845be004899--------------------------------)

## Utils <a href="#670d" id="670d"></a>

Trechos de código pequeno que são utilizado por mais de uma classe. Funções que são utilizadas para auxiliar na construção de um código maior e podendo ser utilizado em qualquer parte das camadas aplicação, por exemplo um _helper_ pode utilizar mais de um _util_ para construir um código mais completo para uma finalidade especifica.

## Helpers <a href="#96db" id="96db"></a>

Trechos de arquitetura de código que contém apresentações lógicas que podem ser compartilhadas entre as camadas da aplicação contendo várias funções englobada para servir de bootstrap a outros componentes e ergonomia do desenvolvedor.

## Constants <a href="#3519" id="3519"></a>

A utilização das constantes strings são muito importantes para você poder centralizar uma palavra de retorno de error, sucesso, status HTTP, nome de uma entidade que se repete por várias partes do código pois na hora quando houver uma mudança de valor naquela constantes todas as partes que utilizarem vão ser alteradas sem a necessidade de ficar procurando em cada arquivo pelo projeto.

## Config <a href="#97e1" id="97e1"></a>

É onde vamos centralizar todas as nossas variáveis de ambiente e outras configurações que utilizaremos pela aplicação, como: acesso a banco de dados, chave secreta, email, testes e muito mais..

É parecido com o problema que temos com as constantes, imagine termos _**process.env.JWT\_SECRET**_ espalhados em models, tests, controllers e quando você tiver que alterar o nome da variavel ou algo do tipo por conta que mudou a forma como você cria um token e sair atualizando e caçando cada arquivo para fazer a alteração.

## Rotas <a href="#24e7" id="24e7"></a>

Separamos as rotas das controllers, pois uma rota pode ter vários tipo de requisições (post, get, put, delete, option) e assim mantemos o código limpo.

## Esse padrão é perfeito para qualquer projeto? <a href="#fb0c" id="fb0c"></a>

Cada projeto é único e com isso temos que ter a flexibilidade para adequar com a necessidades que surgem, para essa organização já foi testada em produção correspondendo bem nos quesitos de escalabilidade e sustentabilidade do código e alinhando a agilidade com a qualidade na entrega dos resultados.
