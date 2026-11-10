# Plano de Trabalho de Pesquisa 2026–2027 — Saga coreografada × orquestrada

> ## ⚠️ DOCUMENTO DE TRABALHO — NÃO OFICIAL
>
> - **Período planejado:** 01/09/2026 a 30/06/2027.
> - **Edital-base:** DIRPE nº 7/2026; a **errata** (`ic/errada-edital.webarchive`)
>   pertence à mesma Chamada PIVIC UFU 2026-2027 e **retifica o prazo de
>   submissão para 31/12/2026** (não é um edital distinto).
> - Este é um **plano orientador de organização e execução**, dirigido ao discente
>   **Rafael Lopes Fraga**, com base no que já está registrado no repositório.
> - **Não substitui a proposta oficial.** O formulário institucional (ANEXO I /
>   Formulário PIVIC I) e os produtos oficiais continuam sendo os documentos
>   entregues à Diretoria de Pesquisa.
> - Este plano **precisa ser validado pelo(a) orientador(a)** antes de qualquer
>   decisão ser tratada como definitiva.
> - **Recomendações acatadas e incorporadas:** as **recomendações deste plano
>   foram acatadas** pelo(a) orientador(a)/discente e as decisões
>   correspondentes já estão **incorporadas ao texto** (por exemplo, instâncias
>   independentes de banco por serviço — §3.2). Permanecem marcados como
>   **`[A CONFIRMAR]`** não apenas os itens em aberto no próprio **protocolo** e
>   no **piloto** — como o **valor de `N`** dos retries e os
>   **limiares/janelas** numéricos (§3.10) — mas também **pendências
>   deliberadas de protocolo/piloto** (a fechar com o(a) orientador(a)) e
>   **decisões administrativas/documentais ainda abertas**: a **forma/licença de
>   publicação do artefato** (§8), a **política de versionamento** (§5.1 e §8),
>   o **nome da pasta do experimento** (§5.1) e a **agenda de orientação**
>   (dia/horário, §6.1). Nenhuma dessas decisões foi antecipada ou inventada
>   neste plano.
> - **Já decididos:** modalidade (PIVIC), edital/período, grupo CNPq
>   (Dependable Distributed Computing), unidade/campus (FACOM — Campus Santa
>   Mônica), área/linha (Sistemas Distribuídos e Engenharia de Software),
>   formato de citação (ABNT) e **stack experimental** (Java/Spring Boot,
>   PostgreSQL, RabbitMQ, Docker Compose, k6, Prometheus/OpenTelemetry).
> - **Pendências de pesquisa** (não administrativas) seguem marcadas como
>   **`[A CONFIRMAR]`/`[?]`** quando ainda dependem de conferência de fonte ou
>   de execução futura. **Nenhum resultado foi inventado.**
> - Este documento **não edita** os arquivos existentes nem a pasta `ic/`
>   (conteúdo de terceiros e modelos institucionais são somente leitura).

---

## Como ler os rótulos

Ao longo do plano, cada afirmação é classificada segundo o padrão já usado nos
documentos do repositório (`AGENTS.md`, seções 5 e 8):

- **[E] — Evidência verificada:** informação sustentada pelas fontes já
  registradas em `research/` (fichamentos e manifesto), com origem identificável.
  Não implica aprovação do(a) orientador(a) nem conclusão de fichamento.
- **[R] — Decisão recomendada:** proposta deste plano, ainda sujeita a decisão
  do(a) orientador(a) e do discente.
- **[A CONFIRMAR] — Pendência:** dado, data, ferramenta ou decisão que **não
  pode ser tratada como confirmada** e exige verificação.

Escala temporal: todos os meses se referem a **2026** (set–dez) e **2027**
(jan–jun).

---

## 1. Objetivo geral e delimitação

### 1.1 Objetivo geral

**[R]** **Implementar duas versões funcionalmente equivalentes do padrão Saga**
— uma **coreografada** (coordenação por eventos entre serviços) e uma
**orquestrada** (coordenação por um serviço central) — sobre **o mesmo caso de
referência, a mesma lógica de negócio e a mesma infraestrutura**, e
**compará-las** em cenários delimitados quanto a:

1. **desempenho** (eixo complementar);
2. **compensação e consistência sob falhas** (eixo principal);
3. **observabilidade e manutenção** (extensões possíveis, condicionadas à
   viabilidade do MVP).

### 1.2 Delimitação (o que o projeto faz e o que não faz)

| Dimensão                | Delimitação                                                                                                                                                                                                                                                                                                                                                                                                              |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Escopo**              | Um **único caso de referência** de saga de **liquidação de ordem**, com **4 serviços de domínio** (`ordens`, `conta`, `custódia`, `liquidação`) e um **orquestrador técnico** separado (não participante de domínio); **4 passos** ([R]; ver §3.1).                                                                                                                                                                      |
| **Infraestrutura**      | **Uma única** stack, **um único** broker de mensagens, **um único** modelo de banco e **uma única** topologia para as duas abordagens (decidida: Java/Spring Boot + PostgreSQL + RabbitMQ + Docker Compose; ver §3.2).                                                                                                                                                                                                   |
| **Comparação**          | As duas implementações devem diferir **apenas** no estilo de coordenação; todo o resto é igualado (controles, §3.4).                                                                                                                                                                                                                                                                                                     |
| **Generalização**       | Resultados valem **para o cenário estudado**, não para “microsserviços em geral”. Nada de generalização além do contexto.                                                                                                                                                                                                                                                                                                |
| **O que NÃO se afirma** | **Não** se afirma que uma abordagem seja **universalmente melhor**. A conclusão esperada é **condicional ao cenário** (carga, falha, métrica).                                                                                                                                                                                                                                                                           |
| **O que NÃO se faz**    | **Não** se cria experimento antes de o **protocolo** estar fechado (§3; §8). **Não** se busca “provar” de antemão a hipótese.                                                                                                                                                                                                                                                                                            |
| **Novidade**            | **[E]** A comparação coreografia × orquestração **não é inédita** (Rudrabhatla, 2018; Kristianto & Zahra, 2021; Marques, 2024; Aydin & Çebi, 2022) e **tampouco o é “artefato aberto”** (Anas Nadeem & Malik, 2022). A contribuição deve ser a **lacuna empírica/quantitativa**: **caudas (p95/p99)**, **matriz de falhas/consistência/compensação** e **controle de confundidores** — conforme `research/fichamentos/`. |

> **[E]** Justificativa da delimitação: os comparativos já cobrem **latência
> média, throughput, CPU e esforço de mudança de fluxo**; os fichamentos apontam
> que **nenhum texto integral obtido** reporta **p95/p99** para sagas com
> compensação, e que **falhas/consistência** são tratadas de forma pontual.

---

## 2. Perguntas de pesquisa, hipóteses e expectativas

### 2.1 Perguntas de pesquisa

| ID      | Eixo                      | Formulação                                                                                                                                                                                                 | Papel                  |
| ------- | ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- |
| **RQ2** | **Falhas e consistência** | Como as abordagens diferem em **recuperação de falhas** e **garantia de consistência**, incluindo **taxa e tempo de compensação**, **idempotência** e comportamento sob **duplicação/perda de mensagens**? | **Eixo principal**     |
| **RQ1** | **Desempenho**            | Como diferem em **latência (p50, p95, p99)**, **throughput** e **uso de recursos** sob diferentes níveis de carga?                                                                                         | Eixo complementar      |
| **RQ3** | **Observabilidade**       | Como diferem na **capacidade de observar e diagnosticar** o fluxo da saga (cobertura de traces, tempo de localização da causa de uma falha)?                                                               | Extensão (condicional) |
| **RQ4** | **Manutenção**            | Como diferem no **esforço/risco de manutenção** ao evoluir o fluxo (acoplamento, número de pontos de alteração)?                                                                                           | Extensão (condicional) |

**[R]** RQ1 e RQ2 formam o **MVP** do projeto. RQ3 e RQ4 entram como
**extensões opcionais**, **sem** tornar o MVP inviável; podem ser aproveitadas
tanto na IC quanto no TCC, que **podem compartilhar o mesmo experimento** no
período planejado. Se o tempo apertar, RQ3/RQ4 são **cortadas antes** de
comprometer RQ2/RQ1 (critérios de corte em §9.2).

