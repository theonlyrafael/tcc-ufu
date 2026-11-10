# Fichamento — Fundamentos do padrão Saga

> **DOCUMENTO DE TRABALHO — NÃO OFICIAL**
> Data: 22/09/2026
> Autor do fichamento: agente de pesquisa (a validar por Rafael Fraga / orientador)
> Escopo: fundamentação conceitual do padrão Saga e insumos para a comparação
> coreografia × orquestração.

## Legenda de marcações

- **[E]** — Evidência: afirmação verificada na fonte primária (ou em cópia
  oficial aberta), com referência conferível.
- **[R]** — Recomendação: decisão ou encaminhamento sugerido pelo fichamento,
  sujeito a validação do orientador.
- **[?]** — Pendência: lacuna, acesso não obtido ou ponto que **não** deve ser
  afirmado como fato.

> Regra do repositório: nenhum dado foi inferido ou inventado. Lacunas ficam
> explicitamente marcadas.

---

## 1. Garcia-Molina & Salem (1987) — *Sagas*

### Referência

- **Autores:** Hector Garcia-Molina; Kenneth Salem.
- **Título:** *Sagas*.
- **Veículo (anais):** Proceedings of the 1987 ACM SIGMOD International
  Conference on Management of Data (SIGMOD '87), pp. 249–259.
  DOI `10.1145/38713.38742`.
- **Versão em periódico:** ACM SIGMOD Record, v. 16, DOI `10.1145/38714.38742`.
- **Technical report:** CS-TR-070-87, Princeton University, 20 páginas.
  URL: <https://www.cs.princeton.edu/techreports/1987/070.pdf>
- **Arquivo local:**
  - `research/sources/garcia-molina-1987.pdf` — arquivo principal informado
    pelo usuário (versão de anais, SIGMOD '87, pp. 249–259). **[E]**

### Objetivo

**[E]** Propor o conceito de *saga* como mecanismo para lidar com
transações de longa duração (LLT) em sistemas de banco de dados, nas quais
manter locks por todo o tempo inviabiliza concorrência aceitável.

### Conceitos

- **[E] LLT (long-lived transaction):** transação de longa duração cuja
  execução com locks prolongados degrada disponibilidade e concorrência.
- **[E] Saga:** uma LLT decomposta em uma sequência de subtransações
  intercaláveis (interleavable), que podem ser executadas concorrentemente
  com outras transações.
- **[E] Compensação semântica:** cada subtransação possui uma transação
  compensatória, definida *pela aplicação*, que desfaz semanticamente o
  efeito — não é um rollback físico/automático.
- **[E] Ausência de atomicidade e de isolamento:** a saga não garante
  atomicidade nem isolamento no sentido ACID; efeitos intermediários podem
  ser visíveis a outras transações.
- **[E] SEC / TEC (saga execution coordinator / transaction execution
  coordinator):** componentes responsáveis por coordenar a execução das
  subtransações e disparar as compensações.
- **[E] Recuperação backward/forward:** em caso de falha, a saga pode
  retroceder (compensando subtransações já concluídas) ou avançar
  (reexecutando/continuando), conforme a política adotada.
- **[E] Compensações que podem falhar:** a compensação pode não ser
  executável, deixando a saga em estado parcialmente desfeito — exigindo
  tratamento explícito.

### Evidência

**[E]** O artigo é a fonte fundacional do padrão e sustenta os conceitos
acima. Ele **não** trata de microsserviços e **não** compara coreografia com
orquestração. **[E]** Também **não** fornece métricas de desempenho
(latência, throughput, p95/p99) que possam ser reaproveitadas.

### Implicação para o projeto

- **[E]** Oferece o vocabulário canônico (saga, subtransação, compensação,
  SEC/TEC, backward/forward) que fundamenta a Seção de fundamentação teórica.
- **[R]** Usar como referência fundacional na fundamentação; **não** usar como
  fonte de comparação coreografia × orquestração.
- **[R]** Citar a versão de anais (SIGMOD '87, pp. 249–259, DOI
  `10.1145/38713.38742`) como referência principal; o technical report serve
  como cópia aberta de apoio.

### Limitações / status

- **[E]** Contexto original: banco de dados centralizado; não é trabalho de
  sistemas distribuídos baseados em microsserviços.
- **[E]** Sem experimentos com métricas de desempenho modernas.
- **[E]** Versão correspondente resolvida conforme decisão do projeto: a versão
  do PDF local (`garcia-molina-1987.pdf`) é a versão a considerar para citação.
- **Status:** fonte primária disponível localmente; conceitos verificados.

---

## 2. Megargel, Poskitt & Shankararaman (2021) — *Microservices Orchestration vs. Choreography: A Decision Framework*

> **Nota de rastreabilidade — exclusão do corpus ativo:** esta referência foi
> analisada e **excluída do uso ativo** no projeto, por ser um **framework
> conceitual de decisão** (não um estudo de **Saga/transações com
> compensação**). O fichamento é preservado aqui apenas como **registro
> histórico**, para rastreabilidade; ela **não** deve ser citada como base
> empírica nem figurar nas listas bibliográficas ativas.

### Referência

- **Autores:** Alan Megargel; Christopher M. Poskitt; Venky Shankararaman
  (Singapore Management University).
- **Título:** *Microservices Orchestration vs. Choreography: A Decision
  Framework*.
- **Veículo:** 25th IEEE International Enterprise Distributed Object Computing
  Conference (EDOC 2021), Gold Coast, Australia, 25–29/10/2021, pp. 134–141.
  DOI `10.1109/EDOC52215.2021.00024`.
- **Cópia aberta (OA):**
  <https://cposkitt.github.io/files/publications/microservices_df_edoc21.pdf>

### Objetivo

**[E]** Propor um framework de apoio à decisão para escolher entre
orquestração e coreografia em microsserviços, com base em fatores de
contexto.

### Conceitos — 6 fatores de decisão

1. **[E]** Distribuição WAN / *chattiness* (volume de chamadas de rede).
2. **[E]** Previsibilidade de resposta (determinismo do fluxo).
3. **[E]** Acoplamento ao processo de negócio.
4. **[E]** Reuso e atomicidade.
5. **[E]** Complexidade / número de serviços envolvidos.
6. **[E]** Visibilidade em runtime (observabilidade).

**[E]** Avaliação em escala Likert 1–5. **[E]** Casos citados: Danske Bank
(coreografia), LGB (orquestração), Netflix (híbrido).

### Evidência

**[E]** Trata-se de um **framework não empírico**: a avaliação real dos casos
é apontada como trabalho futuro e os *scores* são subjetivos.

### Implicação para o projeto

- **[R]** Usar os 6 fatores como eixo inicial de comparação e como insumo para
  definir variáveis e instrumentos de avaliação.
- **[R]** Aproveitar os casos (Danske Bank, LGB, Netflix) como exemplos de
  aplicação dos padrões, sempre com atribuição.
- **Não usar** os *scores* como resultado experimental comparável.

### Limitações / status

- **[E]** Não empírico; sem medições de desempenho.
- **[E]** *Scores* subjetivos; avaliação real declarada como futura.
- **Status — registro histórico:** referência **fora do corpus ativo**;
  metadados completos conferidos (título, autores, EDOC 2021, pp. 134–141,
  DOI `10.1109/EDOC52215.2021.00024`).

---

## 3. Nadeem & Malik (2022) — *TrainTicket coreografado → Temporal*

### Referência

- **Autores:** Anas Nadeem; Muhammad Zubair Malik (North Dakota State
  University).
- **Título:** *A Case for Microservices Orchestration Using Workflow Engines*.
- **Veículo:** ICSE-NIER '22 (44th International Conference on Software
  Engineering — New Ideas and Emerging Results), pp. 6–10.
  DOI `10.1145/3510455.3512777`.
- **arXiv:** 2204.07210.
- **Artefato aberto:** TrainTicket (coreografia) portado para **Temporal**
  (orquestração) — <https://arise-ndsu.github.io/trainticketworkflows>

### Objetivo

**[E]** Portar o sistema de referência TrainTicket (arquitetura coreografada)
para execução sobre o motor de workflows Temporal, e avaliar o efeito na
depuração de falhas.

### Conceitos

- **[E]** Migração de coreografia para orquestração via motor de workflow
  (Temporal).
- **[E]** Uso de workflow explícito para tornar o fluxo inspecionável.

### Evidência

- **[E]** 22 bugs considerados.
- **[E]** Medição do **tempo de depuração**, com ganhos em vários casos.
- **[E]** Mas **n = 1** (um sistema) e **sem poder estatístico**.
- **[E]** **Não** é um estudo específico do padrão Saga.
- **[E]** **Não** mede compensação, latência nem throughput.

### Implicação para o projeto

- **[R]** Citar como indício de que a orquestração pode facilitar depuração,
  **sem** extrapolar para desempenho ou para Saga.
- **[R] Correção de escopo importante:** **já existe artefato aberto** nessa
  dimensão (coreografia → orquestração). Portanto, **não alegar ineditismo
  absoluto** quanto a "construir um artefato aberto" ou "portar um sistema".
  A contribuição deve ser delimitada por outra lacuna (ver síntese final).

### Limitações / status

- **[E]** n = 1, sem poder estatístico.
- **[E]** Fora do escopo estrito de Saga; não mede compensação/latência/throughput.
- **Status:** referencia conferida (autores **Anas Nadeem; Muhammad Zubair
  Malik**, pp. 6–10, DOI `10.1145/3510455.3512777`) e artefato aberto
  (TrainTicket → Temporal) registrado.

---

## 4. microservices.io — *Saga pattern* (Chris Richardson)

### Referência

- **URL:** <https://microservices.io/patterns/data/saga.html>
- **Arquivo local:** `research/sources/microservices-io-pattern-saga.html`

### Objetivo

**[E]** Referência canônica (industrial) de definição do padrão Saga no
contexto de microsserviços e das coordenações coreografada/orquestrada.

### Conceitos

- **[E]** Definição canônica de Saga aplicada a microsserviços.
- **[E]** Coordenações coreografada (eventos) e orquestrada (coordenador
  central).
- **[E]** *Drawbacks*: ausência de rollback automático e de isolamento
  (necessidade de compensações e de contramedidas a anomalias).

### Evidência

**[E]** Fonte industrial, sem experimento e sem métricas.

### Implicação para o projeto

- **[R]** Usar para definição de termos e para a descrição qualitativa dos
  *drawbacks*.
- **Não** usar como evidência empírica.

### Limitações / status

- **[E]** Sem experimento; não quantifica.

---

## 5. AWS Prescriptive Guidance — *Saga patterns* (conjunto de páginas oficiais)

### Referência

- **Páginas oficiais (AWS Prescriptive Guidance):**
  - *Saga patterns* —
    <https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/saga-patterns.html>
    (visão geral; variantes **choreography** e **orchestration**).
  - *Saga choreography pattern* —
    <https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/saga-choreography.html>
  - *Saga orchestration pattern* —
    <https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/saga-orchestration.html>
  - *Coordination options* —
    <https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-integrating-microservices/workflow-coordination.html>
    e *Choosing your coordination approach* —
    <https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-integrating-microservices/choosing-approach.html>
- **Arquivo local (página antiga):**
  `research/sources/aws-prescriptive-guidance-saga-pattern.html` refere-se à
  página **antiga** *Saga pattern* (modernization-data-persistence); **não**
  atribuir a ela a comparação entre coordenações.

### Objetivo

**[E]** Documentar o padrão Saga e suas variantes na AWS, incluindo a
comparação explícita entre **coreografia** e **orquestração** e as opções de
coordenação.

### Conceitos

- **[E]** O padrão Saga tem **duas variantes**: **coreografia** e
  **orquestração** (*Saga patterns*).
- **[E]** **Coreografia:** participantes reagem a eventos publicados, sem
  coordenador central; adequada a **poucos participantes** e sem ponto único
  de falha (*saga choreography pattern*).
- **[E]** **Orquestração:** um **orquestrador** central conduz o ciclo de vida
  da transação e dispara compensações; adequada a **muitos participantes**,
  mas o orquestrador pode ser ponto único de falha (*saga orchestration
  pattern*).
- **[E]** *Coordination options*: duas abordagens de workflow —
  **orquestração** e **coreografia**; *Choosing your coordination approach*
  orienta escolher coreografia **dentro** de um microsserviço e orquestração
  **entre** fronteiras de microsserviços, com Saga para rollback distribuído.
- **[E]** Rollback via transações compensatórias e dificuldade de *debugging*
  de fluxos distribuídos.

### Evidência

- **[E]** Fonte industrial; **não** apresenta números/experimentos.
- **[E]** As páginas oficiais **comparam explicitamente** coreografia e
  orquestração (na página antiga local esse recorte **não** aparecia).

### Implicação para o projeto

- **[R]** Fonte industrial de apoio para definição de termos e para a
  descrição qualitativa de coordenação; **não** é evidência empírica.

### Limitações / status

- **[E]** Sem experimento; não quantifica.
- **[R]** A cópia local diz respeito à **página antiga**; as páginas oficiais
  vigentes do conjunto *Saga patterns* são as listadas acima.

---

## 6. Azure Architecture Center — *Saga distributed transactions pattern*

### Referência

- **URL:** <https://learn.microsoft.com/en-us/azure/architecture/patterns/saga>
- **Arquivo local:**
  `research/sources/azure-architecture-center-saga-pattern.html`

### Objetivo

**[E]** Descrever o padrão Saga como solução para transações distribuídas,
com foco em coordenação e tratamento de inconsistências.

### Conceitos

- **[E]** Comparação **explícita** entre coordenações coreografada e
  orquestrada.
- **[E]** Classificação de transações: *compensable*, *pivot* e *retryable*.
- **[E]** Idempotência.
- **[E]** Inconsistência e anomalias, com contramedidas.

### Evidência

**[E]** Fonte industrial; **não** apresenta números/experimentos.

### Implicação para o projeto

- **[R]** Fonte de maior utilidade **entre as industriais** para a comparação
  coreografia × orquestração (por comparar explicitamente as duas).
- **[R]** Usar a taxonomia *compensable/pivot/retryable* e o tratamento de
  idempotência/anomalias na fundamentação.

### Limitações / status

- **[E]** Sem medições; não substitui evidência empírica.

---

## 7. Richardson (2018) — *Microservices Patterns*, cap. 4

### Referência

- **Autor:** Chris Richardson.
- **Título:** *Microservices Patterns: With Examples in Java*; **cap. 4 —
  "Managing transactions with sagas", pp. 110–145**.
- **Editora / ano:** Manning Publications, 2018.
- **ISBN (edição impressa):** **9781617294549** — **[E]** confirmado.
- **ISBN (e-book):** **9781638356325** — **[E]** confirmado.
- **Páginas-chave (cap. 4):** **coreografia** (p. 118), **orquestração**
  (p. 121), **compensação** (pp. 115–116), **isolamento** (pp. 126–129).
- **Proveniência da paginação:** **[R]** a paginação foi conferida no **PDF
  local** (`research/sources/microservices-richardson-2018.pdf`) e **deve ser
  reconferida em edição licenciada** caso haja **citação literal**; o PDF local
  **não** é tratado como cópia licenciada.
- **Prévia oficial:**
  <https://livebook.manning.com/book/microservices-patterns/chapter-4>

### Papel no projeto

**[E]** Obra adotada como **fonte central da fundamentação** sobre o padrão
Saga, a coordenação (coreografia × orquestração) e a consistência em
microsserviços. **[R]** **Não** é — e não deve ser tratada como — **fonte de
resultados experimentais**: cenário, métricas e números são deste projeto.

### Contribuição do capítulo (síntese)

- **[E]** Trata as **transações locais** como unidade de trabalho de cada
  serviço participante, em contraste com transações distribuídas atômicas
  (p. ex. 2PC).
- **[E]** Apresenta as duas formas de coordenação de uma saga: **coreografia**
  (participantes reagem a eventos, sem coordenador central) e **orquestração**
  (um coordenador central conduz os passos por comandos).
- **[E]** Descreve as **transações compensáveis** e as **compensações**: ações,
  definidas pela aplicação, que **desfazem semanticamente** o efeito de passos
  já concluídos.
- **[E]** Discute a **falta de isolamento** das sagas (efeitos intermediários
  podem ser observados) e as **contramedidas** a anomalias — p. ex. *lock*
  semântico, visão pessimista, atualizações comutativas e re-leitura de
  valor —, além de idempotência e retentativa.

### Evidência

- **[E]** Fonte de **fundamentação conceitual/de engenharia**: livro técnico
  **sem** experimento que compare coreografia × orquestração com métricas.
- **[R]** As páginas-chave foram **registradas** (ver Referência); **citação
  literal** exige **reconferência em edição licenciada** antes de reproduzir
  texto — evitar páginas/citações inventadas.
- **[R]** **Não** usar como evidência empírica nem como validação dos
  resultados deste experimento.

### Implicação para o projeto

- **[R]** Base conceitual principal da fundamentação teórica: vocabulário de
  saga, transações locais, estilos de coordenação, compensações, isolamento e
  contramedidas.
- **[R]** Complementa o artigo fundacional (Garcia-Molina & Salem, 1987) e a
  referência industrial `microservices.io`, do mesmo autor.

### Limitações / status

- **[E]** Obra comercial; o acesso **não** é mais tratado como pendência
  bloqueante para uso conceitual.
- **[E]** **Não** fornece os resultados do experimento deste projeto.
- **[E]** **Paginação** do cap. 4 registrada (pp. 110–145, com páginas-chave);
  **[R]** **reconferir em edição licenciada** se houver **citação literal**.

---

## Síntese transversal

- **[E]** Há duas camadas de fontes: **fundacional** (Garcia-Molina & Salem,
  1987) e de **fundamentação conceitual + industrial/qualitativa**
  (Richardson 2018 — **fonte central**; microservices.io; AWS; Azure).
  Nenhuma delas fornece evidência experimental de desempenho para a
  comparação coreografia × orquestração.
- **[E]** Entre as **páginas industriais** locais, somente a **Azure** compara
  explicitamente as duas coordenações; a página AWS local **não** cobre a
  comparação. (A comparação explícita entre coreografia e orquestração
  também está no cap. 4 de Richardson 2018 — fonte central de fundamentação.)
- **[E]** Existe **um** trabalho empírico com artefato aberto (Nadeem & Malik,
  2022), mas com n = 1, foco em depuração e **sem** características de Saga
  ou métricas de compensação/latência/throughput.
- **[E]** O framework de Megargel et al. (2021) organiza a decisão em 6
  fatores, porém sem validação empírica (avaliação futura, *scores*
  subjetivos).
- **[R]** Portanto, a contribuição deste projeto **não** pode ser o mero
  "artefato aberto" nem a "comparação qualitativa" — ambas já existem em
  alguma medida. A lacuna plausível é **empírica e quantitativa**.

## Correções ao escopo (encaminhamentos)

- **[R] Lacuna a perseguir:** comparação **Saga/compensação** com métricas de
  cauda (**p95/p99**) sob **injeção de falhas** e verificação de
  **consistência** (não simplesmente "existe artefato aberto").
- **[R]** **Fichar os trabalhos comparativos** (coreografia × orquestração)
  **antes** de congelar as RQs, para evitar reinvenção e justificar a
  contribuição.
- **[R]** Manter a distinção entre o que é fato verificado **[E]**, proposta
  de encaminhamento **[R]** e pendência de acesso **[?]**.
- **[E]** Pendências **resolvidas** nesta atualização: título/páginas de
  **Megargel et al. (2021)** (registro histórico), autoria de **Nadeem & Malik
  (2022)** (Anas Nadeem; Muhammad Zubair Malik) e paginação do **cap. 4 de
  Richardson (2018)** (pp. 110–145, com páginas-chave) — esta última **com
  ressalva de proveniência** (reconferir em edição licenciada se houver
  citação literal). As páginas **AWS** foram atualizadas para o conjunto
  oficial *Saga patterns* / *choreography* / *orchestration* / *coordination
  options*.
- **[R]** A **política de versionamento/licença do artefato** deste projeto é
  **decisão interna de protocolo**, não pendência bibliográfica. (Versão do PDF
  de Garcia-Molina & Salem **[E]** resolvida conforme decisão do projeto.)
