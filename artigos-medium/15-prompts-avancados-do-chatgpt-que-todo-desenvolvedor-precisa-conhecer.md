---
icon: head-side-brain
---

# 15 prompts avançados do ChatGPT que todo desenvolvedor precisa conhecer

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*-Dqdqrx9uvMLRyAf" alt="Scripts de automação do ChatGPT para desenvolvedores | Técnicas de refatoração de código orientadas por IA | ChatGPT em fluxos de trabalho de integração contínua | Usando o ChatGPT para revisões de código de segurança | Dicas de otimização de desempenho do ChatGPT" height="467" width="700"><figcaption><p>Foto de <a href="https://unsplash.com/@solenfeyissa?utm_source=medium&#x26;utm_medium=referral">Solen Feyissa</a> no <a href="https://unsplash.com/?utm_source=medium&#x26;utm_medium=referral">Unsplash</a></p></figcaption></figure>

A maioria dos desenvolvedores está apenas pedindo para **o ChatGPT** "escrever uma função" ou "explicar esse erro". Isso é coisa de nível básico.

E se você pudesse automatizar a refatoração, gerar documentação completa do projeto em segundos ou transformar **o ChatGPT** em seu assistente de depuração personalizado com tecnologia de IA?

Este artigo aborda técnicas **avançadas do ChatGPT** que economizarão horas de codificação, depuração e otimização.

Sem rodeios. Apenas estratégias e dicas **contundentes** , práticas e **acionáveis** .

Vamos começar.

## 1. Solicitação avançada de cadeia de pensamento <a href="#b295" id="b295"></a>

A cadeia de pensamento padrão é útil, mas os desenvolvedores podem levá-la mais além com estruturas de raciocínio explícitas:

**Estratégia rápida:**

```
Aborde este problema usando a estrutura de resolução de problemas IDEAL: 

1. Identifique o problema precisamente: [DESCREVA O PROBLEMA ESPECÍFICO] 
2. Defina as restrições e os requisitos: [LISTE TODAS AS RESTRIÇÕES TÉCNICAS] 
3. Explore estratégias potenciais: gere pelo menos três abordagens diferentes 
4. Aja com a melhor estratégia: implemente a solução com código limpo e documentado 
5. Olhe para trás e aprenda: avalie a eficiência da solução, os casos extremos e as melhorias potenciais 

Problema a ser resolvido: [SEU DESAFIO DE DESENVOLVIMENTO ESPECÍFICO]
```

Essa abordagem estruturada garante que o modelo explore diversas soluções antes de se comprometer com uma.

Também incentiva uma análise mais completa de compensações e casos extremos que, de outra forma, poderiam ser ignorados.

## 2. Preparação para entrevistas para FAANG ou multinacionais <a href="#id-58d8" id="id-58d8"></a>

Se você estiver se preparando para alguma entrevista técnica, pode seguir isto.

**Estratégia rápida:**

```
Estou me preparando para entrevistas técnicas em empresas de tecnologia de ponta (FAANG). 
Crie um plano de preparação personalizado e uma sessão de coaching com base na minha formação: 

PERFIL DO CANDIDATO: 
- Nível de experiência: [Júnior/Médio/Sênior] 
- Formação técnica: [Suas principais linguagens, frameworks, especializações] 
- Funções-alvo: [Engenheiro de software/Engenheiro de ML/Cientista de dados/etc.] 
- Empresas-alvo: [Empresas específicas que você está almejando] 
- Cronograma da entrevista: [Quando suas entrevistas estão agendadas] 
- Status atual da preparação: [O que você fez até agora] 
- Pontos fortes: [Seus pontos fortes técnicos/de entrevista] 
- Pontos fracos: [Áreas onde você precisa melhorar] 

NECESSIDADES DE PREPARAÇÃO: 
1. Crie um plano de estudo sistemático para as próximas [X semanas] com metas diárias/semanais específicas 
2. Forneça uma lista abrangente de algoritmos e estruturas de dados essenciais com classificação de dificuldade 
3. Gere 3 perguntas de codificação específicas da empresa que reflitam seu estilo de entrevista real 
4. Desenvolver uma estrutura para abordar questões de design de sistema relevantes para meu nível-alvo 
5. Crie cenários de entrevista comportamental com base na minha formação e nas empresas-alvo valores 
6. Sugira estratégias para lidar com pressão e perguntas difíceis 
7. Identifique aspectos culturais específicos da empresa que devo enfatizar em minhas respostas 

Para os problemas de codificação, forneça: 
- Uma declaração detalhada do problema 
- Exemplos esperados de entrada/saída 
- Dicas para abordar a solução 
- Uma solução completa com análise de complexidade de tempo/espaço 
- Armadilhas comuns e casos extremos a serem considerados 
- Perguntas de acompanhamento que um entrevistador pode fazer 

Comece avaliando meu perfil e criando uma 
estratégia de preparação personalizada e, em seguida, prossiga com os componentes específicos de preparação para a entrevista.
```