### 2.2 Hipóteses / expectativas (provisórias — podem ser refutadas)

| ID     | Expectativa inicial                                                                                                                                             | Observação                                                                                                                   |
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **H1** | A **coreografia** tende a ter **menor latência mediana** no caminho feliz (sem coordenador central); a **orquestração** tende a ter **overhead** nesse caminho. | **[E]** Convergente com Rudrabhatla (2018) e Kristianto & Zahra (2021); **não** é novidade — servir de checagem de sanidade. |
| **H2** | A **orquestração** tende a apresentar **menor variabilidade/cauda (p99)** em cenários de falha (controle e estado centralizados).                               | **Lacuna principal**; os comparativos não reportam caudas.                                                                   |
| **H3** | A **orquestração** tende a oferecer **melhor observabilidade/diagnóstico**.                                                                                     | Insumo indireto de Anas Nadeem & Malik (2022, n = 1); **não** extrapolar.                                                    |
| **H4** | A **localização do acoplamento** difere: coreografia distribui dependências; orquestração as concentra no coordenador (trade-off, possível SPOF).               | **[E]** Convergente com Marques (2024); insumo para RQ4.                                                                     |

> **[R]** H1–H4 são **expectativas**, não resultados. Elas devem ser
> **revisadas após o fichamento dos comparativos prioritários** (§7), para evitar
> hipóteses já respondidas pela literatura. **[E]** Achado de Kristianto &
> Zahra (2021) que sustenta H4: esforço de mudança de fluxo de **6 APIs em 4
> serviços** (coreografia) vs. **1 API em 1 coordenador** (orquestração).

### 2.3 Natureza da conclusão esperada

**[R]** O resultado do projeto é uma **afirmação condicional**: _“no cenário
X, sob a carga Y e a falha Z, a abordagem A apresentou [métrica] com [IC];
no cenário X′, o resultado se inverte / não há evidência de diferença
relevante”_. Evitar qualquer conclusão do tipo “A é melhor que B” em termos
absolutos.

---

## 3. Estratégia experimental detalhada

> **Regra de ouro:** nenhuma coleta de dados experimental começa **antes** de o
> **protocolo** (§3.9) estar escrito, revisado e aprovado pelo(a) orientador(a).
> O mês de **outubro/2026** é reservado para isso.

### 3.1 Caso de referência

> **[R]** Cenário **proposto, a validar** com o(a) orientador(a). Os nomes dos
> serviços e o fluxo são **decisões de desenho**, **não** resultados.

- **[R] Fluxo-base único (liquidação de ordem):** quatro **serviços de domínio**
  — `ordens`, `conta`, `custódia` e `liquidação` — em **4 passos** com
  compensações: **receber ordem** (`ordens`) → **reservar/debitar saldo**
  (`conta`) → **reservar/creditar ativo** (`custódia`) → **confirmar liquidação**
  (`liquidação`).
- **[R] Compensações:** falha na **custódia** desfaz a reserva/débito de saldo
  (`conta`) e a ordem; falha na **liquidação** desfaz a reserva/crédito de ativo
  (`custódia`) e a reserva/débito de saldo (`conta`).
- **[R] Orquestrador técnico separado:** na variante orquestrada, um
  **orquestrador técnico** conduz os passos e as compensações e **não é
  participante de domínio** (não tem regra de negócio própria nem estado de
  domínio); os quatro serviços de domínio são os **mesmos** nas duas variantes.
- **[R] Duas implementações da MESMA lógica de negócio:**
  - (i) **coreografada** — coordenação por eventos publicados/consumidos nos serviços;
  - (ii) **orquestrada** — o **orquestrador técnico** conduz os passos e as compensações.
- **[R]** Nenhuma das duas versões “ganha” capacidade extra: mesmas regras de
  negócio, mesmos endpoints, mesmas mensagens de entrada.

#### 3.1.1 Fluxo feliz e papel do orquestrador técnico

**[R] Caminho feliz (sem falhas), quatro serviços de domínio:**

1. `ordens` — recebe a **ordem** do cliente e a registra (ponto de entrada do
   fluxo); publica/expõe o **fato inicial** de que a ordem foi recebida.
2. `conta` — **reserva/debita saldo** para a ordem.
3. `custódia` — **reserva/credita ativo** correspondente.
4. `liquidação` — **confirma a liquidação** da ordem, encerrando o fluxo.

**[R] Papel do orquestrador técnico:** na variante **orquestrada**, o
orquestrador conduz a sequência acima, **comandando** cada passo na ordem
correta, **aguardando a resposta** de cada serviço e decidindo o próximo passo
ou a compensação. Ele **não é participante de domínio**: não possui regra de
negócio nem estado de domínio próprio — apenas **estado de coordenação/da
Saga** (§3.2). Os quatro serviços de domínio são **os mesmos** nas duas
variantes.

**[R] Comandos/respostas (orquestração) × eventos/fatos (coreografia):**

- **Orquestração:** o orquestrador envia **comandos** a cada serviço e recebe
  **respostas** (sucesso/falha); a comunicação é **imperativa e ponto a ponto**,
  e o orquestrador mantém o **estado da Saga** e decide as compensações.
- **Coreografia:** cada serviço publica **eventos/fatos** de domínio e os
  demais **reagem** a eles, **sem** coordenador central; a coordenação é
  **emergente** a partir dos eventos. Exemplos de fatos:
  `OrdemRecebida` → `SaldoReservado` → `AtivoReservado` →
  `LiquidacaoConcluida`.

**[R] Compensações (desfazimento em ordem inversa):**

- **Falha em `conta`:** a ordem **não avança**; desfaz-se qualquer efeito
  parcial em `conta` e a ordem é **rejeitada**. Como `custódia` e `liquidação`
  não chegam a ser acionados, **não há compensação a jusante**.
- **Falha em `custódia`:** desfaz-se a **reserva/débito de saldo** em `conta` e
  a ordem é encerrada (rejeição).
- **Falha em `liquidação`:** desfaz-se a **reserva/crédito de ativo** em
  `custódia` e, em seguida, a **reserva/débito de saldo** em `conta`.
- **Falha em `ordens` (passo inicial):** **não** se define regra de negócio de
  compensação para este passo; uma falha já na **etapa inicial** pode resultar
  em **rejeição sem compensação**, pois nada a jusante foi efetivado.

> **[A CONFIRMAR]** os nomes exatos dos eventos/comandos, contratos e regras de
> rejeição devem ser fechados no protocolo (§3.10) e refletidos nas duas
> implementações, preservando a equivalência funcional (§3.8).

#### 3.1.2 Diagramas conceituais (Mermaid)

> **[R]** Diagramas **conceituais** de coordenação. **Não** afirmam _endpoints_,
> _paths_, protocolos nem tecnologias não decididos; servem para alinhar o
> desenho do fluxo, **não** para especificar a implementação.

<!-- Diagramas em células `{mermaid}` (Quarto) com largura fixada em polegadas
     (`fig-width`) para caberem na coluna de texto do PDF; a configuração
     `%%{init}%%` do primeiro diagrama só reduz espaçamentos/colunas, sem
     alterar rótulos ou conteúdo. Renderização: `docs/render-pdf.sh`. -->

**Saga orquestrada — caminho feliz (comandos e respostas):**

```{mermaid}
%%| fig-width: 6.1
%%{init: {"sequence": {"actorMargin": 20, "width": 90, "boxMargin": 6}}}%%
sequenceDiagram
    participant C as Cliente
    participant O as Orquestrador técnico
    participant OR as ordens
    participant CO as conta
    participant CU as custódia
    participant LI as liquidação

    C->>O: solicita ordem
    O->>OR: comando: registrar ordem
    OR-->>O: resposta: ordem registrada
    O->>CO: comando: reservar/debitar saldo
    CO-->>O: resposta: saldo reservado
    O->>CU: comando: reservar/creditar ativo
    CU-->>O: resposta: ativo reservado
    O->>LI: comando: confirmar liquidação
    LI-->>O: resposta: liquidação concluída
    O-->>C: ordem liquidada
```

