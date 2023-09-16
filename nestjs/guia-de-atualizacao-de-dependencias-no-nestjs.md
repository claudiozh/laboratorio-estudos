---
description: >-
  Guia para atualizar dependências no NestJS: aprenda a instalar ferramentas,
  atualizar pacotes e manter seu projeto NestJS eficiente.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🐈⬛ Guia de Atualização de Dependências no NestJS

Instalar globalmente duas ferramentas de linha de comando: o `@nestjs/cli`, que é a interface de linha de comando oficial do NestJS, e o `npm-check-updates`, uma ferramenta útil para verificar e atualizar as versões das dependências do projeto.

```bash
npm install -g @nestjs/cli npm-check-updates
```

Abrir projeto NestJS:&#x20;

* Após instalar as ferramentas e atualizar as dependências, é necessário abrir o projeto do NestJS em um editor de código ou IDE.

Atualizar todas as dependências do NestJS que começam com `@nestjs` para suas versões mais recentes. O sinalizador `-u` indica que as versões devem ser atualizadas.

```bash
npm-check-updates "/@nestjs*/" -u
```

Atualizar todas as dependências do projeto que começam com `nestjs` (em minúsculas) para suas versões mais recentes. O sinalizador `-u` indica que as versões devem ser atualizadas.

```bash
npm-check-updates "/nestjs*/" -u
```

Instalar todas as dependências atualizadas e adicionadas ao projeto.

```bash
npm install
```

Em resumo, esses comandos são utilizados para atualizar todas as dependências do projeto NestJS para suas versões mais recentes, garantindo que o projeto esteja utilizando as versões mais recentes dos pacotes necessários.

\
\
