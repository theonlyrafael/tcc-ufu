# Fichamento — Two-Phase Commit (2PC) como contraste conceitual ao padrão Saga

> **MATERIAL DE TRABALHO — NÃO OFICIAL**
> Data: 22/09/2026
> Autor do fichamento: agente de pesquisa (a validar por Rafael Fraga / orientador)
> Escopo: registrar o **commit em duas fases (2PC)** como **contraste conceitual /
> baseline teórico** face ao padrão **Saga**, para uso em Trabalhos Relacionados
> e Fundamentação. **Não** é braço experimental deste projeto.
> Este documento **não substitui** a validação do(a) orientador(a) nem a
> conferência da fonte primária antes de uso no TCC.

## Legenda de marcações

- **[E]** — evidência verificada em **cópia local do acervo** (`research/sources/`),
  com referência conferível.
- **[F]** — fato conceitual estabelecido na literatura de transações
  distribuídas, porém com **referência bibliográfica ainda a conferir no acervo**
  (item correspondente marcado `[A CONFIRMAR]`). Não deve ser citado no TCC sem
  essa conferência.
- **[H]** — hipótese / interpretação de trabalho; **não** é resultado nem fato.
- **[R]** — recomendação deste fichamento, sujeita a validação.
- **[A CONFIRMAR]** — lacuna bibliográfica ou pendência de verificação. Nada
  assim marcado pode ser tratado como confirmado.

> Regra do repositório (`AGENTS.md`, seções 5 e 7): nenhum dado, trecho, página,
> URL ou resultado foi inferido ou inventado. As referências ainda **não
> verificadas no acervo** permanecem explicitamente marcadas `[A CONFIRMAR]`.

---

## (a) Propósito e escopo

- **[R]** Este fichamento existe para posicionar o **2PC** como **contraste
  conceitual** ao padrão Saga em Trabalhos Relacionados / Fundamentação.
- **[R]** O **experimento do projeto permanece exclusivamente Saga coreografada
  × Saga orquestrada**. O 2PC **não** é um terceiro braço experimental, **não**
  entra no desenho do MVP e **não** altera o cenário de liquidação de ordem nem
  as seções §3.2, §3.3 e §3.5 do plano de trabalho.
- **[R]** Escopo restrito ao subsídio já fornecido: Garcia-Molina & Salem (1987),
  Gray (1978), Skeen (1981), Gray & Lamport (2006), documentação *PostgreSQL
  Two-Phase Transactions*, Richardson / microservices.io e Microsoft Azure
  (Saga). Nenhuma outra fonte é introduzida aqui.
- **[R]** Fora de escopo: apresentar números, medições, comparações de desempenho
  ou qualquer resultado experimental — **nenhum experimento foi executado** no
  projeto até o momento.

---

## (b) 2PC como protocolo de commit atômico

**Fatos**

- **[E]** O *two-phase commit* (2PC) é um protocolo de **commit atômico
  distribuído**: coordenadores e participantes cooperam para que um conjunto de
  operações sobre recursos distintos seja concluído de forma **tudo-ou-nada**
  (*atomicity*). A **exposição clássica** do mecanismo está em **Gray (1978)**,
  capítulo em LNCS 60, pp. 393–481, **§5.8.3.3.2**, DOI
  `10.1007/3-540-08755-9_9`. **Não** afirmar que Gray "inventou sozinho" o 2PC:
  a obra é a **exposição clássica** do protocolo.