**Saga coreografada — fluxo por eventos/fatos:**

```{mermaid}
%%| fig-width: 2.0
flowchart TD
    C[Cliente] -->|solicita ordem| OR[ordens]
    OR -->|OrdemRecebida| CO[conta]
    CO -->|SaldoReservado| CU[custódia]
    CU -->|AtivoReservado| LI[liquidação]
    LI -->|LiquidacaoConcluida| FIM[Ordem liquidada]
```

**Falha e compensação — desfazimento em ordem inversa:**

```{mermaid}
%%| fig-width: 5.5
flowchart TD
    subgraph FC [Falha na custódia]
        direction TB
        A1[ordens: ordem recebida] --> A2[conta: saldo reservado]
        A2 --> A3[custódia: falha ao reservar/creditar ativo]
        A3 -->|compensação| A4[conta: desfaz reserva/débito de saldo]
        A4 --> A5[ordens: ordem rejeitada]
    end

    subgraph FL [Falha na liquidação]
        direction TB
        B1[ordens: ordem recebida] --> B2[conta: saldo reservado]
        B2 --> B3[custódia: ativo reservado]
        B3 --> B4[liquidação: falha ao confirmar]
        B4 -->|compensação em ordem inversa| B5[custódia: desfaz reserva/crédito de ativo]
        B5 --> B6[conta: desfaz reserva/débito de saldo]
        B6 --> B7[ordens: ordem rejeitada]
    end
```

### 3.2 Stack, broker, banco e infraestrutura (decididos para o protocolo)

| Item                | Decisão                                | Observação                                                                                                              |
| ------------------- | -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Linguagem/framework | **Java / Spring Boot**                 | Coerente com os comparativos (Rudrabhatla, 2018; Kristianto & Zahra, 2021) **[E]**.                                     |
| Broker de mensagens | **RabbitMQ**                           | **Único** broker para as duas abordagens.                                                                               |
| Banco de dados      | **PostgreSQL (instância independente por serviço)** | Mesma **tecnologia por serviço** nas duas abordagens; **não** é um banco compartilhado; **uma instância por serviço** (ver nota abaixo). |
| Orquestrador        | **Orquestrador próprio de referência** | Evita igualar stack a um motor externo e reduz confundidor; motor de workflow (ex.: Camunda/Temporal) fica fora do MVP. |
| Infra de execução   | **Docker Compose** (ambiente isolado)  | Uma máquina/VM ou cluster, sem cargas coexecutadas; registrar CPUs, memória e versões.                                  |
| Tracing/métricas    | **Prometheus + OpenTelemetry**         | Métricas de runtime e tracing distribuído.                                                                              |
| Carga               | **k6**                                 | Scripts de carga automatizados e reproduzíveis.                                                                         |
| Lógica de domínio   | **Módulo compartilhado**               | Consumido pelos dois modos de coordenação (§3.3).                                                                       |

> **[R] Persistência logicamente isolada (correção de ambiguidade):** cada
> **serviço de domínio** (`ordens`, `conta`, `custódia`, `liquidação`) possui
> **armazenamento logicamente isolado**; o **orquestrador** possui
> **armazenamento próprio apenas para o estado da Saga/coordenação**. **Nenhum**
> serviço acessa tabelas de outro (**sem acesso cruzado**). **PostgreSQL** é a
> **mesma tecnologia** nas **duas** implementações — **não** um banco único
> compartilhado. O **RabbitMQ** permanece **infraestrutura comum** (mesmo broker
> nas duas abordagens).
>
> **[R] Empacotamento físico (decisão incorporada):** adotar
> **instâncias independentes de banco por serviço** — cada `ordens`, `conta`,
> `custódia`, `liquidação` e o **orquestrador** (estado da Saga) com **sua
> própria instância PostgreSQL**, pois as entidades rodarão em **máquinas
> diferentes** nos **testes reais**. Fica assim **resolvida** a antiga ambiguidade
> entre instância por serviço e _schemas_ separados em um mesmo servidor: **não**
> se usa um servidor/banco compartilhado. O **isolamento lógico** e a **ausência
> de acesso cruzado** permanecem fixados.

### 3.3 Variáveis

**Independentes [R]:**

- **estilo de saga** (coreografada × orquestrada) — variável principal;
- **nível de carga** (patamares crescentes);
- **cenário/protocolo de falha** injetada;
- **tamanho da saga** (nº de passos) — opcional, se o tempo permitir.

**Dependentes [R]:**

- latência **p50, p95, p99** e dispersão;
- throughput (RPS concluídas);
- **taxa de erro**;
- **taxa de compensação** e **tempo de compensação**;
- **tempo de recuperação** e **consistência final** (reconciliação de estado);
- consumo de **CPU/memória**;
- contagem/duplicidade/perda de mensagens;
- proxies explícitos de **observabilidade** (ex.: cobertura de traces) e de
  **acoplamento/manutenção** (ex.: nº de pontos de alteração) — **somente** se
  RQ3/RQ4 entrarem.

**Controles — igualar entre as duas abordagens (crítico) [R]:**

| Controle              | Como igualar                                                                                                                                  |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Linguagem e versão    | mesmas versões fixadas                                                                                                                        |
| Framework             | mesma stack (quando aplicável)                                                                                                                |
| Hardware/host         | mesmo ambiente, mesma alocação                                                                                                                |
| Topologia e rede      | mesmos limites e localização                                                                                                                  |
| Broker de mensagens   | mesmo broker e configuração                                                                                                                   |
| Lógica de negócio     | mesmo código compartilhado (biblioteca comum)                                                                                                 |
| Armazenamento         | mesmo **padrão** de isolamento lógico por serviço; mesma tecnologia (PostgreSQL) e topologia nas duas abordagens; **sem** banco compartilhado |
| Carga, dados e seed   | mesmos geradores e mesma seed                                                                                                                 |
| Aquecimento (warm-up) | mesmo procedimento antes das medições                                                                                                         |

> **[R] Estratégia de equivalência:** extrair a **lógica compartilhada** para um
> módulo comum consumido pelos dois modos de coordenação. Isso reduz o risco de
> “implementações não equivalentes” (§9.1) e precisa ser **demonstrado** no
> relatório (mesmos contratos, mesmos testes funcionais).

### 3.4 Cenários de carga [R]

| Cenário          | Descrição                                      | Valores iniciais (a calibrar no piloto)                                                 |
| ---------------- | ---------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Steady-state** | carga constante por janela estável             | **[R]** patamares de concorrência (ex.: 1, 10, 50, 100 usuários virtuais) e/ou RPS-alvo |
| **Rampa**        | aumento gradual até saturação                  | **[A CONFIRMAR]** inclinação/limites                                                    |
| **Estresse**     | carga acima do joelho da curva, até degradação | **[A CONFIRMAR]**                                                                       |

> **[E]** Kristianto & Zahra (2021) usaram de **1 a 1000 usuários** e **1–2
> instâncias**; Rudrabhatla (2018) reportou as **razões de tempo de resposta sob
> carga 1×:5×:10×** (sem **req/s**, **hardware** nem **versões**). Os patamares
> concretos deste projeto devem ser **calibrados pelo piloto** e **não copiados**
> de outros estudos.

**Escalonamento horizontal — cenário secundário [R]:**

> Múltiplas **instâncias** por serviço são uma **extensão/cenário secundário**,
> **não** uma variável principal do desenho.

- **Baseline:** **uma instância** por serviço (incluindo o orquestrador).
- **Extensão:** **duas instâncias** por **serviço de domínio**, com a **mesma
  quantidade nas duas abordagens**; o **orquestrador** permanece inicialmente
  com **uma instância**.
- Os patamares de carga da extensão são **`[A CONFIRMAR]`** (calibrados no
  piloto); **não** se fixam números de throughput.

### 3.5 Injeção de falhas [R]

Seis tipos de falha (**cenários mínimos**), aplicados de forma **igual** às duas
abordagens (mesma matriz de falhas — §3.8):

