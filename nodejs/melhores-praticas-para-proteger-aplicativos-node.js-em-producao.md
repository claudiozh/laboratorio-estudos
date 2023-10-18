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

# 🔐 Melhores práticas para proteger aplicativos Node.js em produção

Node.js é uma das tecnologias favoritas dos desenvolvedores quando se trata de desenvolvimento backend. A sua popularidade continua a aumentar e é hoje um dos principais alvos de ataques online. É por isso que é crucial proteger o Node.js contra vulnerabilidades e ameaças.

Neste guia, você verá as 15 práticas recomendadas para desenvolver uma arquitetura de aplicativo Node.js segura para produção. Implemente todos eles para tornar seu back-end mais seguro do que nunca!

## Por que você deve criar um aplicativo Node.js seguro? <a href="#cec7" id="cec7"></a>

Construir um aplicativo Node.js seguro é importante por pelo menos três bons motivos:

1. **Protegendo dados do usuário** : seu aplicativo pode lidar com informações confidenciais do usuário, como detalhes pessoais, credenciais de login, dados de pagamento ou informações comerciais confidenciais. Deixar de proteger esses dados pode custar milhões em multas impostas pelos reguladores de privacidade. Ao implementar medidas de segurança robustas, você pode proteger os dados do usuário e evitar problemas legais.
2. **Protegendo a funcionalidade do aplicativo** : vulnerabilidades de segurança podem comprometer os recursos oferecidos pelo seu back-end. Os invasores podem explorar pontos fracos para alterar seus serviços, manipular dados ou injetar código malicioso. Ao proteger seu aplicativo, você pode evitar isso e fornecer uma experiência perfeita para seus usuários.
3. **Preservando a reputação** : um incidente de segurança pode prejudicar seriamente a sua reputação e minar a confiança no seu serviço. Clientes e usuários esperam que seus dados sejam tratados com segurança e que seu aplicativo funcione conforme esperado. Uma violação pode levar à perda de confiança, mas ao priorizar a segurança você mostra seu compromisso com a qualidade.

Você pode pensar que os problemas de segurança não são tão difundidos e que é suficiente seguir as melhores práticas de codificação e arquitetura, mas isso não é verdade. O poder do Node.js está no ambiente NPM, que oferece milhões de bibliotecas. O problema é que a maioria dos pacotes NPM envolve algumas vulnerabilidades de segurança.

