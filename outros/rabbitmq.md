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

# RabbitMQ

### Sobre o RabbitMQ Comment <a href="#sobre-o-rabbitmq" id="sobre-o-rabbitmq"></a>

* Message Broker
* Implementa AMQP, MQTT, STOMP e HTTP
* Desenvolvido em Erlang
* Desacoplamento entre serviços
* Rápido e Poderoso

### Funcionamento básico <a href="#funcionamento-basico" id="funcionamento-basico"></a>

* **Publisher**:
  * Responsável por fazer o envio das mensagens para uma exchange
* **Exchange**:
  * Pega todas as mensagens enviadas por um publisher, processa e redireciona para as queues interessadas
* **Queue**:
  * Armazena todas as mensagens enviadas e distribui para o consumidor
* **Consumer**:
  * Cliente responsável por consumir as mensagens que estão armazenadas nas queues

### Tipos de exchange <a href="#tipos-de-exchange" id="tipos-de-exchange"></a>

* Direct
* Fanout
* Topic
* Headers

### Direct exchange <a href="#direct-exchange" id="direct-exchange"></a>

![](https://i.imgur.com/dgv6scs.png)

### Fanout exchange <a href="#fanout-exchange" id="fanout-exchange"></a>

![](https://i.imgur.com/4azTIYS.png)