| #   | Tipo de falha                           | Efeito pretendido                 | Mecanismo concreto de injeção | Duração             | Reversão            | Parâmetros          |
| --- | --------------------------------------- | --------------------------------- | ----------------------------- | ------------------- | ------------------- | ------------------- |
| 1   | **Queda de serviço**                    | um participante fica indisponível | **`[A CONFIRMAR]`**           | **`[A CONFIRMAR]`** | **`[A CONFIRMAR]`** | **`[A CONFIRMAR]`** |
| 2   | **Latência artificial**                 | atraso injetado em um passo       | **`[A CONFIRMAR]`**           | **`[A CONFIRMAR]`** | **`[A CONFIRMAR]`** | **`[A CONFIRMAR]`** |
| 3   | **Erro/negativa de negócio**            | falha semântica → compensação     | **`[A CONFIRMAR]`**           | **`[A CONFIRMAR]`** | **`[A CONFIRMAR]`** | **`[A CONFIRMAR]`** |
| 4   | **Duplicação de mensagens**             | teste de idempotência             | **`[A CONFIRMAR]`**           | **`[A CONFIRMAR]`** | **`[A CONFIRMAR]`** | **`[A CONFIRMAR]`** |
| 5   | **Perda/indisponibilidade de mensagem** | teste de recuperação/reenvio      | **`[A CONFIRMAR]`**           | **`[A CONFIRMAR]`** | **`[A CONFIRMAR]`** | **`[A CONFIRMAR]`** |
| 6   | **Falha parcial no meio da Saga**       | exercita compensações em cascata  | **`[A CONFIRMAR]`**           | **`[A CONFIRMAR]`** | **`[A CONFIRMAR]`** | **`[A CONFIRMAR]`** |

**[R]** Cada cenário deve declarar: **onde** a falha é injetada, **quando**
(passo/instante), **duração**, **reversão** e **critério de sucesso** (estado
final consistente ou não).

**[R] Momento da definição (decisão incorporada):** a
**injeção de falhas** será **definida após o amadurecimento do caminho feliz**
— isto é, depois de as duas implementações operarem sem falhas e de os
contratos, eventos/comandos e compensações estarem estáveis. Até então, a
**tabela/matriz de falhas** acima é **mantida** como referência de escopo; o
**mecanismo concreto** de cada tipo permanece **`[A CONFIRMAR]`** no protocolo
(§3.10). **Nenhuma** ferramenta de injeção é fixada aqui.

**[R] Retries (decisão incorporada):** adotar **retries
limitados** com **número máximo `N` fixo** ainda **`[A CONFIRMAR]`** (**a
definir no protocolo**) e **backoff exponencial**. Retries, reenvios e ações de
**compensação** devem ser **idempotentes** (tratamento explícito de mensagens
duplicadas).

### 3.6 Métricas — primárias e secundárias

| Classe         | Métrica                                                    | Papel                  |
| -------------- | ---------------------------------------------------------- | ---------------------- |
| **Primária**   | **taxa de compensação correta** e **tempo de compensação** | RQ2 (eixo principal)   |
| **Primária**   | **consistência final** (estado reconciliado após falha)    | RQ2                    |
| **Primária**   | **latência p95/p99**                                       | RQ1 (cauda — lacuna)   |
| **Secundária** | latência p50, throughput, taxa de erro                     | RQ1                    |
| **Secundária** | CPU/memória, contagem/duplicidade de mensagens             | contexto/confundidores |
| **Secundária** | proxies de observabilidade e de manutenção                 | RQ3/RQ4 (condicional)  |

**[R]** Definir, **por escrito**, a **definição operacional** de cada métrica
antes da coleta (o que conta como “compensação correta”, “consistente”, etc.).

**Definições operacionais das métricas [R]** (decisão incorporada; os
**limiares** e **janelas** permanecem
**`[A CONFIRMAR]`** até o protocolo — **nenhum valor numérico** é fixado aqui):

- **Sucesso:** ordem/Saga **concluída** com **todos** os passos efetivados e
  estado final igual ao esperado, **sem** compensação.
- **Rejeição:** ordem encerrada **sem** efeitos de domínio remanescentes
  (nenhum saldo/ativo reservado); pode decorrer de falha inicial (ex.: em
  `ordens`) ou de negativa de negócio.
- **Saga compensada:** Saga que sofreu falha **após** ao menos um passo
  efetivado e cujas **compensações** foram aplicadas, retornando os serviços
  envolvidos ao estado anterior à Saga.
- **Consistência final:** após a conclusão (inclusive compensações), os estados
  dos serviços de domínio são **mutuamente compatíveis** segundo os
  **invariantes** definidos no protocolo (**`[A CONFIRMAR]`** os invariantes e a
  janela de convergência).
- **Tempo até consistência:** intervalo entre a **falha/início da compensação**
  e a **observação do estado consistente**; janela de convergência
  **`[A CONFIRMAR]`**.
- **Tempo de recuperação:** intervalo entre a **injeção da falha** e o retorno
  do sistema ao **estado operacional esperado** (serviço/passo novamente
  responsivo); pontos de início/fim e limiar **`[A CONFIRMAR]`**.
- **p50/p95/p99:** percentis da **distribuição de latência por ordem** (unidade
  observacional), agregados por execução/campanha (§3.8).
- **Throughput:** **ordens/Sagas concluídas por unidade de tempo** (RPS
  concluídas) em uma execução.
- **Taxa de erro:** **proporção de ordens/Sagas** que terminam em **erro não
  compensado** (resposta de erro/estado inválido) sobre o total iniciado —
  denominador exato **`[A CONFIRMAR]`** no protocolo.
- **Taxa de compensação:** **proporção de Sagas compensadas** sobre o total de
  Sagas iniciadas — denominador exato **`[A CONFIRMAR]`** no protocolo (total
  iniciado ou apenas as que atingiram o ponto de falha).
- **CPU/memória:** consumo dos **serviços** e do **orquestrador** observado
  (Prometheus) durante a **janela estável** de cada execução; agregação e
  unidade **`[A CONFIRMAR]`**.
- **Mensagens (contagem/duplicidade/perda):** contagem de mensagens
  **publicadas × consumidas** e nº de **duplicatas/reatribuições** detectadas;
  baseline esperado e tolerância **`[A CONFIRMAR]`**.
- **Proxies de observabilidade (RQ3, condicional):** **cobertura de traces**
  (proporção de passos da Saga correlacionados) e **tempo de localização da
  causa** de uma falha; instrumentação e limiar **`[A CONFIRMAR]`**.
- **Proxies de manutenção (RQ4, condicional):** **nº de pontos de alteração** e
  medida de **acoplamento** para uma mudança de fluxo controlada; escala de
  medição **`[A CONFIRMAR]`**.

> **[A CONFIRMAR]** os **limiares** de relevância prática, os **denominadores**
> e a **janela de convergência** da consistência são fixados no protocolo (§3.9),
> antes da coleta. Nenhum resultado ou valor experimental é antecipado aqui.

### 3.7 Piloto

- **[R]** Executar um **piloto de calibração** em **janeiro/2027**, **sem**
  valor probatório: serve para (i) validar o harness de carga, (ii) calibrar
  patamares, (iii) medir o **ruído** do ambiente, (iv) estimar **variância** e
  dimensionar o **nº de repetições**.
- **[R]** O piloto também verifica **equivalência funcional** das duas
  implementações (mesmos resultados de negócio no caminho feliz).
- **[R]** Nenhum número do piloto entra como resultado; apenas orienta o
  protocolo.

### 3.8 Execução, repetições e coleta

**Unidades de análise [R]:**

- **Unidade experimental:** **uma execução** de um cenário de carga/falha para
  **uma** abordagem (uma rodada).
- **Unidade observacional:** **uma ordem/Saga processada** (cada requisição
  individual dentro de uma execução).
- **Unidade de análise:** **uma campanha/run** — o conjunto de execuções de um
  cenário para uma abordagem, sobre o qual se calculam distribuições e
  comparações.

**Critérios objetivos de equivalência funcional [R]:** as duas implementações
devem diferir **apenas** na **coordenação** e ser iguais em:

- **mesmos casos de negócio** (mesmas entradas/contratos);
- **mesmos estados finais** (sucesso, rejeição, compensação);
- **mesmos invariantes** de consistência (**`[A CONFIRMAR]`** no protocolo);
- **mesma carga** e **mesma matriz de falhas** (§3.5);
- **mesma suíte de testes funcionais** aprovada nas duas versões.