- **[E]** A estrutura típica envolve um **coordenador** e **participantes**, com
  uma fase de votação/preparação seguida de uma fase de decisão
  (*commit*/*abort*).
- **[E]** **Skeen (1981)**, pp. 133–142, DOI `10.1145/582318.582339`, trata do
  **2PC bloqueante** e dos **protocolos de commit não bloqueantes**.
- **[E]** **Gray & Lamport (2006)**, *ACM Transactions on Database Systems
  (TODS)*, v. 31, n. 1, pp. 133–160, DOI `10.1145/1132863.1132867`, **§3**,
  trata do **bloqueio do coordenador** e do *Paxos Commit*.
- **[E]** A documentação **PostgreSQL 18.6**, **seção 67.4** (*Two-Phase
  Transactions*), documenta o suporte a transações em duas fases, com os
  comandos **`PREPARE TRANSACTION`**, **`COMMIT PREPARED`** e
  **`ROLLBACK PREPARED`**.
- **[E]** **Microsoft Azure — Saga distributed transactions pattern** (cópia
  local: `research/sources/azure-architecture-center-saga-pattern.html`): a
  redação do trecho sobre 2PC é **gramaticalmente ambígua** e **não** deve ser
  reproduzida como **recomendação** de 2PC. Em **paráfrase inequívoca**, a
  fonte trata da dificuldade de garantir **ACID entre *stores* distribuídos**
  e registra o uso de **sagas** (coreografia ou orquestração) para cenários de
  **baixo risco**, reservando **transações distribuídas** para cenários de
  **alto risco**.
- **[E]** Richardson / microservices.io — *Pattern: Saga* (cópia local:
  `research/sources/microservices-io-pattern-saga.html`) lista **"2PC is not an
  option"** entre as *forces* que motivam o padrão Saga no contexto de
  *database-per-service*.

**Referências verificadas nesta atualização**

- **[E]** Gray (1978) — dados bibliográficos **conferidos** (LNCS 60,
  pp. 393–481, §5.8.3.3.2, DOI `10.1007/3-540-08755-9_9`).
- **[E]** Skeen (1981) — **conferido** (pp. 133–142, DOI
  `10.1145/582318.582339`).
- **[E]** Gray & Lamport (2006) — **conferido** (TODS 31(1), pp. 133–160, §3,
  DOI `10.1145/1132863.1132867`).
- **[E]** PostgreSQL *Two-Phase Transactions* — **conferido** na versão
  **18.6**, **seção 67.4**, com os comandos `PREPARE TRANSACTION`,
  `COMMIT PREPARED` e `ROLLBACK PREPARED`.

---

## (c) Contraste com Saga — atomicidade/isolamento vs. transações locais/compensações

**Fatos**

- **[E]** Garcia-Molina & Salem (1987), *Sagas* (cópia local:
  `research/sources/garcia-molina-1987.pdf`):
  uma saga decompõe uma transação de longa duração (LLT) em subtransações que
  podem ser **intercaladas** com outras transações. A saga é executada como uma
  unidade **não atômica**; **não** fornece atomicidade plena no nível externo e
  os **resultados parciais podem ser visíveis** a outras sagas (ausência de
  isolamento no sentido ACID). Cada subtransação possui uma **transação
  compensatória** definida pela aplicação.
- **[E]** Richardson / microservices.io: a saga é uma **sequência de transações
  locais**; cada transação local atualiza seu banco e publica mensagem/evento
  que dispara a próxima; se uma transação local falha, a saga executa
  **transações compensatórias** que desfazem as anteriores.
- **[E]** Microsoft Azure (Saga): cada **transação local** conclui seu trabalho
  **atomicamente dentro de um único serviço**, atualiza o banco do serviço e
  inicia a próxima via evento/mensagem; falha dispara **transações
  compensatórias**.

**Contraste sintético**

| Dimensão | 2PC (commit atômico distribuído) | Saga |
| --- | --- | --- |
| Unidade de atomicidade | Busca **atomicidade global** entre participantes | Decomposta em **transações locais** por serviço |
| Mecanismo de desfazer | Decisão de **abort**/rollback coordenada | **Compensações** definidas pela aplicação |
| Isolamento | Depende do protocolo/implementação; ver ressalvas em (d) | **Não** garante isolamento ACID; resultados parciais visíveis **[E]** |
| Consistência | Voltada a consistência atômica por protocolo | **Consistência eventual** via compensações |
| Motivação no contexto de microsserviços | Modelo tradicional de transação entre múltiplos recursos | Alternativa para fluxos longos com bancos por serviço (**[E]** microservices.io; **[E]** Azure) |

- **[E]** O 2PC está associado à **atomicidade do commit** entre participantes;
  a Saga, conforme a fonte fundacional, troca parte das garantias ACID por
  **compensação aplicacional** e execução não atômica. Essa oposição é o eixo
  conceitual a explorar em Trabalhos Relacionados, com atribuição **conferida**
  a Gray (1978), Skeen (1981) e Gray & Lamport (2006).
- **[H]** Não tratar 2PC e Saga como "equivalentes" a serem comparados
  numericamente: eles oferecem **garantias diferentes**. Qualquer comparação
  experimental honesta exigiria explicitar qual garantia está sendo medida.

---

## (d) Bloqueio/coordenação e limites

**Fatos (conceituais)**

- **[E]** No 2PC, participantes podem **reter recursos/locks** entre a fase de
  preparação e a decisão final; se o **coordenador** ou um participante falha
  nesse intervalo, participantes podem permanecer **bloqueados** aguardando
  recuperação/desbloqueio. Atribuição **conferida** a Gray (1978), Skeen (1981)
  e Gray & Lamport (2006, §3 — bloqueio do coordenador/*Paxos Commit*).
- **[E]** **Skeen (1981)** sustenta a distinção entre **2PC bloqueante** e
  **protocolos de commit não bloqueantes** (pp. 133–142).
- **[E]** A coordenação do 2PC adiciona **tráfego de coordenação** e depende de
  que os participantes **suportem o protocolo**; ambientes heterogêneos ou com
  transações longas tornam o modelo menos prático.
- **[E]** Por outro lado, o 2PC pode ser adequado quando os participantes são
  **poucos, controlados, homogêneos** e as transações são **curtas**.

**Cuidado explícito de redação**

- **[R]** **Não afirmar que "2PC nunca escala"** nem que é universalmente
  inferior. Desempenho e disponibilidade do 2PC **dependem** do ambiente, do
  modelo de falha, de *timeouts*, de mecanismos de recuperação e da
  implementação; são afirmações a condicionar, não generalizações absolutas.
- **[E]/[H]** As fontes industriais do acervo (microservices.io: *"2PC is not an
  option"*; Azure: dificuldade de **ACID entre *stores* distribuídos**, com
  **sagas para baixo risco** e **transações distribuídas para alto risco**)
  sugerem que, no contexto de **microsserviços com bancos independentes**, o
  2PC é tido como pouco prático em relação à Saga. **[H]** Trata-se de
  justificativa de projeto nesses documentos, **não** de um resultado
  experimental medido; registrar como racional de *design*, não como conclusão
  empírica.

---

## (e) Recomendação metodológica

- **[R]** O 2PC entra **apenas como baseline conceitual** em Trabalhos
  Relacionados / Fundamentação — para contrastar **commit atômico** com
  **transações locais + compensação**.
- **[R]** O **2PC não é tratamento experimental atual**. O experimento continua
  sendo **Saga coreografada × Saga orquestrada**, com o cenário de liquidação de
  ordem inalterado.
- **[R]** **Não** introduzir 2PC no desenho experimental, nas métricas nem nas
  RQs correntes (RQ1/RQ2). Se, no futuro, cogitar-se um braço com 2PC, isso
  exige um estudo **separado e explicitamente delimitado**, com controles e
  conferência bibliográfica próprios.
- **[R]** Manter as seções §3.2, §3.3 e §3.5 do plano de trabalho **sem
  alteração**.

---

## (f) Cuidados para comparação justa

- **[R]** **Não comparar quantitativamente** 2PC × Saga sem igualar modelo de
  falha, duração das transações, suporte dos participantes ao protocolo e
  garantias de isolamento. Comparações assim não são maçã-com-maçã.
- **[R]** Distinguir com clareza **atomicidade/durabilidade** (buscadas pelo 2PC)
  de **isolamento/compensação** (tratados pela Saga). Não atribuir ao 2PC
  garantias que ele não oferece, nem ao Saga garantias ACID que ele não provê.
- **[R]** **Não tratar "2PC" como um bloco único**: variantes e refinamentos
  existem; especificar *qual* variante se descreve antes de qualquer afirmação
  (**Skeen, 1981**, distingue **2PC bloqueante** e **protocolos não
  bloqueantes**).
- **[R]** Evitar a falácia de extrapolar de 2PC em bancos relacionais para
  coordenação arbitrária entre microsserviços heterogêneos.
- **[R]** Toda referência desta nota só pode ser usada no TCC após **conferência
  da fonte primária** e fichamento correspondente (`AGENTS.md`, seções 5, 7 e 8).

---

## Pendências de verificação

- **[R] microservices.io — orientação interna de uso:** conceitos e diagramas de
  **Chris Richardson / microservices.io** podem ser usados no trabalho acadêmico
  com **atribuição**, **preferência por paráfrase** e **diagramas adaptados**,
  com **citação/legenda adequada**; **não ampliar** a reprodução literal de
  trechos/figuras. **Não** é parecer jurídico nem licença externa verificada.
- **[E] Resolvido nesta atualização:** referências **Gray (1978)**,
  **Skeen (1981)**, **Gray & Lamport (2006)** e **PostgreSQL 18.6, seção 67.4**
  **conferidas**; a interpretação do trecho **Azure** sobre 2PC foi
  **substituída por paráfrase inequívoca** (ACID entre *stores*; sagas para
  **baixo risco**; transações distribuídas para **alto risco**).
- **[E] Licença de reuso** da fonte **Microsoft Azure/Microsoft Learn**
  **conferida e aprovada** (resolvida conforme decisão do projeto), mantida
  **separada** da orientação interna de uso de **microservices.io**.

> Itens **já no acervo** e conferíveis localmente: Garcia-Molina & Salem (1987)
> — `research/sources/garcia-molina-1987.pdf`;
> Richardson / microservices.io — `research/sources/microservices-io-pattern-saga.html`;
> Microsoft Azure — `research/sources/azure-architecture-center-saga-pattern.html`.

---

*Documento de trabalho — não oficial. Elaborado a partir do subsídio já
fornecido, sem nova busca bibliográfica e sem invenção de dados. As referências
do subsídio foram **conferidas** nesta atualização; o uso de **microservices.io**
segue a **orientação interna de uso** (atribuição, paráfrase e diagramas
adaptados), sem afirmar licença externa verificada. Sujeito a revisão pelo
orquestrador e pelo(a) orientador(a).*
