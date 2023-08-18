---
description: >-
  Neste artigo, exploraremos como aplicar os princípios SOLID no contexto do
  Nest, uma estrutura popular para criar aplicativos escaláveis ​​e modulares
  com TypeScript.
---

# Aplicando Princípios SOLID no NestJS

## Introdução

No mundo do desenvolvimento de software, escrever um código limpo, sustentável e escalável é de extrema importância. Uma maneira de conseguir isso é seguir os princípios SOLID, um conjunto de cinco princípios de design que ajudam os desenvolvedores a criar sistemas de software robustos e flexíveis.

Vamos nos aprofundar em cada princípio, fornecendo exemplos de código concisos para ilustrar sua implementação em um projeto NestJS. No final deste guia, você entenderá como aproveitar esses princípios para escrever um código mais limpo e de fácil manutenção em seus aplicativos NestJS. Então, vamos começar e nivelar nosso desenvolvimento NestJS com princípios SOLID!

## **Os princípios SOLID que abordaremos neste artigo são:**

1. Princípio de Responsabilidade Única (SRP)
2. Princípio Aberto-Fechado (OCP)
3. Princípio da Substituição de Liskov (LSP)
4. Princípio de Segregação de Interface (ISP)
5. Princípio de Inversão de Dependência (DIP)

## **1. Princípio da Responsabilidade Única (SRP):**

O Princípio da Responsabilidade Única, como o nome sugere, propõe que cada módulo ou classe de software tenha uma função ou responsabilidade específica. Esse princípio trata da coesão nas aulas e visa tornar o design de software mais compreensível, flexível e sustentável.

Vamos considerar um exemplo no contexto de um aplicativo NestJS:

```typescript
// Before applying SRP
import { Injectable } from '@nestjs/common';

@Injectable()
export class UserService {
  createUser() {
    // code to create a user
  }

  deleteUser() {
    // code to delete a user
  }

  sendEmail() {
    // code to send an email
  }
}js
```

No exemplo acima, a classe UserService está lidando com o gerenciamento de usuários e envio de e-mail, o que viola o Princípio de Responsabilidade Única.

Para aderir ao SRP, podemos refatorar o código da seguinte maneira:

```typescript
// After applying SRP
import { Injectable } from '@nestjs/common';

@Injectable()
export class UserService {
  createUser() {
    // code to create a user
  }

  deleteUser() {
    // code to delete a user
  }
}

@Injectable()
export class EmailService {
  sendEmail() {
    // code to send an email
  }
}

```

Agora, temos duas classes separadas, cada uma lidando com uma única responsabilidade. O UserService é responsável pelo gerenciamento de usuários e o EmailService é responsável pelo envio de e-mails. Isso torna nosso código mais sustentável e fácil de entender.

## **2. Princípio Aberto-Fechado (OCP):**

O Princípio Aberto-Fechado afirma que entidades de software (classes, módulos, funções, etc.) devem ser abertas para extensão, mas fechadas para modificação. Isso significa que uma classe deve ser facilmente extensível sem modificar a própria classe.

Vamos ilustrar esse princípio com um exemplo de NestJS:

Antes de aplicar o OCP:

```typescript
// greeter.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class GreeterService {
  greeting(type: string) {
    if (type === 'formal') {
      return 'Good day to you.';
    } else if (type === 'casual') {
      return 'Hey!';
    }
  }
}
```

No exemplo acima, se quisermos adicionar um novo tipo de saudação, teríamos que modificar o método de saudação na classe GreeterService, que viola o Princípio Aberto-Fechado.

Para aderir ao OCP, podemos refatorar o código da seguinte forma:

```typescript
// greeting.interface.ts
export interface Greeting {
  greet(): string;
}

```

```typescript
// formalGreeting.ts
import { Greeting } from './greeting.interface';

export class FormalGreeting implements Greeting {
  greet() {
    return 'Good day to you.';
  }
}
```

```typescript
// casualGreeting.ts
import { Greeting } from './greeting.interface';

export class CasualGreeting implements Greeting {
  greet() {
    return 'Hey!';
  }
}
```