**Execução e coleta [R]:**

- **[R] Repetições (decisão incorporada):** **começar com 5
  repetições** por cenário/combinação e **reavaliar após o piloto** (§3.7), por
  análise de variância/análise de poder. O **valor final** (que pode diferir
  de 5) permanece **`[A CONFIRMAR]`** até a conclusão do piloto. Rodadas de
  **aquecimento** são descartadas.
- **[R] Ordem aleatorizada** dos cenários, para reduzir efeitos de ordem.
- **[R] Ambiente isolado**, **nenhum experimento coexecutado**.
- **[R] Coleta automatizada:** scripts produzem arquivos **brutos** e imutáveis
  (um por execução), com metadados de **seed, versão, ambiente e timestamp**.
- **[R]** Nunca editar dados brutos à mão; a limpeza/derivação gera novo arquivo.

### 3.9 Análise

- **[R]** **Não** reportar apenas médias. Descrever **distribuições**: mediana,
  p95, p99, IQR, dispersão.
- **[R]** Reportar **intervalos de confiança** (ex.: **bootstrap**) para as
  diferenças entre abordagens.
- **[R]** Reportar **tamanho de efeito** (ex.: **Cliff's delta** ou equivalente
  não paramétrico), não só p-valor.
- **[R]** Testes **não paramétricos** quando a distribuição não for normal
  (ex.: Mann-Whitney), com correção para múltiplas comparações.
- **[R]** **Regra de decisão condicional** (a fixar no protocolo):
  1. **diferença sem evidência:** IC da diferença contém 0 e/ou efeito abaixo do
     limiar de relevância prática → declarar _“sem evidência de diferença
     relevante neste cenário”_;
  2. **diferença com evidência:** IC **exclui 0**, efeito ≥ limiar de relevância
     e resultado **reproduzido** entre repetições → declarar _“vantagem
     condicional de A sobre B no cenário X”_;
  3. **inconclusivo:** sobreposição/instabilidade → **aumentar repetições** ou
     declarar estudo **subdimensionado** (não forçar conclusão).
- **[R]** O **limiar de relevância prática** e o teste escolhido são definidos
  **antes** da coleta (`[A CONFIRMAR]` no protocolo), para evitar análise _post
  hoc_.

### 3.10 Protocolo experimental (documento de saída de outubro/2026)

**[R]** O protocolo deve conter, no mínimo: RQs e hipóteses congeladas; stack e
versões; topologia; caso de referência; variáveis e controles; cenários de carga
e falha; definição operacional das métricas; nº de repetições; critérios
objetivos de equivalência funcional (§3.8); procedimento de
aleatorização/aquecimento; plano de análise (testes, IC, tamanho de efeito,
regra de decisão); plano de descarte de execuções inválidas.

---

## 4. Cronograma mensal — setembro/2026 a junho/2027

> **Nota sobre o edital:** o edital-base é o **DIRPE nº 7/2026** (PIVIC UFU
> 2026-2027). A **errata** (`ic/errada-edital.webarchive`) pertence à **mesma**
> Chamada (mesmo processo) e **corrige apenas** o prazo de submissão eletrônica
> para **31/12/2026** — **não** é um edital distinto. O **período planejado** de
> execução adotado neste plano é **01/09/2026 a 30/06/2027** **[R]**. O edital
> prevê **PITCH** (vídeo de divulgação ao público leigo) e **Relatório Técnico
> Circunstanciado** (aluno + orientador), este último **até 30 dias após o
> término da vigência** **[E]**. Demais requisitos e datas formais devem ser
> conferidos com o(a) orientador(a).

### 4.1 Visão geral

| Mês          | Foco                      | Entregável principal                                           | Critério de saída                                  |
| ------------ | ------------------------- | -------------------------------------------------------------- | -------------------------------------------------- |
| **Set/2026** | Planejamento + fichamento | Plano de trabalho validado; comparativos prioritários fichados | Escopo e RQ2/RQ1 aprovados; Aydin & Çebi fichado   |
| **Out/2026** | Protocolo                 | **Protocolo experimental** v1                                  | Protocolo revisado pelo(a) orientador(a)           |
| **Nov/2026** | Implementação A           | Saga **coreografada** funcional + testes                       | Caminho feliz e compensações passando              |
| **Dez/2026** | Implementação B           | Saga **orquestrada** funcional + equivalência                  | Testes funcionais iguais nas duas versões          |
| **Jan/2027** | Piloto                    | Relatório de piloto + protocolo ajustado                       | Cargas calibradas; nº de repetições fixado         |
| **Fev/2027** | Experimento — carga       | Execuções de RQ1 concluídas                                    | Dados brutos completos e íntegros                  |
| **Mar/2027** | Experimento — falhas      | Execuções de RQ2 concluídas                                    | Matriz de falhas completa; consistência verificada |
| **Abr/2027** | Análise                   | Resultados analisados (distribuições, IC, efeito)              | Relatório de análise revisado                      |
| **Mai/2027** | Redação                   | Relatório Técnico Circunstanciado + texto IC/TCC               | Documentos em versão de revisão                    |
| **Jun/2027** | Fechamento                | Relatório final + PITCH + apresentação                         | Entrega e compatibilização com o edital concluídas |

### 4.2 Detalhamento mensal

**Setembro/2026 — Planejamento e fichamento**

- **[R]** Validar este plano e o `research/relatorio-validacao-escopo-2026-09-22.md`
  com o(a) orientador(a); fechar RQ2 (principal) e RQ1 (complementar).
- **[E/R]** Fichar os comparativos prioritários **1, 3 e 5** do relatório de
  validação (Rudrabhatla; Aydin & Çebi; Anas Nadeem & Malik) e revisar Marques
  (2024).
- **[E]** **Obtido** o **texto integral de Aydin & Çebi (2022)**
  (`research/sources/aydin-2022.pdf`, via assinatura UFU) e **fichado** — ver
  `research/fichamentos/fichamento-estudos-comparativos-saga.md`.
- **[E]** Kristianto & Zahra (2021): metadados conferidos (JATIT, v. 99, n. 18,
  pp. 4220–4230, 2021; Handi Kristianto e Amalia Zahra); **sem DOI
  identificado** — citar pela URL do PDF (acesso aberto).
- **Entregável:** plano validado + fichamentos atualizados.
- **Critério de saída:** RQs/hipóteses provisórias congeladas para o protocolo;
  pendências bibliográficas restantes encaminhadas.

**Outubro/2026 — Protocolo e preparação**

- **[R]** Escrever o **protocolo experimental** (§3.10), incluindo análise e
  regra de decisão.
- **[R]** **Stack já decidida** (§3.2): Java/Spring Boot, PostgreSQL, RabbitMQ,
  Docker Compose, k6 e Prometheus/OpenTelemetry; validar com o(a) orientador(a)
  — decisão que congela os confundidores.
- **[R]** Estruturar o repositório do experimento (§5) e o `README` inicial.
- **Entregável:** protocolo v1 + esqueleto do repositório.
- **Critério de saída:** protocolo **aprovado**; nenhuma coleta antes disso.

**Novembro/2026 — Implementação da saga coreografada**

- **[R]** Implementar a versão **coreografada** com a lógica compartilhada
  isolada em módulo comum; definir eventos, contratos e compensações.
- **[R]** Escrever testes funcionais (caminho feliz + compensações).
- **Entregável:** versão coreografada funcional, sob versionamento.
- **Critério de saída:** testes do caminho feliz e das compensações passando.

**Dezembro/2026 — Implementação da saga orquestrada + equivalência**

- **[R]** Implementar a versão **orquestrada** reutilizando a **mesma** lógica
  compartilhada; implementar coordenação e compensações.
- **[R]** **Demonstrar equivalência funcional**: mesma suíte de testes
  funcionais nas duas versões.
- **[R]** Preparar _scripts_ de carga e de injeção de falhas (revisão de fim de
  ano: reservar folga de calendário).
- **Entregável:** versão orquestrada + relatório curto de equivalência.
- **Critério de saída:** ambas as versões aprovadas na **mesma** suíte funcional.

