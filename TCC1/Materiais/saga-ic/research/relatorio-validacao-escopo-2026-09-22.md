# Relatório de Validação de Escopo — Saga coreografada vs. orquestrada

**Status:** DOCUMENTO DE TRABALHO — NÃO OFICIAL

**Data:** 22/09/2026

**Projeto:** Comparação entre as abordagens coreografada (choreography) e
orquestrada (orchestration) do padrão Saga em microsserviços.

**Discente:** Rafael Lopes Fraga (FACOM/UFU).
**Orientador(a):** Paulo Rodolfo da Silva Leite Coelho.

**Origem e limites deste documento:**

- Este relatório **sintetiza** o levantamento bibliográfico produzido pelo
  agente _librarian_ e repassado pelo orquestrador. **Não houve nova busca**
  bibliográfica nesta tarefa.
- O documento **não cria fatos novos**: não foram inventados DOI, URLs,
  resultados, números ou dados administrativos.
- Este relatório é um **insumo de decisão** para o orquestrador e o(a)
  orientador(a). Nada aqui é oficial até validação explícita (ver
  `AGENTS.md`, seções 5 e 8).

**Legenda de rótulos (aplicada rigorosamente ao longo do texto):**

- **[E]** — evidência verificada / informação sustentada pelo levantamento
  bibliográfico registrado (fonte identificada no relatório do _librarian_).
  Não implica fichamento concluído nem aprovação do(a) orientador(a).
- **[R]** — recomendação / proposta deste relatório (decisão ainda a tomar).
- **[?]** — pendência de conferência (dado, leitura ou verificação em aberto).
  Nada marcado como [?] deve ser tratado como confirmado.

---

## 0. Reconciliação de estado (atualização de 24/09/2026)

> Esta seção **registra a mudança de estado** desde a emissão do relatório
> (22/09/2026) **sem apagar o histórico original**: os itens que estavam **[?]**
> e foram **superados** por conferência/fichamento posteriores ficam aqui
> apontados. O restante do documento é preservado como registro de época.

- **[E]** **Aydin & Çebi (2022):** o **texto integral foi obtido** (via
  assinatura institucional UFU/IEEE Xplore) e **fichado** em
  `research/fichamentos/fichamento-estudos-comparativos-saga.md` — **deixa de
  ser pendência de acesso**.
- **[E]** **Comparativos fichados:** os quatro comparativos prioritários
  (Rudrabhatla 2018; Kristianto & Zahra 2021; **Aydin & Çebi 2022**;
  **Nadeem & Malik 2022**) e **Marques (2024)** estão **fichados** em
  `research/fichamentos/`; a lacuna passa de hipótese a **lacuna sustentada**.
- **[E]** **Modalidade e formato de citação:** **PIVIC** (Edital DIRPE nº
  7/2026) e **ABNT** estão **definidos** (cf. `AGENTS.md`); **deixam de ser
  pendência**.
- **[E]** **Rudrabhatla (2018):** as razões **1:3,6:8,2** e **1:3,9:6,4** foram
  reconferidas como **razões de tempo de resposta** para carga
  **1×:5×:10×** (p. 21) — **não** são latências absolutas; **"~40×"** é
  **relato aproximado** da seção IV.B (4 e 6 microsserviços, 5 execuções).
- **[E]** **Marques (2024):** metadados/survey/método confirmados; **stack sem
  versões**; **ambiente, carga, latência/throughput/CPU próprios e
  URL/disponibilidade/licença do código não informados**.
- **[E] Wohlin:** referência bibliográfica conferida (seção 8).
- **[R] microservices.io — orientação interna de uso:** conceitos e diagramas de
  **Chris Richardson / microservices.io** podem ser usados no trabalho acadêmico
  com **atribuição** ao autor/fonte, **preferência por paráfrase** e
  **diagramas adaptados**, com **citação/legenda adequada**; **não** é parecer
  jurídico nem licença externa verificada.

---

## 1. Sumário executivo