Você pode ajustar as seções com base em suas necessidades específicas e cronograma.

[Quer crescer mais rápido em tecnologia? Estes 10 prompts do ChatGPT desenvolvem as habilidades que importamNão apenas código — comunicação, liderança, pensamento sistêmico.levelup.gitconnected.com](https://levelup.gitconnected.com/want-to-grow-faster-in-tech-these-10-prompts-build-the-skills-that-matter-556027cc2815?source=post_page-----43af9351c0dc---------------------------------------)

## 3. Refinamento iterativo por meio de feedback direcionado <a href="#id-6325" id="id-6325"></a>

A maioria dos desenvolvedores aceita saídas iniciais sem refinamento.

Implemente este ciclo de feedback:

**Estratégia rápida:**

```
Avaliarei sua solução com base nestes critérios específicos: 
1. Desempenho: Ela escala eficientemente com grandes conjuntos de dados? 
2. Manutenibilidade: O código está bem estruturado e documentado? 
3. Tratamento de erros: Ela lida com casos extremos com elegância? 
4. Segurança: Há alguma vulnerabilidade em potencial? 
5. Cobertura de testes: Esta solução pode ser testada exaustivamente? 

Para quaisquer áreas que não atendam às expectativas, 
fornecerei feedback específico para melhorias. 

Tarefa inicial: [SUA TAREFA DE DESENVOLVIMENTO]
```

Após receber a solução inicial:

```
Obrigado pela solução. Aqui está meu feedback: 
- Desempenho: A abordagem O(n²) não escala bem com conjuntos de dados grandes 
- Tratamento de erros: Validação ausente para [CASO ESPECÍFICO] 
- Manutenibilidade: A função está fazendo muitas coisas. 

Refine sua solução para abordar essas preocupações específicas.
```

## 4. Design de API progressivo <a href="#cab2" id="cab2"></a>

Projete APIs abrangentes por meio de expansão iterativa:

**Estratégia rápida:**

```
Estou projetando uma API RESTful para um sistema [DOMÍNIO ESPECÍFICO]. 
Vamos desenvolver isso progressivamente: 

ETAPA 1: Definição do recurso principal 
- Definir os recursos essenciais e seus relacionamentos 
- Especificar os atributos primários para cada recurso 
- Descrever as operações CRUD básicas 

ETAPA 2: Padrões de interação 
- Definir endpoints especializados além do CRUD 
- Especificar parâmetros de consulta e recursos de filtragem 
- Projetar abordagens de paginação e classificação 

ETAPA 3: Considerações avançadas 
- Padrões de autenticação e autorização 
- Estratégias de limitação de taxa e cota 
- Diretivas de cache e implementação de ETag 
- Abordagem de controle de versão 
- Padronização do tratamento de erros 

ETAPA 4: Documentação e exemplos 
- Gerar especificações OpenAPI - 
Fornecer solicitações/respostas de exemplo para operações comuns 
- Documentar as melhores práticas para consumidores de API 

Vamos começar com a Etapa 1: [DETALHES DO SEU DOMÍNIO ESPECÍFICO DA API]
```

Essa abordagem progressiva ajuda a criar APIs abrangentes e bem pensadas, em vez de rascunhos incompletos.

Cada etapa se baseia na anterior, garantindo que todos os aspectos recebam a devida consideração.

## 5. Otimização do fluxo de trabalho do Git <a href="#id-8656" id="id-8656"></a>

Simplifique operações complexas do Git com linguagem natural:

**Estratégia rápida:**

```
Preciso de ajuda com um cenário complexo de fluxo de trabalho do Git: 

ESTADO ATUAL: 
- Trabalhando no branch de recurso 'feature/payment-processing' 
- O branch principal progrediu com mais de 15 confirmações desde a criação do branch 
- Meu branch tem 7 confirmações, mas as confirmações 2 a 4 devem ser compactadas 
- Há um arquivo conflitante: 'src/services/payment.js' 
- Preciso manter alterações específicas nas linhas 120 a 140 durante a mesclagem 

RESULTADO DESEJADO: 
- Histórico limpo com um único commit lógico para o recurso de pagamento 
- Conflitos resolvidos adequadamente, favorecendo minha implementação do processador de pagamento 
- Mesclado com sucesso no branch principal atualizado 

Forneça os comandos Git passo a passo para fazer isso, explicando cada etapa e sua finalidade.
```

Esse contexto detalhado permite que **o ChatGPT** gere instruções Git precisas para cenários complexos, economizando tempo e evitando erros durante operações desafiadoras de repositório.

[12 prompts SQL do ChatGPT que parecem códigos de trapaçaEscreva junções, CTEs e funções de janela sem pesquisar no Googlelevelup.gitconnected.com](https://levelup.gitconnected.com/12-chatgpt-sql-prompts-that-feel-like-cheat-codes-3c6a29871726?source=post_page-----43af9351c0dc---------------------------------------)

## 6. Estratégia de Depuração Avançada <a href="#ff8e" id="ff8e"></a>

Obtenha ajuda com cenários complexos de depuração:

**Estratégia rápida:**

```
Estou depurando um problema com as seguintes características: 

SINTOMAS: 
- [Descreva o problema observável em detalhes] 
- Ocorre aproximadamente [FREQUÊNCIA] em [CONDIÇÕES ESPECÍFICAS] 
- Começou após [ALTERAÇÃO OU CRONOGRAMA RELEVANTE] 

AMBIENTE: 
- [Tecnologias, versões e plataformas relevantes] 
- [Detalhes de configuração que podem ser relevantes] 

INVESTIGAÇÃO ATÉ O MOMENTO: 
- [Etapas já tomadas] 
- [Evidências coletadas] 
- [Teorias exploradas e descartadas] 

REGISTROS DE ERROS: 
[Inclua os registros relevantes aqui] 

CÓDIGO RELEVANTE: 
[Inclua seções de código suspeitas] 

Ajude-me: 
1. Sugerindo as causas raiz mais prováveis ​​com base nessas informações 
2. Propondo etapas de diagnóstico específicas para confirmar cada hipótese 
3. Recomendando correções direcionadas assim que a causa for identificada
```

Essa abordagem abrangente fornece o contexto necessário para assistência de depuração eficaz, muitas vezes identificando causas negligenciadas de problemas complexos.

### — ABORDAGEM ALTERNATIVA <a href="#id-4379" id="id-4379"></a>

* **O hack de depuração do Rubber Duck**

Quando estiver preso em um bug, você também pode tentar essa abordagem.

**Estratégia:**

```
Estou depurando um problema onde [SINTOMA]. Tentei: 
- [abordagem 1] 
- [abordagem 2] 
- [abordagem 3] 

Suspeito que o problema possa ser [SUA HIPÓTESE]. 

Faça-me 5 perguntas específicas que ajudem a identificar a causa raiz.
```

## 7. Estrutura de decisão arquitetônica <a href="#id-08cc" id="id-08cc"></a>

Ao enfrentar decisões de design complexas, use esta abordagem estruturada:

**Estratégia rápida:**

```
Preciso avaliar abordagens arquitetônicas para [COMPONENTE DE SISTEMA ESPECÍFICO]. 

Para cada uma das seguintes opções, forneça uma análise abrangente: 

OPÇÃO 1: [Abordagem arquitetônica A] 
OPÇÃO 2: [Abordagem arquitetônica B] 
OPÇÃO 3: [Abordagem arquitetônica C] 

Para cada opção, analise: 
1. Características de desempenho sob nossa carga esperada de [MÉTRICAS ESPECÍFICAS] 
2. Complexidade de desenvolvimento e implicações no cronograma 
3. Considerações operacionais (monitoramento, depuração, implantação) 
4. Limitações de escalabilidade e potencial de crescimento 
5. Implicações de segurança 
6. Fatores de custo (desenvolvimento, infraestrutura, manutenção) 

Conclua com uma recomendação com base em nossa prioridade de [PRIORIDADES ESPECÍFICAS].
```

Essa estrutura força uma consideração cuidadosa das compensações em diversas dimensões, levando a decisões arquitetônicas mais bem informadas, apoiadas por uma análise abrangente.

## 8. Estrutura de Geração de Documentação <a href="#id-308f" id="id-308f"></a>

Crie documentação amigável ao desenvolvedor usando este prompt estruturado:

**Estratégia rápida:**

```
Gere documentação abrangente do desenvolvedor para esta [FUNÇÃO/COMPONENTE/MÓDULO]: 

[SEU CÓDIGO] 

Estruture a documentação da seguinte forma: 

1. VISÃO GERAL 
   - Finalidade e funcionalidade principal 
   - Quando usar este componente vs. alternativas 
   - Contexto arquitetônico (onde ele se encaixa no sistema maior) 

2. ESPECIFICAÇÃO TÉCNICA 
   - Referência de API com todos os métodos/propriedades 
   - Parâmetros, valores de retorno e tipos 
   - Gerenciamento de estado (se aplicável) 
   - Eventos emitidos/escutados 

3. EXEMPLOS DE IMPLEMENTAÇÃO 
   - Exemplo de uso básico 
   - Exemplo de configuração avançada 
   - Cenários de personalização 
   - Padrões comuns e melhores práticas 

4. SOLUÇÃO DE PROBLEMAS 
   - Erros comuns e suas soluções 
   - Estratégias de depuração 
   - Considerações de desempenho 

5. COMPONENTES RELACIONADOS 
   - Dependências 
   - Componentes comumente usados ​​junto com este 
   - Abordagens alternativas 

Formate usando markdown com títulos adequados, blocos de código, tabelas e ênfase quando apropriado.
```

Esta estrutura produz documentação que é realmente útil para desenvolvedores, em vez de apenas descrever a superfície da API.

## 9. Gerador de suíte de testes <a href="#id-3f23" id="id-3f23"></a>

Gere uma cobertura de teste completa com esta estratégia de solicitação detalhada:

**Estratégia rápida:**

```
Preciso criar um conjunto de testes abrangente para esta função: 

[CÓDIGO DA FUNÇÃO] 

Por favor, gere testes organizados nestas categorias: 

1. CORREÇÃO FUNCIONAL 
   - Testes de caminho feliz com várias entradas válidas 
   - Tratamento de casos extremos (entradas vazias, valores de limite, etc.) 
   - Condições de erro esperadas e tratamento de erros 

2. CARACTERÍSTICAS DE DESEMPENHO 
   - Testes que verificam o desempenho sob cargas esperadas 
   - Testes para padrões de uso de memória 
   - Verificação da complexidade de tempo para operações de teclas 

3. PONTOS DE INTEGRAÇÃO 
   - Testes para interações com dependências externas 
   - Estratégias de simulação para isolar a função 
   - Teste de efeitos colaterais no sistema 

4. CONSIDERAÇÕES DE SEGURANÇA 
   - Testes de validação e higienização de entrada 
   - Tentativas de bypass de autorização 
   - Vetores de injeção em potencial 

Use a sintaxe [Jest/Mocha/SUA ESTRUTURA DE TESTE] e siga o padrão AAA (Arrange-Act-Assert). 
Inclua a configuração e a desmontagem quando apropriado.
```

Essa abordagem garante uma cobertura de teste abrangente em várias dimensões, em vez de apenas testes básicos de funcionalidade, resultando em um código mais robusto.

### — ABORDAGEM ALTERNATIVA <a href="#id-0e3b" id="id-0e3b"></a>

* **Geração de Teste Unitário**

Pare de escrever testes padronizados

**Estratégia:**

```
Gerar testes unitários abrangentes para esta função: 
[CÓDIGO DE FUNÇÃO] 

Requisitos de teste: 
- Usar [estrutura de teste] 
- Cobrir casos extremos, incluindo [cenários específicos] 
- Alcançar pelo menos 90% de cobertura de código 
- Seguir o padrão AAA (Arrange-Act-Assert)
```

Isso poderia economizar 40% do tempo de desenvolvimento ao automatizar a criação de testes e, ao mesmo tempo, melhorar a qualidade do código.

## 10. Revisão de código multipessoal <a href="#b5e7" id="b5e7"></a>

Simule diversas perspectivas criando uma revisão de equipe virtual:

**Estratégia rápida:**

```
Revise este código de três perspectivas diferentes: 

1. Como especialista em segurança: 
identifique vulnerabilidades potenciais, riscos de injeção ou problemas de autenticação 

. 2. Como engenheiro de desempenho: 
destaque padrões ineficientes, vazamentos de memória ou gargalos. 

3. Como especialista em manutenibilidade: 
aponte nomenclatura pouco clara, lógica complexa ou problemas de arquitetura. 

CÓDIGO: 
[SEU CÓDIGO AQUI] 

Para cada função, forneça feedback específico e sugestões de melhorias.
```

Essa técnica ajuda a identificar pontos cegos ao forçar a consideração de vários pontos de vista especializados, muitas vezes identificando problemas que passariam despercebidos de uma única perspectiva.

## 11. Analisador de Avaliação de Dependências <a href="#id-52d6" id="id-52d6"></a>

Faça escolhas informadas sobre bibliotecas com esta estrutura de análise abrangente:

**Estratégia rápida:**

```
Estou avaliando [BIBLIOTECA/FRAMEWORK] para uso em nosso [CASO DE USO ESPECÍFICO]. 
Forneça uma análise abrangente incluindo: 

AVALIAÇÃO DE FUNCIONALIDADE 
- Completude dos recursos para nossos requisitos: [LISTE OS REQUISITOS PRINCIPAIS] 
- Qualidade do design da API e experiência do desenvolvedor 
- Recursos de personalização e extensão 

AVALIAÇÃO TÉCNICA 
- Benchmarks de desempenho e oportunidades de otimização 
- Tamanho do pacote e impacto no carregamento 
- Compatibilidade do navegador/ambiente 
- Qualidade da definição de tipo/TypeScript 

COMUNIDADE E SUPORTE 
- Atividade de manutenção (confirmações, tempo de resposta a problemas) 
- Tamanho e engajamento da comunidade 
- Qualidade da documentação e exemplos 
- Opções de suporte comercial 

AVALIAÇÃO DE INTEGRAÇÃO 
- Compatibilidade com nossa pilha existente: [liste as tecnologias] 
- Caminho de migração de nossa solução atual 
- Preocupações com bloqueio e estratégias de saída 

COMPARAÇÃO DE ALTERNATIVAS 
- Como se compara a [alternativa A], [alternativa B] em áreas-chave 
- Vantagens e desvantagens exclusivas 

Com base nessa análise, forneça uma recomendação com justificativa específica.
```

Essa abordagem fornece uma estrutura para fazer escolhas tecnológicas com base em análises abrangentes em vez de comparações superficiais.

## 12. Estrutura de Migração de Código <a href="#id-20c0" id="id-20c0"></a>

Automatize migrações de código tediosas com esta estrutura detalhada:

**Estratégia rápida:**

```
Preciso migrar este código [LINGUAGEM/FRAMEWORK DE ORIGEM] para [LINGUAGEM/FRAMEWORK DE DESTINO]: 

CÓDIGO-FONTE: 
[Seu código legado aqui] 

REQUISITOS DE MIGRAÇÃO: 
- Manter lógica e comportamento de negócios idênticos 
- Seguir as práticas recomendadas e expressões idiomáticas de [linguagem/framework de destino] 
- Otimizar para legibilidade e manutenibilidade 
- Aproveitar recursos modernos de [linguagem/framework de destino] 
- Garantir tratamento de erros adequado equivalente ao original 

CONSIDERAÇÕES ESPECÍFICAS: 
- O mecanismo de autenticação precisa ser adaptado para usar [nova abordagem de autenticação] 
- Os tipos de dados devem ser mapeados corretamente (especialmente [tipos de dados específicos]) 
- [Quaisquer outras considerações especiais] 

Forneça-me: 
1. A implementação do código totalmente migrado 
2. Um resumo das principais alterações e decisões de adaptação 
3. Quaisquer notas sobre possíveis problemas ou diferenças de comportamento 
4. Recomendações para testar o código migrado
```

Essa abordagem produz migrações de alta qualidade que respeitam tanto a intenção do código original quanto os idiomas e práticas recomendadas da plataforma de destino.

## 13. Gerenciamento abrangente de janelas de contexto <a href="#id-3e32" id="id-3e32"></a>

Os LLMs modernos têm janelas de contexto impressionantes, mas limitadas. A gestão estratégica de contexto é crucial para tarefas complexas:

**Estratégia rápida:**

```
Vamos estabelecer nosso contexto atual para esta sessão de desenvolvimento: 

CONTEXTO DO PROJETO: 
- Estamos construindo uma API GraphQL para um sistema de agendamento de saúde 
- Pilha de tecnologia: Node.js, TypeScript, Apollo Server, Prisma, PostgreSQL 
- Autenticação usando JWT com controle de acesso baseado em funções 
- Requisitos de conformidade com a HIPAA em vigor 

TAREFA ATUAL: 
- Implementar mutação de agendamento de consultas com detecção de conflitos 
- Deve lidar com conversões de fuso horário corretamente 
- Deve validar de acordo com as regras de disponibilidade do provedor 

Compartilharei trechos de código durante nossa conversa. 
Mantenha-se ciente deste contexto para todas as suas respostas. 

Primeira pergunta: [Sua pergunta específica]
```

Ao estabelecer explicitamente um contexto persistente, você reduz a repetição e cria uma sessão mais coerente.

Essa abordagem é particularmente valiosa ao trabalhar em tarefas de desenvolvimento complexas e multipartes.

## 14. Consultas GraphQL geradas por IA <a href="#f94e" id="f94e"></a>

Escrevendo consultas GraphQL complexas manualmente?

Isso está ultrapassado.

**Estratégia rápida:**

```
Gere uma consulta GraphQL para: 
- Buscar todos os usuários que se inscreveram nos últimos 6 meses 
- Incluir seus últimos 3 pedidos e endereços de entrega 
- Classificar pela inscrição mais recente
```

**Próximo nível:** automatize a documentação da API convertendo esquemas GraphQL em documentos limpos **instantaneamente.**

## 15. Fluxos de trabalho de desenvolvimento com tecnologia de IA <a href="#b0e0" id="b0e0"></a>

Crie fluxos de trabalho de desenvolvimento abrangentes usando assistência de IA:

```
Quero estabelecer um fluxo de trabalho assistido por IA para [TAREFA DE DESENVOLVIMENTO ESPECÍFICA]. 

Por favor, projete um fluxo de trabalho abrangente que: 

1. DEBITE A TAREFA 
   - Identifique componentes distintos e seus relacionamentos 
   - Sugira uma sequência lógica de desenvolvimento 
   - Defina marcos e entregas claros 

2. DEFINA MODELOS DE PROMPT 
   - Para geração de código inicial 
   - Para revisão e melhoria de código 
   - Para geração de documentação 
   - Para desenvolvimento de caso de teste 

3. ESTABELEÇA PORTÕES DE QUALIDADE 
   - Critérios para avaliar o código gerado 
   - Ciclos de feedback para melhoria iterativa 
   - Integração com processos de qualidade existentes 

4. FORNEÇA ESTRUTURAS CONTEXTUAIS 
   - Conhecimento específico do projeto para incluir em prompts 
   - Padrões e convenções para impor 
   - Exemplos de referência para orientar saídas de IA 

O fluxo de trabalho deve ser reutilizável para tarefas semelhantes e otimizado para 
eficiência e qualidade.
```

Essa abordagem de nível meta ajuda a criar processos sistemáticos e repetíveis para desenvolvimento assistido por IA em vez de uso ad hoc, melhorando drasticamente a consistência e a qualidade ao longo do tempo.