**Janeiro/2027 — Piloto e calibração**

- **[R]** Executar o **piloto** (§3.7); medir ruído; calibrar patamares de carga;
  dimensionar repetições.
- **[R]** Ajustar o protocolo e congelá-lo.
- **Entregável:** relatório de piloto + protocolo final.
- **Critério de saída:** nº de repetições e cargas fixados; protocolo **final**.

**Fevereiro/2027 — Experimento (desempenho — RQ1)**

- **[R]** Executar os cenários de **carga** (steady-state, rampa, estresse) para
  as duas abordagens, com ordem aleatorizada e repetições definidas.
- **[R]** Coletar dados brutos com metadados de seed/versão/ambiente.
- **Entregável:** conjunto de dados de RQ1 + registro de execução.
- **Critério de saída:** dados brutos completos; execuções inválidas registradas
  e justificadas.

**Março/2027 — Experimento (falhas/consistência — RQ2, eixo principal)**

- **[R]** Executar a **matriz de injeção de falhas** (§3.5); medir taxa/tempo de
  compensação e consistência final.
- **Entregável:** dados de RQ2 + verificação de consistência.
- **Critério de saída:** todos os cenários de falha executados; consistência
  verificada segundo a definição operacional.

**Abril/2027 — Análise**

- **[R]** Analisar com distribuições, IC (bootstrap) e tamanho de efeito;
  aplicar a regra de decisão condicional (§3.9).
- **[R]** Discutir **ameaças à validade** nas **quatro** categorias de
  experimentos controlados — **conclusão**, **interna**, **de construção** e
  **externa** (Wohlin et al., 2012). **Confiabilidade**, se usada, entra **à
  parte** como critério de **reprodutibilidade** (ver Runeson & Höst, 2009) e
  **não** é atribuída a Wohlin.
- **Entregável:** análise consolidada + tabelas/figuras.
- **Critério de saída:** resultados rastreáveis aos dados brutos; análise
  revisada pelo(a) orientador(a).

**Maio/2027 — Redação e divulgação (início)**

- **[R]** Redigir o **Relatório Técnico Circunstanciado** (aluno + orientador) e
  o texto da IC/TCC, com seções de fundamentação, método, resultados,
  limitações.
- **[R]** Produzir o **PITCH** (vídeo de divulgação ao público leigo, exigido
  pelo edital) **[E]**; preservar o arquivo por **pelo menos 1 ano** e registrar
  o link no relatório **[E]**.
- **Entregável:** relatório e texto em versão de revisão; roteiro do PITCH.
- **Critério de saída:** revisão do(a) orientador(a) incorporada.

**Junho/2027 — Fechamento, entrega e compatibilização com o edital**

- **[R]** Finalizar relatório, PITCH e material de apresentação; conferir
  documentos administrativos (§11.2).
- **[E]** O relatório deve ser encaminhado **até 30 dias após o término da
  vigência**; período planejado de execução: **01/09/2026 a 30/06/2027**;
  prazo de submissão do edital: **31/12/2026** (errata do DIRPE nº 7/2026).
- **[R]** Reservar a **primeira quinzena** para entrega e a **segunda** para
  ajustes/pendências do edital (margem de segurança).
- **Entregável:** relatório final + PITCH + apresentação.
- **Critério de saída:** entrega realizada conforme o edital; pendências
  administrativas encerradas.

---

## 5. Organização do repositório

### 5.1 Estrutura recomendada (sem editar o que já existe)

| Caminho                     | Uso                                                             | Situação                             |
| --------------------------- | --------------------------------------------------------------- | ------------------------------------ |
| `research/sources/`         | Cópias de conveniência de fontes (não são fonte oficial)        | **[E]** já existe                    |
| `research/fichamentos/`     | Fichamentos por obra (`fichamento-<autor-ano>.md`)              | **[E]** já existe                    |
| `research/` (raiz)          | Manifesto de fontes, relatórios de trabalho, atas/diário        | **[E]** já existe                    |
| `docs/`                     | Documentos de apoio (este plano, relatórios de escopo)          | **[E]** já existe                    |
| `ic/submissao/`             | Rascunhos de submissão e formulários institucionais             | **[E]** já existe                    |
| `tcc/`                      | Documento formal de TCC (só conteúdo validado)                  | **[E]** já existe                    |
| `experimento/` (**nova**)   | Código, infraestrutura, scripts e dados do experimento          | **[R]** a criar **após** o protocolo |
| `experimento/codigo/`       | Duas implementações + módulo de lógica compartilhada            | **[R]**                              |
| `experimento/infra/`        | `docker-compose`, configuração de broker/banco, versões fixadas | **[R]**                              |
| `experimento/carga/`        | Scripts de carga e de injeção de falhas                         | **[R]**                              |
| `experimento/dados-brutos/` | Saída imutável por execução (não editar à mão)                  | **[R]**                              |
| `experimento/analise/`      | Scripts de análise reprodutível + figuras/tabelas               | **[R]**                              |
| `experimento/registros/`    | Decisões, ambiente e versões                                    | **[R]**                              |

> **[A CONFIRMAR]** o nome exato da pasta de experimento é detalhe do protocolo;
> a **política de versionamento** (o repositório **não** é git hoje — ver §8) é
> uma **decisão interna de protocolo**, **não** uma pendência bibliográfica.
> Sugestão: manter a convenção `kebab-case` do `AGENTS.md`.

### 5.2 Convenções de nomes

- **[E]** `kebab-case`, minúsculas, sem acentos e sem espaços; datas ISO
  `AAAA-MM-DD`; lacunas sempre como `[A CONFIRMAR]`; fichamentos
  `fichamento-<autor-ano>.md`; atas `ata-<AAAA-MM-DD>.md`.
- **[R]** Arquivos de dados: `<cenário>-<abordagem>-<AAAA-MM-DD>-seed<N>.csv`
  (ou equivalente), com metadados no cabeçalho/arquivo irmão.
- **[R]** Todo rascunho indica o status no topo (ex.: “NÃO OFICIAL”).

### 5.3 Diário / ata e registro de decisões

- **[R] Diário do aluno:** `research/diario-rafael.md` (ou `experimento/diario.md`),
  com entradas datadas (ISO): o que foi feito, bloqueios, próximos passos.
- **[R] Atas de reunião:** `research/atas/ata-AAAA-MM-DD.md` — decisões,
  responsáveis e prazos.
- **[R] Registro de decisões:** `experimento/registros/decisoes.md` (formato
  ADR simplificado: contexto, decisão, alternativas, consequências).
- **[R] Registro de ambiente:** `experimento/registros/ambiente.md` — SO, CPUs,
  memória, versões de linguagem/framework/broker/banco, configuração.
- **[R] Registro de versões e seeds:** cada execução guarda **commit/tag** do
  código, **seed** e parâmetros. Nunca “Média_Final_v2_real_final.csv”.

---

## 6. Rotina de trabalho do aluno

### 6.1 Ciclo semanal [R]

| Bloco                      | Conteúdo                                                        | Frequência                            |
| -------------------------- | --------------------------------------------------------------- | ------------------------------------- |
| **Reunião de orientação**  | Progresso, bloqueios, decisões, próximos passos; ata registrada | semanal **[A CONFIRMAR]** dia/horário |
| **Bloco de leitura**       | Fontes primárias; fichamento com rótulos [E]/[R]/[?]            | ~2 blocos/semana **[R]**              |
| **Bloco de implementação** | Código, testes, scripts, experimentos                           | demais dias úteis                     |
| **Atualização do diário**  | Entrada datada ao fim do dia/bloco                              | diária                                |
| **Revisão por checklist**  | Conferir entregáveis do mês (§4) e checklists (§10, §11)        | semanal                               |

> **[E]** O edital exige **20 horas semanais** de atividades do aluno
> voluntário. A rotina deve ser compatível com essa carga.

### 6.2 Papéis

| Papel                             | Responsabilidades                                                                                                                                                                                        |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Aluno (Rafael)**                | Executar o plano; manter diário e fichamentos; implementar e testar; coletar dados; analisar; redigir; produzir o PITCH; apresentar resultados em eventos; manter Lattes e contatos atualizados **[E]**. |
| **Orientador(a) (Paulo Rodolfo)** | Validar escopo/protocolo; decidir stack e infra; orientar e acompanhar; revisar relatórios; auxiliar no PITCH; enviar o Relatório Técnico Circunstanciado e prestar contas **[E]**.                      |