- **[E]** O tema — **comparar as abordagens coreografada e orquestrada do
  padrão Saga** — é **pertinente** e está ancorado em literatura fundacional
  (Garcia-Molina & Salem, 1987) e em material de referência da indústria
  (Richardson; documentação AWS e Azure).
- **[E]** O tema **não é inédito**: já existem trabalhos que comparam as duas
  abordagens (ver seção 3). Portanto, a contribuição **não pode** ser
  "comparar pela primeira vez".
- **[R]** A contribuição defensável está em **replicar e ampliar** comparações
  existentes com **artefato aberto/reproduzível**, **controle de
  confundidores**, foco em **falhas/consistência**, **caudas de latência
  (p95/p99)** e **operacionalização de manutenção/observabilidade** (seção 4).
- **[R]** Divisão sugerida: **IC** concentra o MVP e as RQ1–RQ2 (desempenho;
  falhas/consistência); **TCC** estende para RQ3–RQ4 (observabilidade;
  manutenção), com possibilidade de desenho híbrido (seção 5).
- **[E]** Os comparativos foram **conferidos e fichados** (ver seções 0, 3 e
  10); a lacuna (seção 4) fica **sustentada** pelos fichamentos. Resta a
  validação do(a) orientador(a).

---

## 2. Conclusão sobre a validade do tema

**[E]** O objeto de estudo é legítimo:

- O padrão Saga tem origem acadêmica formal (transações compensatórias em
  LLTs) e foi adaptado para microsserviços, sendo hoje um padrão de persistência
  e resiliência documentado por provedores de nuvem e frameworks de mercado.
- A distinção **coreografia vs. orquestração** é apresentada na literatura
  técnica como o principal eixo de decisão de projeto para Sagas.

**[E]** O tema, porém, **não é inédito**: há trabalhos que já comparam as duas
abordagens sob diferentes recortes. A novidade deve ser buscada na **forma**
(rigor, reprodutibilidade, recorte de métricas), não na **pergunta** em si.

**[R]** Consequência prática: o projeto deve (i) **assumir explicitamente** a
existência de comparações anteriores, (ii) **posicionar-se em relação a elas**
(replicação/extensão) e (iii) **justificar a lacuna** que pretende cobrir
(seção 4), evitando a alegação de "propor algo inédito".

---

## 3. Trabalhos comparativos prioritários

Os itens abaixo foram apontados pelo relatório bibliográfico do _librarian_
como as comparações mais diretamente relevantes. **[E]** A existência das obras
e os DOIs indicados constam do levantamento recebido. **[?]** No estado original
deste relatório, o **fichamento e a conferência da fonte primária** estavam
pendentes; **após 22/09/2026 os itens foram fichados** (ver seção 0).