```typescript
// greeter.service.ts
import { Injectable } from '@nestjs/common';
import { Greeting } from './greeting.interface';

@Injectable()
export class GreeterService {
  greeting(greeting: Greeting) {
    return greeting.greet();
  }
}
```

Agora, cada classe e interface é definida em seu próprio arquivo, tornando o código mais organizado e fácil de gerenciar. Se quisermos adicionar um novo tipo de saudação, podemos simplesmente criar uma implementação da interface Greeting, sem modificar as classes existentes. Isso segue o Princípio Aberto-Fechado.

## **3. Princípio de Substituição de Liskov (LSP):**

O Princípio de Substituição de Liskov (LSP) é um conceito em Programação Orientada a Objetos que afirma que em um programa, objetos de uma superclasse devem poder ser substituídos por objetos de uma subclasse sem afetar a correção de o programa. Trata-se de garantir que uma subclasse possa substituir sua classe pai sem quebrar a funcionalidade do seu programa.

Vamos ilustrar esse princípio com um exemplo de NestJS:

```typescript
class Bird {
  fly(speed: number): string {
    return `Flying at ${speed} km/h`;
  }
}

class Eagle extends Bird {
  dive(): void {
    // ...
  }

  fly(speed: number): string {
    return `Soaring through the sky at ${speed} km/h`;
  }
}

// LSP Violation:
class Penguin extends Bird {
  fly(): never {
    throw new Error("Sorry, I can't fly");
  }
}
```

No exemplo acima, a classe Eagle, que herda da classe Bird, substitui o método fly com o mesmo número de argumentos, aderindo ao Princípio de Substituição de Liskov. No entanto, a classe Penguin viola o Princípio de Substituição de Liskov porque não pode voar e, portanto, o método fly gera um erro. Isso significa que um objeto Pinguim não pode ser substituído por um objeto Pássaro sem alterar a exatidão do programa.

Para aderir ao Princípio de Substituição de Liskov, precisamos garantir que as subclasses não alterem o comportamento da classe pai de maneira que possa interromper a funcionalidade do nosso programa. No caso de nosso exemplo Bird, Eagle e Penguin, poderíamos refatorar o código para remover o método fly da classe Bird e, em vez disso, usar interfaces para definir os recursos de diferentes tipos de pássaros.

```typescript
interface FlyingBird {
  fly(speed: number): string;
}

interface NonFlyingBird {
  waddle(speed: number): string;
}

```

Em seguida, definimos nossas classes Eagle e Penguin implementando essas interfaces:

```typescript
class Eagle implements FlyingBird {
  fly(speed: number): string {
    return `Soaring through the sky at ${speed} km/h`;
  }
}

class Penguin implements NonFlyingBird {
  waddle(speed: number): string {
    return `Waddling at ${speed} km/h`;
  }
}
```

Agora, temos duas classes separadas para Eagle e Penguin que implementam diferentes interfaces com base em suas habilidades. Desta forma, não estamos violando o Princípio da Substituição de Liskov porque não estamos fingindo que um pinguim pode voar. Em vez disso, estamos definindo claramente o que cada tipo de ave pode fazer e tratando-os de maneira diferente com base em suas capacidades.

Essa abordagem também torna nosso código mais flexível e fácil de manter. Se precisarmos adicionar um novo tipo de pássaro no futuro, podemos simplesmente criar uma nova classe para ele e implementar a interface apropriada com base em suas habilidades.

## **4. Princípio de Segregação de Interface (ISP):**

O Princípio de Segregação de Interface (ISP) afirma que nenhum cliente deve ser forçado a depender de métodos que não usa. Em outras palavras, os clientes não devem ser forçados a implementar interfaces que não usam. Este princípio promove a criação de interfaces refinadas e específicas do cliente.

O objetivo por trás desse princípio é remover código desnecessário das classes para reduzir erros inesperados quando a classe não tem a capacidade de executar uma ação. O ISP incentiva interfaces menores e mais direcionadas. De acordo com esse conceito, várias interfaces específicas do cliente são preferíveis a uma única interface de uso geral.