---

## 7. Fluxo bibliográfico

**[R]** Fluxo padrão por obra:

1. **Ler a fonte primária** (não confiar em resumos de terceiros).
2. **Fichar** em `research/fichamentos/`, registrando referência completa,
   objetivo, desenho, resultados e limitações.
3. **Marcar cada afirmação:** **[E]** verificado na fonte, **[R]** recomendação,
   **[?]** pendência (nada marcado como [?] é fato).
4. **Registrar DOI/URL e data de acesso**; anotar **limitações** e licença de
   reuso.
5. **Não usar** fonte não verificada em `tcc/` ou em afirmações do relatório.
6. **Só citar** após conferência da fonte primária.

**Pendências bibliográficas herdadas:**

| Pendência                                                   | Situação                                                                                                                                                                                                                                                                                                                                          |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Aydin & Çebi (2022)** — texto integral                    | **[E]** **Obtido e fichado** (`research/sources/aydin-2022.pdf`, via assinatura UFU/IEEE). Resultados registrados apenas no que o texto sustenta.                                                                                                                                                                                                 |
| **Kristianto & Zahra (2021)** — dados bibliográficos        | **[E]** conferidos: JATIT, v. 99, n. 18, pp. 4220–4230, 2021; **sem DOI identificado** — citar pela URL do PDF (acesso aberto).                                                                                                                                                                                                                   |
| **Rudrabhatla (2018)** — razões 1:3,6:8,2 vs. 1:3,9:6,4     | **[E]** Resolvido: são **razões de tempo de resposta** sob carga **1×:5×:10×** (não latências absolutas); "~40×" é relato **aproximado** da seção IV.B para 4/6 microsserviços e 5 execuções, sem tabela/tempos absolutos.                                                                                                                        |
| **Marques (2024)** — versões/stack, ambiente/carga e código | **[E]** **Não informados na fonte primária** (sem versão experimental específica, ambiente/carga ou código-fonte). Metodologia **confirmada** (corpo do PDF): estudo misto com implementação + _survey_ de 14 profissionais. Os números de **JMeter/1–1000 usuários** são de **Kristianto & Zahra (2021)**, não de Marques.                       |
| **Garcia-Molina & Salem (1987)** — versão do PDF            | **[E]** Resolvido: a versão do PDF presente no repositório é a versão a considerar.                                                                                                                                                                                                                                                               |
| **Richardson (2018), cap. 4**                               | **[E]** adotado como **fonte central da fundamentação** sobre Saga, coreografia × orquestração e consistência. Cap. 4, **pp. 110–145**; páginas-chave **115–116, 118, 121 e 126–129**. **Não** é fonte de resultados experimentais; citações **literais** exigem reconferência em **edição licenciada** (o PDF local **não** é cópia licenciada). |
| **Wohlin et al. (2012)** — ameaças à validade               | **[E]** Resolvido: **quatro** categorias (conclusão, interna, de construção, externa). **Confiabilidade** não é atribuída a Wohlin; quando usada, entra à parte como critério de **reprodutibilidade** (ver Runeson & Höst, 2009).                                                                                                                |

> **[R] microservices.io — orientação interna de uso:** conceitos e diagramas de
> **Chris Richardson / microservices.io** podem ser usados no trabalho acadêmico
> com **atribuição** ao autor/fonte, **preferência por paráfrase** e
> **diagramas adaptados**, e **citação/legenda adequada** (ex.: *Fonte: Adaptado
> de Richardson (microservices.io)*). Trata-se de **orientação interna de
> redação**, **não** de parecer jurídico nem de licença externa verificada.

> **[R]** A **política de versionamento** do repositório e a forma/licença de
> publicação do artefato são **decisões internas de protocolo** (§5.1 e §8),
> **não** pendências bibliográficas.

> **[R]** Manter a distinção entre as camadas de fontes já registradas
> **[E]**: **fundacional** (Garcia-Molina & Salem, 1987) e de **fundamentação
> conceitual/industrial** (Richardson 2018 — **fonte central**; microservices.io;
> AWS; Azure). Richardson (2018) e a **Azure** tratam explicitamente das duas
> coordenações; a página AWS local **não** cobre a comparação. Nenhuma dessas
> fontes fornece os **resultados experimentais** — esses são deste projeto.

---

## 8. Plano de reprodutibilidade

**[R]** Objetivo: qualquer terceiro consegue reexecutar o experimento a partir
das instruções do repositório.

| Item                | Requisito                                                               |
| ------------------- | ----------------------------------------------------------------------- |
| **Código**          | Duas implementações + módulo de lógica compartilhada, versionados       |
| **Infraestrutura**  | `Docker Compose` (ou equivalente) subindo broker, banco e serviços      |
| **Versões fixadas** | Linguagem, framework, broker, banco e bibliotecas com versão exata      |
| **Scripts**         | Carga e injeção de falhas automatizadas e idempotentes                  |
| **Dados brutos**    | Um arquivo imutável por execução, com seed e metadados                  |
| **Análise**         | Scripts reprodutíveis que geram tabelas/figuras a partir dos brutos     |
| **README**          | Passo a passo: pré-requisitos, como subir, como executar, como analisar |

- **[R]** Versionar **desde o início**; a **política de versionamento** (o
  repositório **não é git** hoje) é **decisão interna de protocolo**, a definir
  com o(a) orientador(a) — **não** é pendência bibliográfica.
- **[R]** Registrar seeds, versões e ambiente (§5.3).
- **[R]** **Não criar experimentos antes do protocolo** (§3; outubro/2026).
- **[A CONFIRMAR]** forma e licença de publicação do artefato (decisão interna
  de protocolo).

---

## 9. Riscos e contingências

### 9.1 Matriz de riscos

| Risco                               | Sinal/descrição                   | Mitigação                                                      | Plano B                                                                     |
| ----------------------------------- | --------------------------------- | -------------------------------------------------------------- | --------------------------------------------------------------------------- |
| **Atraso de acesso bibliográfico**  | Aydin & Çebi e outros não obtidos | Solicitar via UFU em **set/2026**; usar OA já obtidos          | Registrar “texto indisponível” e **não citar resultados**; seguir com os OA |
| **Escopo excessivo**                | Muitos cenários/métricas          | Fixar RQ2 + RQ1 no MVP; RQ3/RQ4 condicionais                   | Cortar RQ3/RQ4 e cenários extras (§9.2)                                     |
| **Ambiente instável**               | Ruído, variância alta, quedas     | Piloto mede ruído; isolamento; repetições; aquecimento         | Aumentar repetições; reportar variabilidade                                 |
| **Implementações não equivalentes** | Controle = confundidor            | Módulo de lógica compartilhada + suíte funcional única         | Refatorar até equivalência; documentar diferenças residuais                 |
| **Dados faltantes/corrompidos**     | Execuções incompletas             | Scripts validam integridade; critério de descarte **a priori** | Reexecutar cenário; registrar motivo                                        |
| **Resultado nulo**                  | Sem diferença relevante           | **Não** é fracasso: é achado condicional                       | Relatar “sem evidência de diferença” com IC e efeito                        |
| **Dependência de terceiros**        | Versões/frameworks mudam          | Fixar versões; `Docker`                                        | Documentar mudança de versão                                                |
| **Tempo para o PITCH/relatório**    | Concentração no fim               | Reservar **mai–jun/2027**; roteiro adiantado                   | Blocos semanais dedicados à redação                                         |

### 9.2 Critérios de corte (ordem de prioridade)

**[R]** Se o prazo apertar, cortar **nesta ordem** (tudo abaixo do **baseline**
vem **depois** dele):

1. **replicação múltipla** — escalonamento horizontal (§3.4), que é cenário
   secundário/extensão;
2. **observabilidade avançada** — métricas/proxies de RQ3;
3. métricas/proxies de **manutenção** (RQ4);
4. **tamanho da saga** (manter os **4 passos** do fluxo-base de liquidação de
   ordem fixos);