Em outras palavras, seu projeto Node.js não será seguro se suas dependências não o forem. Considerando o quão popular é o uso de bibliotecas externas, isso é preocupante. De acordo com o [relatório Snyk State of Open Source Security](https://snyk.io/reports/open-source-security/) , um projeto Node.js médio tem 49 vulnerabilidades de 79 dependências diretas. Essa estatística preocupante ressalta o quão importante é proteger seu aplicativo Node.js.

## 15 práticas recomendadas para tornar seu aplicativo Node.js mais seguro <a href="#144b" id="144b"></a>

Vamos dar uma olhada nas práticas recomendadas mais populares para proteger seu projeto Node.js.

## 1. Nunca execute Node.js com privilégios de root <a href="#0494" id="0494"></a>

Executar o Node.js com privilégios de root não é recomendado, pois vai contra o [princípio do menor privilégio](https://en.wikipedia.org/wiki/Principle\_of\_least\_privilege) . Não importa se o seu backend está em um servidor dedicado ou contêiner Docker, você deve sempre iniciá-lo como um usuário não root,

Se, em vez disso, você executar o Node.js com privilégios de root, quaisquer vulnerabilidades em seu projeto ou em suas dependências poderão ser potencialmente exploradas para obter acesso não autorizado ao seu sistema. Por exemplo, um invasor pode aproveitá-los para executar código arbitrário, acessar arquivos confidenciais ou até mesmo assumir o controle de toda a máquina. Portanto, o uso de usuários root para Node.js deve ser evitado.

A prática recomendada aqui é criar um usuário dedicado para executar o Node.js. Este usuário deve ter apenas as permissões necessárias para iniciar o aplicativo. Dessa forma, os invasores que conseguirem comprometer seu back-end ficarão restritos aos privilégios desse usuário, limitando os danos potenciais que podem causar.

## 2. Mantenha suas bibliotecas NPM atualizadas <a href="#74aa" id="74aa"></a>

As bibliotecas NPM tornam mais fácil e rápido construir um backend Node.js completo. Ao mesmo tempo, eles também podem introduzir riscos de segurança na sua aplicação. Novas vulnerabilidades são descobertas o tempo todo e é trabalho dos mantenedores resolvê-las e lançar uma versão atualizada do pacote. Veja por que você deve manter suas dependências atualizadas.

Para garantir que as bibliotecas NPM nas quais você confia sejam seguras, você pode usar [`npm audit`](https://docs.npmjs.com/cli/v9/commands/npm-audit)e [`snyk`](https://www.npmjs.com/package/snyk). Essas ferramentas analisam a árvore de dependências do seu projeto e fornecem insights sobre quaisquer vulnerabilidades conhecidas.

Aqui está um exemplo de execução `npm audit`:

```
auditoria npm 
... 
encontrou 4 vulnerabilidades ( 2 baixas, 2 moderadas) 
execute `npm audit fix` para corrigi-las ou `npm audit` para obter detalhes
```

Este comando aproveita o [banco de dados consultivo GitHub](https://github.com/advisories) e verifica seus `package.json`problemas `package-lock.json`de segurança conhecidos.

Além disso, você pode usar [o Snyk](https://snyk.io/) para verificar suas dependências no [banco de dados de vulnerabilidades de código aberto do Snyk](https://snyk.io/vuln/) . Instale `snyk`com:

```
npm instalar -g snyk
```

Na pasta raiz do seu projeto, teste sua aplicação com:

```
teste de snyk
```

Para abrir um assistente que orientará você no processo de correção das vulnerabilidades encontradas, execute:

```
mago snyk
```

Usar `snyk`e executar regularmente `npm audit`ajuda a identificar e corrigir problemas de segurança em suas bibliotecas NPM antes que eles se tornem um problema. Lembre-se de que a segurança do seu aplicativo é tão forte quanto o elo mais fraco das suas dependências.

## 3. Evite usar nomes de cookies padrão <a href="#d512" id="d512"></a>

Os nomes dos cookies usados ​​pelo seu aplicativo Node.js podem revelar involuntariamente a pilha de tecnologia na qual seu back-end se baseia. Essa é uma informação valiosa que você deve sempre ocultar, pois os invasores podem usá-la novamente para você. Ao saber qual estrutura você está usando, eles podem explorar pontos fracos específicos associados a ela.

Em detalhes, os invasores tendem a se concentrar no nome do cookie de sessão. Proteja seu aplicativo contra isso definindo um nome de cookie de sessão personalizado com o [`express-session`](https://www.npmjs.com/package/express-session)middleware:

```
const expresso = exigir ( 'expresso' ); 
sessão const = requer ( 'sessão expressa' );
```

```
const app=express();app.use(session({ 
  // define um nome personalizado para o 
  nome do cookie de sessão: 'myCustomCookieName', 
  // uma chave secreta segura para 
  segredo de criptografia de sessão: 'mySecretKey', 
}));
```

## 4. Defina os cabeçalhos HTTP de segurança <a href="#84a0" id="84a0"></a>

Os cabeçalhos HTTP padrão no Express não são muito seguros. Verificamos a segurança do cabeçalho com o [projeto online Security Headers](https://securityheaders.com/) :

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*GuskoPa7YIwhiZnX.png" alt="" height="319" width="700"><figcaption></figcaption></figure>

Alguns desses cabeçalhos contêm informações que não devem ser expostas publicamente, como `X-Powered-By`. Outros estão faltando e devem ser adicionados para lidar com vários aspectos relacionados à segurança, incluindo a prevenção de ataques [de cross-site scripting](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site\_scripting) (XSS).

É aqui que [`helmet`](https://www.npmjs.com/package/helmet)entra em jogo! Esta biblioteca se encarrega de definir os cabeçalhos de segurança mais importantes com base nas recomendações dos Cabeçalhos de Segurança. Use-o da seguinte maneira:

```
const expresso = exigir ( 'expresso' ); 
const capacete = exigir ( 'capacete' );
```

```
const app=express();// registra o middleware do capacete 
// para definir os cabeçalhos de segurança 
app.use(helmet());
```

O `helmet()`middleware remove automaticamente cabeçalhos inseguros e adiciona novos, incluindo `X-XSS-Protection`, `X-Content-Type-Options`, `Strict-Transport-Security`e `X-Frame-Options`. Eles impõem práticas recomendadas e ajudam a proteger seu aplicativo contra ataques comuns.

Os cabeçalhos definidos pelo seu aplicativo Node.js agora serão considerados seguros:

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*Bx1t5ELqckp7MOfr.png" alt="" height="329" width="700"><figcaption></figcaption></figure>

## 5. Implementar limitação de taxa <a href="#5f43" id="5f43"></a>

DDoS (negação de serviço distribuída) e força bruta são dois dos ataques mais comuns na web. Para mitigá-los, você pode implementar [a limitação de taxa](https://en.wikipedia.org/wiki/Rate\_limiting) . Essa técnica envolve controlar o tráfego de entrada para o back-end do Node.js, evitando que agentes mal-intencionados sobrecarreguem seu servidor com muitas solicitações.

A maneira mais fácil de implementar a limitação de taxa é por meio da [`rate-limiter-flexible`](https://www.npmjs.com/package/rate-limiter-flexible)biblioteca. Essa dependência fornece um middleware configurável para restringir o número de solicitações provenientes do mesmo endereço IP ou usuário dentro de um período de tempo especificado.

Aqui está um exemplo de como usá-lo para aplicar limitação de taxa em Node.js:

```
const expresso = exigir ( 'expresso' ); 
const { RateLimiterMemory } = require ( 'limitador de taxa flexível' );
```

```
const app=express();const rateLimiter = new RateLimiterMemory({ 
  pontos: 10, // número máximo de solicitações permitidas 
  duração: 1, // período de tempo em segundos 
});const rateLimiterMiddleware = (req, res, next) => { 
   rateLimiter.consume(req.ip) 
      .then(() => { 
          // solicitação permitida, 
          // prosseguir com o tratamento da solicitação 
          next(); 
      }) 
      .catch( () => { 
          // limite de solicitação excedido, 
          // responde com uma mensagem de erro apropriada 
          res.status(429).send('Too Many Requests'); 
      }); 
   };app.use(rateLimiterMiddleware);
```

Primeiro, uma instância do limitador de taxa que permite um máximo de 10 solicitações em 1 segundo é inicializada. Em seguida, ele é usado em um middleware customizado para analisar o IP da solicitação recebida. Se o limite de taxa não for excedido, a solicitação prossegue. Caso contrário, a solicitação será bloqueada e o servidor retornará uma [`429 Too Many Requests`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429)resposta.

## 6. Garanta políticas de autenticação fortes <a href="#f9d7" id="f9d7"></a>

Para proteger seu aplicativo Node.js contra ataques que exploram a autenticação do usuário, você precisa impor políticas de autenticação fortes. Primeiro, convide os usuários a definirem senhas fortes. Além disso, você deve oferecer suporte à [autenticação multifator](https://en.wikipedia.org/wiki/Multi-factor\_authentication) (MFA) e [ao logon único](https://en.wikipedia.org/wiki/Single\_sign-on) (SSO). A MFA adiciona uma camada extra de segurança ao exigir que os usuários forneçam diversas formas de autenticação, enquanto o SSO simplifica o processo de autenticação e reduz o risco de senhas fracas ou reutilizadas.

Quando se trata de hash de senhas para armazenamento, prefira funções criptográficas fortes, como [`bcrypt`](https://www.npmjs.com/package/bcrypt)os métodos oferecidos pela biblioteca de criptografia Node.js. Esse pacote fornece um algoritmo seguro de hash de senha que torna significativamente mais difícil para os invasores quebrarem senhas. Por fim, mitigue ataques de força bruta restringindo o número de tentativas de login malsucedidas por meio da limitação de taxa, conforme explicado anteriormente.

## 7. Não envie informações desnecessárias <a href="#bc70" id="bc70"></a>

Qualquer informação fornecida involuntariamente ao invasor pode ser usada contra você. Por esse motivo, as respostas do servidor devem conter apenas o que o chamador precisa estritamente. Por exemplo, evite retornar mensagens de erro detalhadas ou rastreamentos de pilha diretamente ao cliente. Em vez disso, forneça mensagens de erro genéricas que não revelem detalhes específicos de implementação. A maneira mais fácil de fazer isso é executar o Node.js no modo de produção configurando o `NODE_ENV=production`env, caso contrário, o Express adicionará o rastreamento de pilha nas respostas de erro.

Da mesma forma, você deve ter cuidado com os dados incluídos nas respostas da API. Retorne apenas os campos de dados necessários e evite expor informações confidenciais não solicitadas pelo chamador. Isto minimizará o risco de divulgação acidental de informações confidenciais ou privilegiadas.

## 8. Monitore seu back-end <a href="#97ca" id="97ca"></a>

Seu back-end em produção pode estar sob ataque e você pode nem estar ciente disso. Veja por que é essencial monitorar seu aplicativo Node.js. Ao conectá-lo a uma ferramenta [de monitoramento de desempenho de aplicativos](https://en.wikipedia.org/wiki/Application\_performance\_management) (APM), você pode acompanhá-lo para identificar problemas de segurança e garantir sua integridade geral.

Felizmente, existem várias bibliotecas e serviços APM disponíveis para Node.js. Alguns dos mais populares são [SigNoz](https://github.com/SigNoz/signoz) , [Sentry](https://github.com/getsentry/sentry-javascript) , [Prometheus](https://github.com/prometheus/prometheus) , [New Relic](https://github.com/newrelic/node-newrelic) e [Elastic.](https://github.com/elastic/apm-agent-nodejs) Eles fornecem informações sobre vários aspectos do aplicativo, incluindo desempenho, taxas de erro, uso de recursos e métricas relacionadas à segurança. Em particular, permitem a recolha de dados em tempo real e a deteção de anomalias ou atividades suspeitas que possam indicar uma violação de segurança. Alguns deles também oferecem recursos de osservabilidade para rastrear o fluxo de trabalho de implantação em seu pipeline de CI/CD.

## 9. Adote uma política somente HTTPS <a href="#ef86" id="ef86"></a>

Ao garantir que seu back-end seja acessível apenas via HTTPS, você melhorará a confidencialidade dos dados trocados entre clientes e seu servidor Node.js. HTTPS estabelece um canal criptografado que protege informações confidenciais, como senhas, tokens de sessão e dados do usuário, contra interceptação.

Como parte dessa política, você também deve usar [cookies HTTPS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) . Para conseguir isso, certifique-se de que todos os cookies definidos pelo seu aplicativo Node.js estejam marcados como [`secure`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#restrict\_access\_to\_cookies)[e](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#restrict\_access\_to\_cookies)[`httpOnly`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#restrict\_access\_to\_cookies) \`:

```
res. cookie ( 'myCookie' , 'cookieValue' , { 
  // cria um cookie HTTPS 
  seguro : true , 
  httpOnly : true , 
});
```

Pessoas ou scripts não intencionais não poderão mais acessar seus cookies. Além disso, eles serão transmitidos exclusivamente por meio de conexões HTTPS.

## 10. Valide a entrada do usuário <a href="#5c23" id="5c23"></a>

Sempre que os usuários têm a oportunidade de inserir informações, os invasores podem explorar isso para enviar dados maliciosos ao servidor. Portanto, validar a entrada do usuário é fundamental para garantir a segurança e a integridade de uma aplicação Node.js.

Graças a bibliotecas como [`express-validator`](https://www.npmjs.com/package/express-validator), você pode impor regras rígidas de validação no corpo e nos parâmetros de consulta das solicitações recebidas. Somente dados no formato esperado poderão passar, evitando comportamentos inesperados devido a entradas imprevistas.

## 11. Use linters de segurança <a href="#d020" id="d020"></a>

Os linters de segurança analisam sua base de código para identificar vulnerabilidades, seções de código inseguras e violações de práticas recomendadas. Um dos mais populares é [`eslint-plugin-security`](https://www.npmjs.com/package/eslint-plugin-security)um conjunto de regras ESLint para reforçar o desenvolvimento de segurança em Node.js.

Ao integrar essas ferramentas ao seu fluxo de trabalho de desenvolvimento, você pode identificar e resolver problemas de segurança desde o início. Especificamente, eles reduzem o risco de introdução de vulnerabilidades em seu aplicativo durante a codificação. Essas ferramentas são particularmente eficazes quando adicionadas e integradas ao pipeline de CI/CD.

## 12. Impedir injeção de SQL <a href="#4389" id="4389"></a>

[A injeção de SQL](https://en.wikipedia.org/wiki/SQL\_injection) é uma vulnerabilidade de segurança comum que ocorre quando um invasor pode manipular dados de entrada transmitidos para uma consulta SQL. Isso geralmente acontece ao concatenar a entrada do usuário diretamente em consultas SQL. Nesse cenário, os invasores podem forjar entradas específicas destinadas a executar código SQL arbitrário, levando a acesso não autorizado e violações de dados.

Existem várias maneiras de evitar a injeção de SQL no Node.js. Os mais populares são:

* **Use instruções preparadas ou consultas parametrizadas** : essas técnicas envolvem a separação do código SQL da entrada do usuário, evitando que seja interpretado como parte da consulta.
* **Sanitização de entrada** : valide a entrada do usuário para rejeitar dados maliciosos, reduzindo o risco de ataques de injeção de SQL.
* **Use um ORM** : tecnologias ORM como Sequelize geralmente fornecem proteção integrada contra injeção de SQL.

## 13. Limitar o tamanho da solicitação <a href="#c8aa" id="c8aa"></a>

O limite de tamanho do corpo da solicitação padrão no Node.js é 5 MB. Para proteger seu back-end de ataques DDoS em que usuários mal-intencionados tentam inundar seu servidor com dados, é recomendável reduzir esse limite. Para fazer isso, você pode usar a [`body-parser`](https://www.npmjs.com/package/body-parser)biblioteca e configurá-la conforme abaixo:

```
const expresso = exigir ( 'expresso' ); 
const bodyParser = require ( 'body-parser' );
```

```
const app=express();// define o limite de tamanho da solicitação para 1 MB 
app.use(bodyParser.json({ limit: '1mb' }));
```

Ajuste o valor de acordo com suas necessidades específicas. Agora, solicitações com corpo maior que 1 MB serão bloqueadas imediatamente, evitando que o servidor aloque recursos para processá-las.

## 14. Detecte vulnerabilidades por meio de ferramentas automatizadas <a href="#d41d" id="d41d"></a>

Ferramentas automatizadas de verificação de vulnerabilidades, como SonarQube ou similares, são recursos valiosos para identificar problemas de segurança em aplicativos Node.js. Essas ferramentas realizam varreduras abrangentes da base de código, dependências, configurações e outros componentes para identificar pontos fracos de segurança.

Aqui estão alguns dos principais benefícios do uso de scanners de vulnerabilidade automatizados:

* **Detecção antecipada** : identifique proativamente problemas de segurança antes de implantar seu aplicativo.
* **Maior cobertura** : execute verificações aprofundadas de todos os arquivos do projeto, garantindo alta cobertura de segurança.
* **Monitoramento contínuo** : integre-os ao seu pipeline de CI/CD para garantir que quaisquer novas vulnerabilidades introduzidas por meio de alterações de código sejam descobertas imediatamente.

## 15. Facilite o relato de vulnerabilidades <a href="#8d17" id="8d17"></a>

Oferecer aos usuários e pesquisadores de segurança a capacidade de relatar vulnerabilidades encontradas no back-end do Node.js é outro aspecto importante para manter seu aplicativo seguro. Isto não só deve ser possível, mas o procedimento deve ser claro e acessível.

Uma abordagem eficaz para permitir que os pesquisadores entrem em contato com você é o [`security.txt`](https://en.wikipedia.org/wiki/Security.txt)padrão proposto. Este é um arquivo de texto simples colocado na raiz do seu projeto que fornece informações sobre como relatar vulnerabilidades de segurança. Segue um formato padronizado e inclui detalhes de contato, métodos de criptografia e diretrizes de divulgação.

Aqui está um exemplo de `security.txt`arquivo básico:

```
Contato: email @example .com 
Criptografia: https: //example.com/pgp-key.asc
```

`Contact`especifica o endereço de e-mail para o qual vulnerabilidades ou preocupações de segurança devem ser relatadas. Esses e-mails podem conter informações críticas e não devem ser acessíveis ao público. Assim, o `Encryption`campo indica a localização da chave pública [PGP](https://en.wikipedia.org/wiki/Pretty\_Good\_Privacy) da organização que pode ser utilizada para criptografar mensagens dentro dos e-mails. Este mecanismo garante que apenas a organização possa descriptografar essas mensagens com a chave privada e lê-las.

Da mesma forma, você também pode considerar adicionar uma página “Relatar vulnerabilidade” em seu site.

## Conclusão <a href="#9555" id="9555"></a>

Neste guia, discutimos maneiras de proteger aplicativos Node.js em produção. Como desenvolvedores, é nosso dever proteger os dados dos usuários, manter a funcionalidade dos aplicativos e preservar nossa reputação.

Ao integrar essas técnicas, abordagens e dicas, você pode criar uma arquitetura Node.js mais confiável, mitigando riscos e garantindo a segurança da sua aplicação.

\
