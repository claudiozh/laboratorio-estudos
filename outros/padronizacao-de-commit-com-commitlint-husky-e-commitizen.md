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

# 🖱 Padronização de commit com (Commitlint, Husky e Commitizen)

### Introdução <a href="#introducao" id="introducao"></a>

Sem dúvidas uma das ferramentas mais importantes para uma pessoa desenvolvedora é o Git. Com essa ferramenta conseguimos fazer o versionamento do nosso código, mantendo um histórico de todas as alterações.

O Git cria uma linha do tempo das modificações feitas em nosso projeto e cada ponto nessa linha do tempo é definido por um commit, que é o ato de empacotar aquelas alterações que foram feitas.

Ao criar um commit, sempre precisamos inserir uma mensagem para o mesmo, que de preferência seja curta, objetiva e descritiva. Se você está aprendendo Git, sua maior preocupação não vai ser criar a melhor mensagem para o seu commit, e nem deveria ser.

Em um primeiro momento, é interessante que você domine as funções básicas do Git, e depois disso dê o próximo passo buscando entender práticas como commits atômicos e padronização das mensagens de commits.

Iniciarei contextualizando sobre as práticas citadas e as bibliotecas que uso para fazer a padronização dos commits e na sequência irei configurar a instalação e configuração das bibliotecas. Dessa maneira, fica mais prático de revisitar o artigo e seguir o passo a passo para configurar futuros projetos

### Commits atômicos <a href="#commits-atomicos" id="commits-atomicos"></a>

A prática de criar commits atômicos consiste em criar um commit para cada modificação no projeto, por exemplo, vamos imaginar que estamos trabalhando em um projeto e fizemos duas ações:

criamos um novo componente fizemos alterações em um já existente Em vez de criarmos um único commit para guardar as alterações que fizemos, faremos dois commits, seguindo um padrão de commits atômicos. O primeiro commit será com os arquivos envolvidos na criação do novo componente, e o segundo com as alterações feitas em um componente já existente.

Dessa maneira conseguimos escrever uma mensagem mais descritiva para o commit, as ações na linha do tempo ficam mais descritivas e é mais prático de navegar pelos commits visualizando as modificações. Na ocorrência de um bug no projeto, é bem mais fácil de encontrar em qual commit ele foi inserido e reverter o que foi feito.

### Padronização dos commits <a href="#padronizacao-dos-commits" id="padronizacao-dos-commits"></a>

Outra prática muito importante e que está ligada a anterior é a padronização das mensagens dos commits, dessa maneira seguimos uma estrutura na hora de escrevermos as mensagens, o que deixa os commits estruturados e padronizados.

Quando iniciei os estudos com o Git, imaginei se existiria uma regra ou padrão a se seguir ao criar os commits. Esse interesse se despertou a um tempo atrás, quando vi um vídeo no canal do Dev Soutinho falando sobre isso, vou deixar o link para você conferir depois:

[Como padronizar commits? #DevTips Conventional Commits + Semantic Release para releases e versioning](https://www.youtube.com/watch?v=1eTofdmfq1g)

### Conventional Commits na prática <a href="#conventional-commits-na-pratica" id="conventional-commits-na-pratica"></a>

O commit tem que seguir a seguinte estrutura:

> \<tipo>\[escopo opcional]: \<descrição>

> \[corpo opcional]

> \[rodapé(s) opcional(is)]

A mensagem deve ser escrita com letras minúsculas, com um espaço entre o dois pontos e a descrição e sem ponto final.

Geralmente eu escrevo apenas a primeira linha, vou colocar a lista com os tipos que podemos utilizar:

* **chore**: Atualização de tarefas que não ocasionam alteração no código de produção, mas mudanças de ferramentas, mudanças de configuração e bibliotecas.
* **feat**: São adições de novas funcionalidades ou de quaisquer outras novas implantações ao código.
* **fix**: Essencialmente definem o tratamento de correções de bugs.
* **refactor**: Utilizado em quaisquer mudanças que sejam executados no código, porém não alterem a funcionalidade final da tarefa impactada. **docs**: Inclusão ou alteração somente de arquivos de documentação.
* **perf**: Uma alteração de código que melhora o desempenho.
* **style**: Alterações referentes a formatações na apresentação do código que não afetam o significado do código, como por exemplo: espaço em branco, formatação, ponto e vírgula ausente etc.
* **test**: Adicionando testes ausentes ou corrigindo testes existentes nos processos de testes automatizados (TDD).
* **build**: Alterações que afetam o sistema de construção ou dependências externas (escopos de exemplo: gulp, broccoli, npm).
* **ci**: Mudanças em nossos arquivos e scripts de configuração de CI (exemplo de escopos: Travis, Circle, BrowserStack, SauceLabs).
* **env**: Utilizado na descrição de modificações ou adições em arquivos de configuração em processos e métodos de integração contínua (CI), como parâmetros em arquivos de configuração de containers.

**Exemplos de Commits:**

* chore: add commitlint e husky
* chore(eslint): obrigar o uso de aspas duplas no jsx
* refactor: refatorando a tipagem
* feat: add axios / buscando e tratando os dados
* feat(page/home): criando o roteamentento no next

[Especificação do Conventional Commits](https://www.conventionalcommits.org/pt-br/v1.0.0/) [O que é Commit e como usar Commits Semânticos](https://blog.geekhunter.com.br/o-que-e-commit-e-como-usar-commits-semanticos/)

### Semantic Versioning (Versionamento Semântico) <a href="#semantic-versioning-versionamento-semantico" id="semantic-versioning-versionamento-semantico"></a>

Ele padroniza o versionamento das bibliotecas que usamos no dia a dia. Estudando-o, você entenderá o porquê dos números que seguem o nome do pacote que você instalou no seu projeto, localizado no arquivo package.json, por exemplo:

```
"dependencies": {
    "@chakra-ui/react": "^1.6.2",
    "@emotion/react": "^11",
    "@emotion/styled": "^11",
    "framer-motion": "^4",
    "next": "10.2.3",
    "react": "17.0.2",
    "react-dom": "17.0.2"
}
```

É muito importante pelo menos entender como funciona o Semantic Versioning, vai te ajudar em diversas situações. Além disso, te ajudará a ter uma compreensão maior na hora de entender o lançamento de uma biblioteca, a evitar uma versão que está com problemas ou entender quando o problema de uma biblioteca foi resolvido. É um conhecimento que só tem a acrescentar na sua vida como dev

### Commitlint <a href="#commitlint" id="commitlint"></a>

Entendemos o Conventional Commits, mas nada garante que vamos respeitar as regras impostas, pois, mesmo trabalhando sozinho em um projeto, pode ser que esqueçamos de seguir o padrão que foi definido.

É ai que entra o commitlint, com ele conseguimos verificar se a mensagem de commit que escrevemos realmente está dentro dos padrões pré definidos. Vamos usar os padrões do Angular, mas ele pode ser alterado e podemos até mesmo criar o nosso próprio padrão.

Antes de fazermos um commit, vamos rodar a biblioteca para fazer essa verificação. Se a mensagem do commit não estiver seguindo o padrão, será gerado um erro no terminal.

### Husky <a href="#husky" id="husky"></a>

O Husky vai nos ajudar a criarmos ganchos para o Git de uma maneira simples. Os ganchos são ações que vão ser disparadas em determinados momentos. Nesse caso, vamos criar um gancho para ser disparado antes de um commit ser inicializado.

Dessa maneira, sempre que fizermos um commit, vamos configurar o Husky para executar o Commitlint e verificar se a mensagem do commit está seguindo os padrões recomendados.

Com isso, automatizamos o processo de verificação da mensagem e não precisamos nos preocupar em rodá-lo manualmente. Mesmo com o Commitlint, pode ser que você esqueça de fazer a verificação e não queremos que isso aconteça, mas dessa maneira nenhum commit com a mensagem errada vai passar.

### Commitizen <a href="#commitizen" id="commitizen"></a>

Chegamos na última biblioteca que iremos utilizar.

O Commitizen é uma biblioteca que vai nos ajudar a criar os commits seguindo o padrão do Conventional Commit. Ela gera uma interface no terminal e assim vamos conseguir acessar todos os tipos de commits e suas descrições:

![](https://i.imgur.com/Zg2DvnY.png)

Ao adotar um novo padrão como estamos fazendo, precisamos de um tempo até decorarmos os tipos e não precisar ficar consultando a documentação para conferir qual tipo usar. É aí que essa biblioteca vai nos ajudar.

Iremos criar um script que podemos rodar sempre que quisermos fazer um commit guiado. Dessa maneira, só precisamos seguir o passo a passo que a biblioteca implementa e geraremos um commit dentro do padrão.

Nem sempre vamos precisar usá-la, mas gosto de deixar instalada, pois se precisar é só executar o script.

### Instalando e configurando as bibliotecas <a href="#instalando-e-configurando-as-bibliotecas" id="instalando-e-configurando-as-bibliotecas"></a>

Basta criar um projeto simples com o yarn ou npm, abrindo uma pasta no seu editor e rodando esse comando no terminal:

```
yarn init -y

# ou 

npm init -y
```

Com o projeto iniciado, podemos começar a instalar as bibliotecas, todas vão ser instaladas como dependências de desenvolvimento, não vamos usá-las no ambiente de produção.

### Instalando o commitlint <a href="#instalando-o-commitlint" id="instalando-o-commitlint"></a>

* [Link para o site da biblioteca](https://commitlint.js.org/#/guides-local-setup)

```
# Instalando e configurando o commitlint
yarn add @commitlint/config-conventional @commitlint/cli -D

echo "module.exports = { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
```

Podemos testar pra ver se deu tudo certo, o primeiro teste precisa gerar um erro porque não está seguindo o padrão do Conventional Commits. O segundo teste deve passar.

```
# Para testar a biblioteca
echo "teste" | yarn commitlint

echo "feat: teste" | yarn commitlint
```

### Instalando o husky <a href="#instalando-o-husky" id="instalando-o-husky"></a>

No site do commitlint vai ter as instruções para instalar o husky também, com yarn ou npm.

```
# Install Husky 

npm install husky --save-dev
or
yarn add husky --dev

# Activate hooks

npx husky install
or
yarn husky install

# Para ativar os hooks automaticamente após a instalação
"scripts": {
  "prepare": "husky install"
}

npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
or
yarn husky add .husky/commit-msg 'yarn commitlint --edit $1'
```

É muito importante criar o script de prepare no arquivo package.json, caso contrário, quando você ou outra pessoa baixar o projeto do GitHub, o husky não vai funcionar, e então teria que rodar o comando yarn husky install manualmente. Com o script, deixamos essa instalação automatizada.

Agora podemos fazer um commit para verificar se está tudo certo.

```
# Primeiro precisamos adicionar os arquivos para serem commitados
git add .

# Vamos fazer o commit falhar para ver se tudo está funcionando
# Se o commit passar, algum passo até aqui deu errado
git commit -m "qualquer coisa"

# Agora vamos fazer o commit corretamente, nesse caso eu vou usar o tipo chore
# Por se tratar de uma configuração no ambiente de desenvolvimento
git commit -m "chore: add commitlint e husky"
```

### Instalando o commitizen <a href="#instalando-o-commitizen" id="instalando-o-commitizen"></a>

* \[Link para o site da biblioteca]\([http://commitizen.github.io/cz-cli](http://commitizen.github.io/cz-cli)

```
# Instalando o commitizen
npm install commitizen -D

# Criando a configuração
npx commitizen init cz-conventional-changelog --save-dev --save-exact

# Add um script no package.json para disparar o commitizen
"scripts": {
  "commit": "git-cz"
}
```

Com a biblioteca instalada e configurada, vamos fazer um commit para entender o passo a passo que ela traz na prática.

```
# Primeiro, precisamos adicionar os arquivos para serem commitados
git add .

# Agora vamos executar o script para iniciar a biblioteca
npm run commit
```

Ao iniciar a biblioteca ela vai mostrar um passo a passo:

1. Select the type of change that you’re committing: (Use arrow keys)

Selecione o tipo de mudança que você está realizando: (Use as teclas de seta)

É só usar as setas e enter para selecionar um tipo.

2. What is the scope of this change (e.g. component or file name): (press enter to skip)

Qual é o escopo desta mudança (por exemplo, componente ou nome do arquivo): (pressione Enter para pular)

Exemplo: chore(eslint): obrigar o uso de aspas duplas no jsx. Em vez de colocar na mensagem do commit onde estava sendo feita a modificação, eu passei essa informação no escopo e a mensagem ficou mais sucinta.

3. Write a short, imperative tense description of the change (max 82 chars):

Escreva uma descrição breve e imperativa da mudança (máx. 82 caracteres):

Utilizando o exemplo a cima, essa é a mensagem que sucede o dois pontos: obrigar o uso de aspas duplas no jsx

4. Provide a longer description of the change: (press enter to skip)

Forneça uma descrição mais longa da mudança: (pressione Enter para pular)

Essa opção é opcional e geralmente eu não preencho, mas você pode inserir um contexto mais amplo que descreve a modificação que foi feita.

5. Are there any breaking changes? (y/N)

Existem alterações importantes? (y/N)

A tradução não descreve bem essa opção, ela está ligada a quebra de compatibilidade estipulada pelo Semantic Versioning, se tiver conhecimento sobre isso, vai saber a resposta, senão quer dizer que não está fazendo versionamento e pode pular sem medo. OBS: Sempre que estiver trabalhando no terminal e ele mostrar duas opções, sendo uma delas maiúscula, se pressionar enter ele seleciona a maiúscula que no caso é o N, ou você pode digitar N e pressionar enter.

6. Does this change affect any open issues? (y/N)

Essa mudança afeta algum problema em aberto? (y/N)

Essa opção está ligada as Issues do GitHub, é uma opção mais avançada e não vou abordá-la nesse artigo, é só pressionar enter. E pronto, o commit vai ser realizado.

Podemos digitar o comando git log no terminal para ver a lista com os últimos commits, e em primeiro lugar vai estar o commit gerado pelo commitizen.