5. cenários de carga extras (manter steady-state + 1 rampa);
6. cenários de falha menos informativos (manter os que exercitam compensação).

**Não cortar (baseline inegociável):** baseline com **uma instância por
serviço**, **bancos logicamente isolados por serviço** (§3.2), **compensações**
e **consistência** (RQ2, eixo principal), além de **equivalência funcional**,
**dados brutos** e **rastreabilidade**.

---

## 10. Checklists

### 10.1 Início imediato (aluno)

- [ ] **[R]** Ler este plano e `research/relatorio-validacao-escopo-2026-09-22.md`.
- [ ] **[R]** Agendar reunião com o(a) orientador(a) para validar escopo e RQ2/RQ1.
- [x] **[E]** Texto integral de **Aydin & Çebi (2022)** obtido e fichado.
- [x] **[E]** Kristianto & Zahra (2021) conferido (JATIT, v. 99, n. 18, pp. 4220–4230, 2021); **sem DOI identificado**.
- [ ] **[R]** Fichar os comparativos prioritários (1, 3 e 5 do relatório de validação).
- [ ] **[R]** Criar o diário e a ata da primeira reunião.
- [x] **[R]** Stack decidida: Java/Spring Boot, PostgreSQL, RabbitMQ, Docker Compose, k6, Prometheus/OpenTelemetry (validar com o(a) orientador(a)).
- [x] **[E]** Modalidade/edital confirmados: PIVIC, DIRPE nº 7/2026 (errata: prazo 31/12/2026); período planejado 01/09/2026–30/06/2027.
- [x] **[E]** Dados cadastrais do aluno conferidos e preenchidos em
      `dados-aluno.md` (CPF, matrícula, telefone e Lattes).
- [ ] **[R]** Não iniciar implementação/experimento antes do protocolo aprovado.

### 10.2 Aprovação do orientador(a)

- [ ] Escopo, título e delimitação validados.
- [ ] RQ2 (principal) e RQ1 (complementar) aprovadas; RQ3/RQ4 como opcionais.
- [x] Stack, broker, banco, orquestrador e ambiente decididos (validar com o(a) orientador(a)).
- [ ] Protocolo experimental aprovado (métricas, repetições, análise, regra de decisão).
- [ ] Plano de reprodutibilidade e licença de publicação aprovados.
- [ ] Critérios de corte acordados.
- [x] Modalidade/edital confirmados: PIVIC, DIRPE nº 7/2026 (errata: prazo 31/12/2026); período planejado 01/09/2026–30/06/2027.
- [x] Rotina semanal e formato de citação (**ABNT**) definidos.
- [x] **[E]** Vinculação IC × TCC: **não há impedimento identificado**; IC e TCC **podem compartilhar o mesmo experimento** (orientação confirmada).

---

## 11. Definição de pronto (IC/TCC compartilhados)

### 11.1 Critérios de pronto do trabalho

| Critério                       | Descrição                                                                                                                                               |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Experimento executado**      | Cenários de RQ1 e RQ2 completos, conforme o protocolo congelado.                                                                                        |
| **Dados rastreáveis**          | Cada resultado liga-se a um arquivo bruto + seed + versão + ambiente.                                                                                   |
| **Análise**                    | Distribuições, IC e tamanho de efeito; regra de decisão aplicada; ameaças à validade discutidas.                                                        |
| **Limitações**                 | Escopo declarado; **nenhuma** generalização além do cenário; nenhuma alegação de superioridade universal.                                               |
| **Documentos administrativos** | Relatório Técnico Circunstanciado (aluno + orientador) e **PITCH** conferidos; Lattes e contatos atualizados; referência à UFU nas divulgações **[E]**. |
| **Reprodutibilidade**          | Código, infra, scripts e README permitem reexecução por terceiros.                                                                                      |
| **Bibliografia**               | Fontes usadas fichadas e conferidas na primária; pendências resolvidas ou explicitamente marcadas.                                                      |

### 11.2 Conferência administrativa final

- [x] **[E]** Modalidade/edital: PIVIC — DIRPE nº 7/2026 (errata: prazo de submissão 31/12/2026); período planejado 01/09/2026–30/06/2027.
- [x] **[E]** Vinculação IC/TCC: **sem impedimento identificado**; IC e TCC podem compartilhar o mesmo experimento; extensões opcionais.
- [ ] **[E]** Relatório entregue **até 30 dias após o término da vigência**
      (aluno + orientador).
- [ ] **[E]** PITCH produzido e link informado **no próprio Relatório Técnico
      Circunstanciado**; vídeo guardado por **≥ 1 ano**.
- [x] **[E]** Menção explícita ao apoio da UFU no material de divulgação.
- [ ] **[E]** Certificação exige **≥ 6 meses** de participação.
- [x] **[E]** Formato de citação: **ABNT**. Licenças de reuso das fontes conferidas; uso de **microservices.io** conforme **orientação interna** (§7), com atribuição, preferência por paráfrase e diagramas adaptados.
- [ ] **[?]** Licença e forma de publicação do artefato.

---

## Apêndice A — Correspondência RQ → métrica → cenário → dados

| RQ             | Métrica primária                              | Cenário                 | Origem dos dados      |
| -------------- | --------------------------------------------- | ----------------------- | --------------------- |
| **RQ2**        | taxa/tempo de compensação; consistência final | matriz de falhas (§3.5) | execuções de mar/2027 |
| **RQ1**        | p95/p99; throughput; erro                     | cargas (§3.4)           | execuções de fev/2027 |
| **RQ3** (ext.) | cobertura de traces; tempo de diagnóstico     | falhas                  | condicional           |
| **RQ4** (ext.) | pontos de alteração; acoplamento              | alteração de fluxo      | condicional           |

## Apêndice B — Fatos [E] usados como linha de base

| Fonte                        | Fato verificado                                                                                                                                                                                                                                                                                               | Uso no projeto            |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- |
| Garcia-Molina & Salem (1987) | Fundamento de saga, compensação, backward/forward                                                                                                                                                                                                                                                             | Fundamentação             |
| Rudrabhatla (2018)           | Coreografia mais rápida; **~40×** aproximado na **seção IV.B** (4/6 microsserviços, 5 execuções); razões **1:3,6:8,2** × **1:3,9:6,4** são de **tempo de resposta** sob carga **1×:5×:10×**; Spring Boot/Eureka/MongoDB isolado/ActiveMQ/VMs Linux; sem hardware/versões/req/s/escala horizontal; sem p95/p99 | Linha de base RQ1         |
| Kristianto & Zahra (2021)    | Coreografia melhor desempenho/CPU; mudança 6 APIs/4 serviços vs. 1 API/1 coordenador                                                                                                                                                                                                                          | Linha de base RQ1/RQ4     |
| Marques (2024)               | Coreografia flexível/escalável/menor latência simples; orquestração melhor controle de erro + SPOF; estudo misto (implementação + _survey_ n = 14 profissionais); versões/ambiente/carga e código-fonte **não informados**                                                                                    | Linha de base RQ2/RQ3/RQ4 |
| Aydin & Çebi (2022)          | Obra diretamente sobre Saga coreografia × orquestração; coreografia mais rápida (tempo/memória); orquestração indicada para cenários complexos; **integral obtido e fichado**                                                                                                                                 | Linha de base RQ1/RQ2     |
| Anas Nadeem & Malik (2022)   | TrainTicket → Temporal; depuração; n = 1; artefato aberto                                                                                                                                                                                                                                                     | Antirreinvenção           |
| Azure Architecture Center    | Compara explicitamente coreografia × orquestração; compensable/pivot/retryable; idempotência                                                                                                                                                                                                                  | Fundamentação             |

---

_Documento de trabalho — não oficial. Elaborado exclusivamente a partir de
`AGENTS.md`, `research/relatorio-validacao-escopo-2026-09-22.md`,
`research/fichamentos/`, `research/sources-manifest.md`, `ic/proposta-rafael.md`
e dos modelos institucionais em `ic/`. Sem nova busca bibliográfica e sem
invenção de datas, resultados, ferramentas obrigatórias ou dados administrativos.
Sujeito a validação pelo(a) orientador(a) e compatibilização com o edital._