| #   | Referência                                                                                                                                              | Identificador                                             | Status de verificação                                                         |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- | ----------------------------------------------------------------------------- |
| 1   | Rudrabhatla (2018) — _Comparison of Event Choreography and Orchestration Techniques in Microservice Architecture_ (IJACSA, v. 9, n. 8, pp. 18–22)       | DOI: `10.14569/IJACSA.2018.090804` **[E]**                | **[E]** Fichado; razões reconferidas como tempo de resposta 1×:5×:10× (p. 21) |
| 2   | Kristianto & Zahra (2021) — _Performance Analysis of Choreography and Orchestration in Microservices Architecture_ (JATIT, v. 99, n. 18, pp. 4220–4230) | PDF via JATIT; **sem DOI identificado** **[E]**           | **[E]** Metadados conferidos (22/09/2026) e **fichado**                       |
| 3   | Aydin & Çebi (2022) — _Comparison of Choreography vs Orchestration Based Saga Patterns in Microservices_ (ICECET 2022, pp. 1–6)                         | DOI: `10.1109/ICECET55527.2022.9872665` **[E]**           | **[E]** Texto integral **obtido (UFU/IEEE) e fichado**                        |
| 4   | Nadeem & Malik (2022) — _A Case for Microservices Orchestration Using Workflow Engines_ (ICSE-NIER '22, pp. 6–10)                                       | DOI: `10.1145/3510455.3512777` **[E]**                    | **[E]** Fichado; artefato **TrainTicket → Temporal** registrado               |
| 5   | Marques (2024) — _Microservices orchestration vs. choreography: a comparison and analysis_ (dissertação de mestrado, ISEP/P.PORTO)                      | Handle `10400.22/27107`; **sem DOI identificado** **[E]** | **[E]** Fichado; stack sem versões; ambiente/carga/código **não informados**  |

Observações:

- **[R]** Priorizar os itens **1, 3 e 4** por tratarem diretamente da
  comparação coreografia vs. orquestração; o item **2** entra como contexto
  adicional de comparativos.
- **[E]** O item **3** (Aydin & Çebi) teve o **texto integral obtido e fichado**
  (ver seção 0) — a pendência de acesso **deixou de existir**.
- **[E]** Para o item **2**, **não há DOI identificado**; metadados conferidos
  (JATIT, v. 99, n. 18, pp. 4220–4230, 2021) — citar pela URL do PDF em acesso
  aberto.
- **[E]** Os itens **1–5** encontram-se **fichados** em
  `research/fichamentos/`; a afirmação original de que "nenhum item teve
  fichamento" fica **superada** (registro preservado aqui por transparência).

### 3.1 2PC como contraste conceitual (não é braço experimental)

- **[R]** O _two-phase commit_ (2PC) será tratado **apenas** como **contraste
  conceitual / baseline teórico** face ao padrão Saga, em Trabalhos Relacionados
  e Fundamentação. O contraste é entre **commit atômico distribuído**
  (tudo-ou-nada, com coordenação e participantes) e **Saga** (sequência de
  transações locais com **compensações**, sem atomicidade/isolamento ACID plenos
  — cf. Garcia-Molina & Salem, 1987).
- **[R]** **O experimento permanece exclusivamente Saga coreografada × Saga
  orquestrada.** O 2PC **não** é um terceiro tratamento, **não** entra no desenho
  do MVP, nas métricas nem nas RQs correntes, e **não** altera o cenário de
  liquidação de ordem descrito na seção 7.
- **[E]** As fontes industriais do acervo motivam o contraste sem o
  transformarem em resultado: Richardson / microservices.io lista _"2PC is not an
  option"_ entre as _forces_ da Saga; a documentação Microsoft Azure trata da
  dificuldade de **ACID entre _stores_ distribuídos** e indica **sagas para
  baixo risco** e **transações distribuídas para alto risco** (paráfrase
  inequívoca — o trecho literal sobre 2PC é ambíguo e **não** deve ser
  reproduzido como recomendação).
- **[E]** Detalhes do 2PC estão consolidados em
  `research/fichamentos/fichamento-2pc-saga.md`, com as referências
  **conferidas**: Gray (1978) — LNCS 60, pp. 393–481, §5.8.3.3.2, DOI
  `10.1007/3-540-08755-9_9`; Skeen (1981) — pp. 133–142, DOI
  `10.1145/582318.582339`; Gray & Lamport (2006) — TODS 31(1), pp. 133–160,
  §3, DOI `10.1145/1132863.1132867`; PostgreSQL **18.6**, seção **67.4**
  (`PREPARE TRANSACTION`/`COMMIT PREPARED`/`ROLLBACK PREPARED`). Para
  **microservices.io**, aplica-se a **orientação interna de uso** (atribuição a
  Chris Richardson/microservices.io, paráfrase e diagramas adaptados).

---

## 4. Lacuna defendível

**[R]** A lacuna a ser defendida **não** é "ninguém comparou". É a seguinte
combinação — que, segundo o levantamento, **não estaria coberta em conjunto**
pelos trabalhos prioritários:

1. **Replicação aberta e reproduzível.** Artefato público (código, cargas,
   scripts de análise) que permita reexecução por terceiros. **[E]** Conferido:
   **Nadeem & Malik (2022)** publicam artefato aberto (**TrainTicket** portado
   para **Temporal**); **Marques (2024)** **não informa** URL/disponibilidade/
   licença do código. Logo, a lacuna de "artefato aberto" é **parcialmente
   ocupada** e deve ser delimitada com cuidado.
2. **Controle de confundidores.** As duas implementações devem diferir
   **apenas** no estilo de coordenação (mesma stack, mesma lógica de negócio,
   mesma topologia, mesmo broker de mensagens). **[E]** Pelos fichamentos,
   **não** se evidencia igualação explícita de **todos** esses fatores entre as
   variantes nos comparativos existentes (cada estudo usa stack/ambiente
   próprios; em Marques 2024, ambiente e carga não são informados).
3. **Falhas e consistência.** Comparação centrada em **compensação, idempotência
   e recuperação** sob injeção de falhas — não apenas em métricas de
   throughput em caminho feliz. **[E]** Rudrabhatla (2018) cobre **falha no
   último serviço com rollback**; Marques (2024) cobre **5 cenários** de
   sucesso/falha/compensação; **nenhum** reporta, no material fichado, uma
   **matriz ampla de injeção de falhas** com verificação de
   consistência/idempotência.
4. **Caudas de latência.** Reportar **p95 e p99** (e não só média/mediana),
   por serem onde diferenças arquiteturais tendem a aparecer.
5. **Manutenção e observabilidade operacionalizadas.** Transformar construtos
   difusos ("acoplamento", "observabilidade", "custo de manutenção") em
   **métricas/proxies explícitos** (ex.: fan-in/fan-out, número de pontos de
   alteração, cobertura de traces, latência de diagnóstico).

**[E]** Os itens 1, 2 e 3 foram **subsidiados pelos fichamentos** em
`research/fichamentos/` (conferência da fonte primária realizada); a lacuna
passa de **hipótese de trabalho** a **lacuna sustentada**, com os ajustes de
escopo indicados nos itens acima.

---

## 5. Divisão proposta: IC vs. TCC

**[R]** A divisão abaixo organiza o esforço em dois produtos encadeados, sem
duplicação de escopo.

### 5.1 IC — MVP experimental (RQ1 + RQ2)

**[R]** Entregáveis:

- Implementação de referência do **MVP** (seção 7) com as duas abordagens.
- Resposta a **RQ1** (desempenho) e **RQ2** (falhas/consistência).
- Artefato reproduzível mínimo + relatório de experimentos.

### 5.2 TCC — extensão (RQ3 + RQ4)

**[R]** Entregáveis:

- **RQ3:** comparar **observabilidade** das duas abordagens (rastreabilidade
  ponta a ponta, facilidade de diagnóstico de falhas).
- **RQ4:** comparar **manutenção/evolução** (esforço para alterar o fluxo,
  acoplamento, pontos de mudança).
- **Híbrido opcional:** incorporar ao TCC os resultados da IC **como capítulo
  experimental**, ampliando o número de cenários/rodadas e aprofundando a
  análise. **[R]** O desenho híbrido é recomendado para aproveitar o mesmo
  artefato e evitar retrabalho.

**[E]** **Não há impedimento identificado** para que a IC seja **aproveitada
como TCC**: IC e TCC **podem compartilhar o mesmo experimento** no período
planejado, com o TCC estendendo/aplicando os mesmos dados e artefato. A
confirmação decorre de **orientação registrada pelo usuário/orientação** —
**não** se invoca norma oficial específica (nenhuma norma foi consultada para
esta afirmação). O vínculo, portanto, **não é bloqueante**.

---

## 6. Perguntas de pesquisa, hipóteses e variáveis

### 6.1 Perguntas de pesquisa

- **[R] RQ1 (desempenho).** Como as abordagens coreografada e orquestrada
  diferem em **latência** (média, p95, p99), **throughput** e **uso de
  recursos** sob diferentes níveis de carga?
- **[R] RQ2 (falhas/consistência).** Como diferem em **recuperação de falhas**
  e **garantia de consistência**, incluindo taxa/tempo de compensação e
  comportamento sob duplicação/perda de mensagens?
- **[R] RQ3 (observabilidade).** Como diferem na **capacidade de observar e
  diagnosticar** o fluxo da saga (cobertura de traces, tempo de localização da
  causa de uma falha)?
- **[R] RQ4 (manutenção).** Como diferem no **esforço e no risco de manutenção**
  ao evoluir o fluxo (acoplamento, número de pontos de alteração)?

### 6.2 Hipóteses (provisórias, a testar)

**[R]** Formulações iniciais; **não** são resultados e podem ser refutadas.

- **H1:** a coreografia tende a apresentar **menor latência mediana** por não
  ter ponto central de coordenação; a orquestração tende a **overhead** em
  caminho feliz.
- **H2:** a orquestração tende a apresentar **menor variabilidade/cauda
  (p99)** em cenários de falha, por centralizar controle e estado.
- **H3:** a orquestração tende a oferecer **melhor observabilidade/diagnóstico**,
  por ter um ponto único de rastreamento do fluxo.
- **H4:** a **localização do acoplamento** difere — coreografia distribui
  dependências entre serviços; orquestração concentra o fluxo no orquestrador
  (trade-off entre acoplamento distribuído e ponto único).

**[E]** Os trabalhos comparativos foram **fichados** (seção 0); H1–H4 devem ser
**refinadas** com base neles, para evitar hipóteses já respondidas pela
literatura.

### 6.3 Variáveis

- **[R] Independentes:** estilo de saga (coreografada vs. orquestrada);
  nível de carga; padrão/severidade de falha injetada; tamanho da saga
  (nº de passos).
- **[R] Dependentes:** latência (média, p95, p99); throughput; taxa de erro;
  taxa e tempo de compensação; tempo de recuperação; consumo de CPU/memória;
  proxies de acoplamento; proxies de observabilidade.
- **[R] Controles (igualar entre as duas abordagens):** linguagem e versão;
  framework (quando aplicável); hardware/host; topologia e limites de rede;
  broker de mensagens; lógica de negócio dos serviços; armazenamento de dados;
  carga, dados e _seed_; procedimento de aquecimento.

---

## 7. Desenho do MVP experimental

**[R]** Os itens abaixo são **recomendações de desenho**. Os números concretos
são **sugestões iniciais**, **não** valores validados nem resultados.

### 7.1 Serviços

- **[R]** **4 serviços de domínio** em um fluxo de **liquidação de ordem**:
  `ordens` (receber ordem) → `conta` (reservar/debitar saldo) → `custódia`
  (reservar/creditar ativo) → `liquidação` (confirmar liquidação). Sugere-se uma
  saga de **4 passos**, com **compensações** para falha na custódia ou na
  liquidação. **[R]** Cenário **proposto, a validar** com o(a) orientador(a).
- **[R]** **Orquestrador técnico** separado: na variante orquestrada, conduz os
  passos e as compensações e **não é participante de domínio** (sem regra de
  negócio própria).
- **[R]** Stack única para as duas abordagens: **Java/Spring Boot** com broker
  de mensagens (ex.: RabbitMQ ou Kafka). **[?]** Confirmar stack e broker com
  o(a) orientador(a); a escolha afeta confundidores.
- **[R]** Duas implementações da **mesma** lógica de negócio:
  (i) **coreografada** (coordenação por eventos entre serviços);
  (ii) **orquestrada** (o **orquestrador técnico** conduz o fluxo).

### 7.2 Cargas

- **[R]** Níveis de carga crescentes por concorrência de usuários virtuais
  (ex.: patamares de 1, 10, 50 e 100) e/ou taxa-alvo (RPS). **[?]** Valores a
  calibrar após teste-piloto.
- **[R]** Incluir pelo menos: _steady-state_, uma rampa de carga e um teste de
  estresse até saturação.

### 7.3 Injeção de falhas

- **[R]** Cenários mínimos: queda de serviço; latência artificial;
  erro/negativa em um passo; duplicação e perda de mensagens; falha parcial
  no meio da saga (para exercitar compensações).

### 7.4 Métricas

- **[R]** Latência (média, p95, p99), throughput, taxa de erro, taxa e tempo
  de compensação, tempo de recuperação, uso de CPU/memória e contagem de
  mensagens. Coleta sugerida com _tracing_ distribuído (ex.: OpenTelemetry) e
  métricas de runtime. **[?]** Ferramental a confirmar.

### 7.5 Protocolo experimental

- **[R]** **Repetições:** sugerido mínimo de 30 execuções por cenário, com
  rodadas de aquecimento descartadas. **[?]** Número a ajustar por análise de
  poder estatístico.
- **[R]** **Ordem aleatorizada** dos cenários para reduzir efeitos de ordem;
  ambiente isolado; nenhum experimento coexecutado.
- **[R]** Análise com estatística apropriada (não apenas médias), reportando
  dispersão e intervalos de confiança.

### 7.6 Artefato reproduzível

- **[R]** Publicar: código dos serviços (duas abordagens), _scripts_ de carga
  e de injeção de falhas, dados/planilhas de resultados e `README` com
  instruções de reexecução. **[R]** A **política de versionamento/licença do
  artefato** é **decisão interna de protocolo** (não pendência bibliográfica).

---

## 8. Ameaças à validade (categorização de Wohlin)

**[E]** Adota-se a categorização clássica de validade em experimentação em
engenharia de software de **Wohlin et al. (2012)**, _Experimentation in
Software Engineering_, **2. ed.**, Springer, DOI
`10.1007/978-3-642-29044-2`, ISBN `978-3-642-29043-5`. A taxonomia adotada tem
**quatro categorias**: **validade de conclusão**, **validade interna**,
**validade de construção** e **validade externa**.

- **Validade de construção (construct):** as métricas representam de fato os
  construtos? "Acoplamento", "observabilidade" e "manutenção" são difusos e
  precisam de proxies explícitos. _Mitigação:_ declarar a definição
  operacional de cada métrica e discutir limitações.
- **Validade interna:** confundidores — implementações não equivalentes,
  diferenças de framework, efeitos de JIT/cache, ruído do ambiente, ordem dos
  experimentos, injeção de falhas imperfeita. _Mitigação:_ igualar stack e
  lógica, aquecimento, aleatorização, isolamento, múltiplas repetições.
- **Validade externa:** generalização a partir de 4 serviços e de uma stack
  específica pode ser limitada. _Mitigação:_ explicitar o escopo e evitar
  generalizações além do cenário estudado.
- **Validade de conclusão:** poder estatístico, número de repetições,
  variabilidade e escolha do teste. _Mitigação:_ definir tamanho amostral,
  reportar dispersão e usar testes adequados.

**[R] Nota sobre confiabilidade (_reliability_):** **não** integra a taxonomia
de Wohlin adotada; quando mencionada, deve ser atribuída **separadamente** a
**Runeson & Höst (2009)**, DOI `10.1007/s10664-008-9102-8` (resultados podem
depender de um artefato específico). **Não** é necessário acrescentar essa
quinta categoria ao experimento controlado.

---

## 9. Fontes prioritárias (além dos comparativos)

**[E]** Fontes já registradas no repositório como base conceitual
(`docs/pesquisa-inicial.md` e `research/sources-manifest.md`):

- Garcia-Molina & Salem (1987), _Sagas_ — artigo fundacional (transações
  compensatórias). **[E]** Resolvido: a versão do PDF presente no repositório é
  a versão a considerar.
- Richardson, _Pattern: Saga_ (Microservices.io) e livro _Microservices
  Patterns_ (2018, cap. 4 — _Managing transactions with sagas_, pp. 110–145).
  **[E]** O capítulo é a **fonte central da fundamentação** (Saga; coreografia ×
  orquestração; compensações; isolamento); **não** é fonte de resultados
  experimentais. **[E]** ISBN impresso `9781617294549` e e-book
  `9781638356325`; páginas-chave registradas. **[R]** Paginação conferida no
  **PDF local** — **reconferir em edição licenciada** se houver citação
  literal.
- Documentação AWS (_Saga patterns_ / _choreography_ / _orchestration_ /
  _coordination options_) e Microsoft Azure (_Saga distributed transactions
  pattern_). **[E]** Licenças de reuso das fontes documentais conferidas e
  aprovadas; para **microservices.io**, vale a **orientação interna de uso**
  (atribuição, paráfrase e diagramas adaptados).
- Frameworks de mercado (Temporal, Camunda, Eventuate Tram Sagas, Axon) como
  contexto de implementação. **[R]** versão-alvo do **Axon 5** a confirmar
  (transição — pendência de protocolo/versão, não bibliográfica).

**[R]** Prioridade de leitura: artigo fundacional → cap. 4 de Richardson
(2018) como **fonte central conceitual** → comparativos 1, 3 e 5 → documentação
de provedores. O capítulo sustenta a **fundamentação**, não os **resultados** —
estes vêm do experimento deste projeto.

---

## 10. Pendências de verificação

Estado **reconciliado** (ver seção 0); os itens originalmente **[?]** foram
reescritos com o status atual, **preservando o registro**:

1. **[E] Superado:** fonte primária conferida e comparativos **fichados**
   (seção 3).
2. **[E] Superado:** **texto integral** de Aydin & Çebi (2022) **obtido e
   fichado**.
3. **[E] Superado:** Marques (2024) — método confirmado (estudo misto +
   _survey_ n = 14); **stack sem versões**; **ambiente, carga,
   latência/throughput/CPU próprios e URL/disponibilidade/licença do código
   não informados**. (**Kristianto & Zahra (2021)**: metadados conferidos;
   **sem DOI identificado** — citar pela URL do PDF.)
4. **[E] Superado:** Nadeem & Malik (2022) **publicam artefato aberto**
   (TrainTicket → Temporal); Marques (2024) **não informa** código — a lacuna
   de "artefato aberto" fica **reposicionada** (seção 4).
5. **[?] Em aberto (protocolo, não bibliográfico):** confirmar **stack,
   broker, ferramental e versão-alvo** do MVP com o(a) orientador(a).
6. **[E] Superado:** dados bibliográficos de **Wohlin (2012)** conferidos
   (seção 8).
7. **[E] Superado:** **modalidade** (**PIVIC**, Edital DIRPE nº 7/2026) e
   **formato de citação** (**ABNT**) definidos (`AGENTS.md`).

**[R] microservices.io — orientação interna de uso:** conceitos e diagramas de
**Chris Richardson / microservices.io** podem ser usados no trabalho acadêmico
com **atribuição**, **preferência por paráfrase** e **diagramas adaptados** e
**citação/legenda adequada**; **não ampliar** a reprodução literal de
trechos/figuras. Trata-se de **orientação interna**, **não** de parecer
jurídico nem de licença externa verificada.

> **[E] Resolvido (fora da lista):** licenças de reuso das fontes de
> documentação (IEEE, AWS, Microsoft, projetos de código aberto) **conferidas e
> aprovadas**; para **microservices.io**, aplica-se a **orientação interna de
> uso** (atribuição, paráfrase e diagramas adaptados). O PDF de **Garcia-Molina
> & Salem (1987)** presente no repositório é a **versão a considerar**.

---

## 11. Próximos passos sugeridos

- **[R]** Validar este relatório com o(a) orientador(a) e fechar o **recorte de
  escopo/entregáveis** IC/TCC (seção 5) — divisão de trabalho e encadeamento
  dos produtos, sem tratar de autorização formal de vínculo (não há
  impedimento identificado).
- **[E]** Comparativos prioritários **fichados** (seção 0); **[R]** revisar a
  lacuna à luz dos fichamentos (seção 4).
- **[R]** Após validação, especificar e construir o MVP (seção 7), com o
  artefato sob versionamento desde o início.
- **[R]** Definir, antes de executar, métricas, número de repetições e testes
  estatísticos (evitar análise _post hoc_).

---

_Documento de trabalho — não oficial. Elaborado a partir do levantamento
bibliográfico do agente librarian, sem nova busca e sem invenção de dados.
Sujeito a revisão pelo orquestrador e pelo(a) orientador(a)._