Vamos ilustrar esse princípio com um exemplo de TypeScript:

```typescript
interface FullFeatureUser {
  viewAd(): void;
  skipAd(): void;
  startParty(): void;
}

class User {
  viewAd(): void {
    // ...
  }
}

class FreeUser extends User implements FullFeatureUser {
  skipAd(): void {
    throw new Error("Sorry, I can't skip ads");
  }

  startParty(): void {
    throw new Error("Sorry, I can't start parties");
  }
}

class PremiumUser extends User implements FullFeatureUser {
  skipAd(): void {
    // ...
  }

  startParty(): void {
    // ...
  }
}

```

No exemplo acima, a classe FreeUser é forçada a implementar os métodos skipAd e startParty mesmo que não os use. Isso é uma violação do princípio de segregação de interface. Para aderir ao ISP, podemos criar interfaces mais específicas:

```typescript
interface User {
  viewAd(): void;
}

interface PremiumFeatureUser {
  skipAd(): void;
  startParty(): void;
}

class FreeUser implements User {
  viewAd(): void {
    // ...
  }
}

class PremiumUser implements User, PremiumFeatureUser {
  viewAd(): void {
    // ...
  }

  skipAd(): void {
    // ...
  }

  startParty(): void {
    // ...
  }
}

```

Com essas mudanças, cada classe implementa apenas os métodos que utiliza, aderindo ao Princípio de Segregação de Interface. Essa abordagem reduz o risco de bugs e torna nosso código mais flexível e fácil de manter.

## **5. Princípio de Inversão de Dependência (DIP):**

O Princípio de Inversão de Dependência (DIP) é o princípio final na metodologia de projeto SOLID. Ele afirma que os módulos de alto nível não devem depender dos módulos de baixo nível. Em vez disso, ambos devem depender de abstrações. As abstrações não devem depender de detalhes. Detalhes devem depender de abstrações.

O Princípio de Inversão de Dependência é um princípio de design que ajuda a desacoplar os módulos de software. Este princípio desempenha um papel vital no controle do acoplamento entre diferentes módulos de um programa.

Vamos ilustrar esse princípio com um exemplo de TypeScript:

```typescript
class MySQLDatabase {
  save(data: string): void {
    // Save data to MySQL database
  }
}

class UserService {
  private database: MySQLDatabase;

  constructor(database: MySQLDatabase) {
    this.database = database;
  }

  saveUser(user: string): void {
    this.database.save(user);
  }
}

```

No exemplo acima, a classe UserService está fortemente acoplada à classe MySQLDatabase. Isso significa que, se você quiser alterar o sistema de banco de dados (como mudar de MySQL para MongoDB), também precisará alterar a classe UserService. Isso é uma violação do Princípio de Inversão de Dependência.

Para aderir ao DIP, podemos introduzir uma abstração (interface) entre as classes UserService e MySQLDatabase:

```typescript
interface Database {
  save(data: string): void;
}

class MySQLDatabase implements Database {
  save(data: string): void {
    // Save data to MySQL database
  }
}

class UserService {
  private database: Database;

  constructor(database: Database) {
    this.database = database;
  }

  saveUser(user: string): void {
    this.database.save(user);
  }
}
```

Agora, a classe UserService depende da interface Database, não da classe MySQLDatabase concreta. Isso significa que você pode alternar facilmente para um sistema de banco de dados diferente criando uma nova classe que implemente a interface Database. Essa abordagem torna seu código mais flexível e fácil de manter.

## Conclusão:

Os princípios SOLID são um conjunto de princípios de design que ajudam os desenvolvedores a escrever um código limpo, sustentável e escalável. Seguindo esses princípios, você pode criar sistemas de software fáceis de entender, flexíveis e robustos.

Compreendendo e aplicando esses princípios, você aprimora suas habilidades de desenvolvimento NestJS e escreve códigos mais limpos e de fácil manutenção em seus aplicativos. Codificação feliz!
