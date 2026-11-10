# Fichamento — Estudos comparativos: Saga coreografada × orquestrada

> **DOCUMENTO DE TRABALHO — NÃO OFICIAL**
> Data: 22/09/2026
> Autor do fichamento: agente de pesquisa (a validar por Rafael Fraga / orientador)
> Escopo: registrar e comparar os estudos empíricos que já comparam as
> abordagens **coreografada (choreography)** e **orquestrada (orchestration)**
> do padrão Saga em microsserviços, para fundamentar a lacuna e o recorte de
> IC/TCC.

## Legenda de marcações

- **[E]** — texto integral confirmado na fonte consultada (PDF/cópia oficial
  aberta), com referência conferível.
- **[R]** — metadado/registro secundário: referência, DOI, *handle* ou URL
  conferidos apenas como registro bibliográfico, **sem** leitura integral nesta
  etapa.
- **[?]** — pendência: acesso não obtido, dado a confirmar ou interpretação em
  aberto. Nada marcado como **[?]** deve ser tratado como confirmado.

> Regra do repositório (`AGENTS.md`, seções 5 e 7): nenhum dado foi inferido ou
> inventado. As conclusões dos artigos foram registradas **somente** quando
> sustentadas pelo texto. Onde o texto integral não foi acessado, os campos
> ficam explicitamente em branco / marcados **[?]**.
>
> Este fichamento **não substitui** a validação formal do(a) orientador(a) nem
> a conferência final da fonte primária antes de uso no TCC.

---

## 1. Rudrabhatla (2018) — *Comparison of Event Choreography and Orchestration Techniques in Microservice Architecture*

### Referência e acesso

- **Autor:** C. K. Rudrabhatla. **[E]**
- **Título:** *Comparison of Event Choreography and Orchestration Techniques
  in Microservice Architecture*. **[E]**
- **Veículo:** International Journal of Advanced Computer Science and
  Applications (IJACSA), v. 9, n. 8, pp. 18–22. **[E]**
- **DOI:** `10.14569/IJACSA.2018.090804`. **[E]**
- **Cópia aberta (OA):**
  <https://thesai.org/Downloads/Volume9No8/Paper_4-Comparison_of_Event_Choreography.pdf>
  — **[E]** PDF de acesso aberto; **texto integral confirmado** no material
  disponibilizado.
- **Licença de reuso:** **[E]** conferida e aprovada antes de reproduzir
  trechos/figuras (resolvida conforme decisão do projeto).

### Objetivo

**[E]** Comparar, do ponto de vista de desempenho, as técnicas de
**coreografia por eventos** e de **orquestração** em uma arquitetura de
microsserviços, usando um caso de comércio eletrônico (*e-commerce*).

### Desenho e variáveis

- **[E] Stack:** Spring Boot, Eureka (descoberta de serviços), MongoDB
  (persistência **isolada por serviço**) e ActiveMQ/JMS (mensageria), em
  **VMs Linux**.
- **[E] Cenário de falha:** falha provocada no **último serviço** do fluxo, com
  **compensação reversa** (rollback da saga).
- **[E] Tamanho da saga:** cenários com **2, 4, 6 e 8 serviços**.
- **[E] Repetições:** **5 execuções**.
- **[E] Medida principal:** tempo de execução **T2 − T1**, incluindo o
  *rollback* (compensação).
- **[E] Carga:** as razões reportadas referem-se a **três patamares relativos de
  carga** (**1× : 5× : 10×**), em razão de tempo de resposta — cf. p. 21.
- **[?] Não informados:** **versões** das ferramentas, **hardware**, taxa de
  requisições (**req/s**) e **dispersão** das medições **não** foram
  reportados.
- **[E] Escala horizontal:** **não** avaliada; múltiplas instâncias/réplicas
  foram explicitadas como **trabalho futuro**.
- **[R] Cuidado de leitura:** **não** apresentar estimativas visuais extraídas
  das **figuras** como se fossem dados.

### Resultados (somente o que o texto sustenta)

- **[E]** A **coreografia foi mais rápida** que a orquestração.
- **[E]** As razões **1:3,6:8,2** (coreografia) e **1:3,9:6,4** (orquestração)
  são **razões de tempo de resposta** para os patamares de carga
  **1× : 5× : 10×** (p. 21). **Não** são **latências absolutas** nem
  **comparação direta** entre as duas abordagens — são medidas normalizadas
  por patamar de carga, com **qualificação** de que a vantagem é
  **condicional ao patamar de carga**.
- **[E]** O valor **"~40×"** é um **relato aproximado** da **seção IV.B**, para
  **4 e 6 microsserviços** e **5 execuções**; **não** há **tabela**,
  **latências absolutas** nem indicação de se o valor se refere a **4**, a
  **6** ou à **média**.
- **[R]** Onde o texto não reporta tabela/número exato, **não inferir** por
  leitura visual das figuras.

### Limitações

- **[E]** Execução em **instância única**, **sem escala horizontal**; o próprio
  estudo indica escala horizontal como **trabalho futuro**.
- **[E]** **Não** reporta métricas de cauda (**p95/p99**) nem variabilidade
  estatística detalhada (**dispersão não informada**).
- **[E]** **Versões, hardware e taxa de requisições (req/s) não informados** →
  **reprodutibilidade limitada**.
- **[R]** As **figuras** não devem ser lidas por **estimativa visual** como se
  fossem dados.
- **[E]** Foco em **um** caso (*e-commerce*) com uma stack específica.

### Relação com o projeto

- **[E]** É a comparação empírica mais direta encontrada entre coreografia e
  orquestração em microsserviços, e inclui **compensação** (rollback) — insumo
  relevante para RQ2.
- **[E]** Cobre parcialmente **desempenho** (tempo/rapidez) → sobrepõe-se
  parcialmente a RQ1, porém **sem p95/p99** e sem escala horizontal.
- **[E]** **Não** cobre observabilidade quantificada nem custo total.

### Necessidade de obtenção via UFU

- **[E]** PDF de **acesso aberto** — não exige assinatura institucional.
- **[E]** Licença de reuso conferida e aprovada (resolvida conforme decisão do
  projeto).

---

## 2. Kristianto & Zahra (2021) — *Performance Analysis of Choreography and Orchestration in Microservices Architecture*

### Referência e acesso

- **Autores:** Handi Kristianto; Amalia Zahra (Bina Nusantara University,
  Jakarta). **[E]**
- **Título:** *Performance Analysis of Choreography and Orchestration in
  Microservices Architecture*. **[E]**
- **Veículo:** Journal of Theoretical and Applied Information Technology
  (JATIT), v. 99, n. 18, 30/09/2021, pp. 4220–4230. **[E]**
- **DOI:** **sem DOI identificado** (o periódico não atribui DOI por artigo; as
  ocorrências de `doi:` no PDF pertencem apenas à lista de referências). **[E]**
- **Cópia aberta (OA):**
  <https://www.jatit.org/volumes/Vol99No18/4Vol99No18.pdf> — **[E]** PDF de
  acesso aberto (11 páginas, pp. 4220–4230); **texto integral confirmado** no
  material disponibilizado.
- **Licença de reuso:** **[E]** conferida e aprovada (resolvida conforme decisão
  do projeto).

### Objetivo

**[E]** Analisar comparativamente o desempenho das abordagens de
**coreografia** e **orquestração** em microsserviços, incluindo o esforço de
mudança do fluxo.

### Desenho e variáveis

- **[E] Stack:** Spring Boot, MySQL, Kafka (mensageria), execução em
  **GCP/Kubernetes**; geração de carga com **JMeter**.
- **[E] Tamanho da saga:** cenários com **2, 4, 6 e 8 serviços**.
- **[E] Carga:** de **1 a 1000 usuários**.
- **[E] Instâncias:** **1 e 2 instâncias**.
- **[E] Variáveis dependentes:** tempo de resposta **médio**, *throughput*,
  uso de **CPU** e **esforço de mudança** (alteração de fluxo).
- **[?]** Versões exatas das ferramentas/serviços **não** estão detalhadas no
  material fichado.

### Resultados (somente o que o texto sustenta)

- **[E]** A **coreografia apresentou melhor desempenho** e **melhor uso de
  CPU** que a orquestração.
- **[E]** **Esforço de mudança de fluxo:** a coreografia exigiu alteração de
  **6 APIs em 4 serviços**, enquanto a orquestração exigiu **1 API em 1
  coordenador**.

### Limitações

- **[E]** **Não** reporta **p95/p99** (apenas tempo médio de resposta).
- **[E]** **Não** detalha **versões** do ambiente/ferramental.
- **[E]** **Sem DOI identificado** — a citação deve usar a URL do PDF (acesso
  aberto); não há identificador oficial a localizar.

### Relação com o projeto

- **[E]** Cobre **latência média, throughput, CPU e esforço de manutenção** →
  sobrepõe-se parcialmente a RQ1 e RQ4.
- **[E]** **Não** cobre **caudas (p95/p99)**, compensação/consistência sob
  falhas variadas, custo total nem observabilidade quantificada.
- **[E]** O achado de esforço de mudança (**6 APIs/4 serviços** vs.
  **1 API/1 coordenador**) é insumo quantitativo para RQ4 (manutenção).

### Necessidade de obtenção via UFU

- **[E]** PDF de **acesso aberto** — não exige assinatura institucional.
- **[E]** Licença de reuso conferida e aprovada (resolvida conforme decisão do
  projeto; não há DOI a localizar).

---

## 3. Aydin & Çebi (2022) — *Comparison of Choreography vs Orchestration Based Saga Patterns in Microservices*

### Referência e acesso

- **Autores:** Sahin Aydin (Department of Information Technologies, Işık
  University, Istanbul, Türkiye); Cem Berke Çebi (Department of Computer
  Engineering, Işık University). **[E]**
- **Título:** *Comparison of Choreography vs Orchestration Based Saga Patterns
  in Microservices*. **[E]**
- **Veículo:** 2022 International Conference on Electrical, Computer and Energy
  Technologies (ICECET), Prague, 20–22 July 2022, pp. 1–6. **[E]**
- **DOI:** `10.1109/ICECET55527.2022.9872665`. **[E]**
- **Cópia integral (local):** `research/sources/aydin-2022.pdf` — **[E]** PDF de
  **6 páginas**, obtido via assinatura institucional (IEEE Xplore:
  *"Authorized licensed use limited to: UNIVERSIDADE FEDERAL DE UBERLANDIA"*);
  **texto integral confirmado**.
- **Licença de reuso:** **[E]** IEEE ©2022; conferida e aprovada antes de
  reproduzir trechos/figuras (resolvida conforme decisão do projeto).

### Objetivo

- **[E]** Investigar a implementação dos métodos de **coreografia por eventos**
  e de **orquestração** para execução do padrão Saga em arquitetura de
  microsserviços (MSA).
- **[E]** Sugerir **qual metodologia de saga empregar em qual cenário**,
  analisando **desempenho e complexidade** com dados obtidos ao modelar uma
  variedade de casos de uso em um projeto sobre microsserviços Spring Boot.
- **[E]** Examinar aspectos como **redundância de dados, uso de recursos e
  requisitos de memória**; o estudo também aborda o problema de transações
  distribuídas/rollbacks em bancos No-SQL segregados (motivação declarada).

### Desenho e variáveis

- **[E] Stack/artefato:** um projeto de **microsserviços Spring Boot**
  ("spring boot-driven microservices"); o texto discute o contraste entre
  métodos de 2PC em RDBMS e bancos No-SQL segregados.
- **[E] Comparação conceitual:** a Tabela I do artigo contrapõe orquestração ×
  coreografia quanto a manutenibilidade, criação de dependências, ponto único
  de falha, visibilidade e esforço de coordenação.
- **[E] Dimensões de análise declaradas:** **tempo**, **memória** e **energia/
  poder de processamento** ("amount of time, memory, and power used").
- **[?]** Número de serviços, patamares de carga, **número de repetições**,
  ambiente de execução e **versões** de ferramentas **não** foram identificados
  de forma explícita no texto extraído.

### Resultados (somente o que o texto sustenta)

- **[E]** A **coreografia por eventos mostrou-se substancialmente mais rápida**
  em desempenho, com base no estudo de **tempo e memória**.
- **[E]** A **orquestração é mais lenta** ("sluggish"), mas **útil em cenários
  de transação complexos**.
- **[E]** Com **muitos eventos/acionadores**, a coreografia torna-se **muito
  difícil de criar e gerenciar** sem um orquestrador central; a orquestração é
  indicada quando há muitos participantes ou membros adicionados ao longo do
  tempo.
- **[E]** Recomendação: coreografia quando há **menos microsserviços** e o
  número de acionadores de eventos **não é complexo**; orquestração para
  **fluxos complexos**.
- **[?]** **Não** foram localizados no texto extraído **valores numéricos**
  (tabelas/gráficos de tempo, memória ou energia); os resultados são
  apresentados de forma **qualitativa/narrativa**. Nenhum número foi inferido.

### Limitações

- **[E]** Resultados **sem valores numéricos explícitos** no texto extraído;
  conclusões apresentadas de modo qualitativo.
- **[E]** O PDF contém trechos remanescentes do **template IEEE** (instruções de
  formatação não removidas), o que afeta a qualidade textual.
- **[E]** **Não** reporta **p95/p99** nem métricas específicas de
  **compensação/consistência**.
- **[?]** Sem detalhamento de ambiente/versões/repetições, a
  **reprodutibilidade** é limitada.

### Relação com o projeto

- **[E]** É uma das obras **diretamente** sobre **Saga** coreografada vs.
  orquestrada — comparativo central e **já fichado**.
- **[E]** Corrobora a expectativa **H1** (coreografia mais rápida no caminho
  simples) e aponta a orquestração para **cenários complexos** — convergente
  com Rudrabhatla (2018) e Kristianto & Zahra (2021).
- **[E]** **Não** cobre **caudas de latência (p95/p99)** nem uma **matriz de
  falhas/consistência** — reforça a lacuna defendida.

### Necessidade de obtenção via UFU

- **[E]** **Obtido** via assinatura institucional (IEEE Xplore / UFU) e
  registrado em `research/sources-manifest.md` (seção 7).
- **[E]** Licença de reuso do conteúdo IEEE conferida e aprovada (resolvida
  conforme decisão do projeto).

---

## 4. Marques (2024) — *Microservices orchestration vs. choreography: a comparison and analysis*

### Referência e acesso

- **Autor:** David Miguel Sousa Marques. **[R]**
- **Título:** *Microservices orchestration vs. choreography: a comparison and
  analysis*. **[E]**
- **Tipo/instituição:** dissertação de **mestrado em Engenharia Informática** —
  Instituto Superior de Engenharia do Porto (ISEP), Instituto Politécnico do
  Porto (P.PORTO), 2024. **[E]**
- **Handle:** <http://hdl.handle.net/10400.22/27107>. **[E]**
- **DOI:** **sem DOI identificado**. **[E]**
- **Cópia aberta (OA):**
  <https://recipp.ipp.pt/bitstreams/364a0a60-8064-4e40-9b5a-bdc2cb1ba1e2/download>
  — **[E]** PDF oficial de acesso aberto (bitstream do Handle
  10400.22/27107); **texto integral confirmado** no material disponibilizado.
- **Licença de reuso:** **[E]** conferida e aprovada (resolvida conforme decisão
  do projeto).

### Objetivo

**[E]** Comparar e analisar as abordagens de **orquestração** e **coreografia**
em microsserviços por meio de um **estudo misto** (experimento técnico +
levantamento com praticantes).

### Desenho e variáveis

- **[E] Tipo:** estudo **misto** (métodos quantitativo e qualitativo).
- **[E] Artefato:** duas versões de um sistema de *meal ordering* (uma
  coreografada, uma orquestrada); a **divisão observada nos listings** do
  trabalho separa as duas variantes.
- **[E] Cenários:** **5 cenários** cobrindo **sucesso, falhas e compensação**.
- **[E] Stack (sem versões):** **Quarkus**, **Spring**, **Camunda**
  (orquestração/BPMN), **Kafka** (mensageria), **MongoDB** (persistência), com
  **OpenAPI/Swagger** e **Lombok**. **[E]** As **versões** exatas **não** são
  informadas pelo trabalho.
- **[E] Levantamento:** estudo misto — **implementação** de referência +
  ***survey* com n = 14 profissionais**.
- **[E] Correção de leitura:** o **corpo do PDF** confirma o *survey* com
  **14 profissionais**; a menção a "entrevistas" consta apenas do **resumo** do
  repositório e **não** corresponde ao corpo — não deve ser usada.
- **[E] Aspectos comparados:** latência (simples), flexibilidade,
  escalabilidade, controle de erro/manutenção e ponto único de falha (SPOF).
- **[E] Não informados:** o **ambiente** de execução, a **carga**, os valores
  próprios de **latência/throughput/CPU** e a **URL/disponibilidade/licença do
  código** **não** são informados pelo trabalho.
- **[R] Cuidado:** **não** usar **Quarkus 1.11.2.Final** (versão citada em
  lista bibliográfica do trabalho) como **versão do projeto** fichado; e **não**
  atribuir a Marques os números de **JMeter / 1–1000 usuários**, que pertencem
  a **Kristianto & Zahra (2021)**.

### Resultados (somente o que o texto sustenta)

- **[E]** A **coreografia** foi considerada **flexível, escalável e com menor
  latência** no caso simples.
- **[E]** A **orquestração** ofereceu **melhor controle/tratamento de erro e
  manutenção**, mas com **maior latência** e **SPOF** (ponto único de falha).
- **[E]** Os resultados de latência **não são apresentados com valores
  numéricos** no material fichado.

### Limitações

- **[E]** **Latência sem valores** numéricos.
- **[E]** **Sem *throughput*** e **sem uso de CPU próprios**.
- **[E]** **Ambiente e carga não informados**.
- **[E]** *Survey* com **n = 14 profissionais** (amostra pequena; resultados
  qualitativos/perceptivos).
- **[E]** **Stack sem versões** informadas.
- **[E]** **URL/disponibilidade/licença do código não informados**.

### Relação com o projeto

- **[E]** É o estudo que **mais se aproxima** do escopo Saga-com-compensação
  (cenários de falha e compensação) e cobre dimensões de **manutenção e
  controle de erro** → insumo para RQ2, RQ3 e RQ4.
- **[E]** Porém **não quantifica** latência (média/cauda), throughput nem CPU.
- **[E]** Reforça a existência de **artefato/implementação comparativa** e de
  material OA — logo, **não alegar ineditismo** de "comparar" nem de "artefato
  aberto". **[E]** A **URL, a disponibilidade e a licença do código-fonte não
  são informadas** pelo trabalho.
- **[R]** A **política de versionamento/licença** do artefato *deste projeto* é
  **decisão interna de protocolo**, não pendência bibliográfica (ver seção de
  pendências).

### Necessidade de obtenção via UFU

- **[E]** PDF de **acesso aberto** — não exige assinatura institucional.
- **[E]** Licença de reuso da **obra** conferida e aprovada (resolvida conforme
  decisão do projeto), mantida **separada** da questão de código-fonte não
  informada.

---

## Síntese transversal

- **[E]** **A comparação coreografia × orquestração não é inédita.** Há pelo
  menos **quatro** estudos empíricos relevantes, todos com texto integral
  obtido: Rudrabhatla (2018), Kristianto & Zahra (2021), Marques (2024) e
  **Aydin & Çebi (2022)**. Portanto, **não alegar novidade da comparação em
  si**.
- **[E]** **Não alegar novidade de "artefato aberto" tampouco:** os estudos são
  de acesso aberto e ao menos um (Marques 2024) descreve duas implementações
  comparativas de um mesmo sistema. **[E]** A **URL/disponibilidade/licença do
  código não são informadas** em Marques (2024); a **política de
  versionamento/licença** do artefato deste projeto é **decisão interna de
  protocolo**, não pendência bibliográfica.
- **[E]** **Métricas já cobertas pelos estudos:** latência **média/tempo de
  resposta** (Rudrabhatla 2018; Kristianto & Zahra 2021), **throughput**
  (Kristianto & Zahra 2021), **CPU** (Kristianto & Zahra 2021) e **esforço de
  mudança de fluxo** (Kristianto & Zahra 2021).
- **[?] Lacunas identificadas (a defender como contribuição):**
  1. **Caudas de latência específicas de Saga (p95/p99):** nenhum texto
     integral obtido reporta p95/p99 para fluxos de saga com compensação.
  2. **Compensação/consistência sob falhas variadas:** os estudos tratam
     falha/rollback de forma pontual (Rudrabhatla: falha no último serviço;
     Marques: cenários de falha), mas **não** há, no material fichado,
     matriz ampla de injeção de falhas com verificação de consistência e
     idempotência.
  3. **Custo total (TCO):** não coberto por nenhum dos estudos fichados.
  4. **Observabilidade quantificada:** mencionada qualitativamente
     (controle/diagnóstico), mas **não operacionalizada** em métricas.
- **[E]** Da mesma forma que o fichamento de fundamentos, a lacuna defensável
  é **empírica e quantitativa**, com **controle de confundidores** e foco em
  **caudas, falhas/consistência, custo e observabilidade** — não a mera
  existência de uma comparação.

## Implicação para IC/TCC compartilhados (09/2026 – 06/2027)

- **[E]** No período **01/09/2026–30/06/2027**, IC e TCC **podem compartilhar o
  mesmo experimento** (o **mesmo artefato e a mesma pergunta-âncora**:
  coreografia × orquestração do padrão Saga), aproveitando os comparativos
  acima como **linha de base**.
- **[R]** Recorte adotado — as duas abordagens implementadas como **MVP**, com
  **RQ2** (falhas/consistência/compensação) como **eixo principal** e **RQ1**
  (latência/throughput/recursos, incluindo **p95/p99**) como **complementar** —
  justamente a lacuna dos comparativos.
- **[R]** **RQ3** (observabilidade quantificada) e **RQ4** (manutenção/evolução)
  são **extensões opcionais**, que podem ser aproveitadas no TCC, **sem**
  comprometer o MVP.
- **[E]** Contra reinvenção: os comparativos 1–4 já cobrem **média,
  throughput, CPU e esforço de mudança**; a contribuição deve **diferenciar-se
  pelas caudas, pela matriz de falhas/consistência e pela observabilidade
  quantificada**.
- **[E/R]** Antes de congelar as RQs: **Rudrabhatla (2018) reconferido** (as
  razões **1:3,6:8,2** e **1:3,9:6,4** são **razões de tempo de resposta** por
  patamar de carga **1× : 5× : 10×**, p. 21 — **não** latências absolutas). A
  **disponibilidade/licença do código** de Marques (2024) **não é informada** e
  passa a ser **decisão interna de protocolo**. Licenças de reuso dos itens
  **[E]** conferidas como **decisão interna do projeto** (não é parecer jurídico
  externo). (O texto integral de **Aydin & Çebi (2022)** já foi obtido e fichado;
  **Kristianto & Zahra (2021)** têm metadados conferidos e **sem DOI
  identificado**.)

---

## Pendências de acesso / verificação

- **[E]** Aydin & Çebi (2022): **texto integral obtido e fichado**
  (`research/sources/aydin-2022.pdf`); licença de reuso **[E]** resolvida
  conforme decisão do projeto.
- **[E]** Kristianto & Zahra (2021): dados conferidos (JATIT, v. 99, n. 18,
  pp. 4220–4230, 2021); **sem DOI identificado** (citar pela URL do PDF).
- **[E]** Rudrabhatla (2018): razões **1:3,6:8,2** (coreografia) e
  **1:3,9:6,4** (orquestração) **reconferidas** como **razões de tempo de
  resposta** para carga **1× : 5× : 10×** (p. 21); **"~40×"** é **relato
  aproximado** da seção IV.B (4 e 6 microsserviços, 5 execuções).
- **[E]** Marques (2024): **stack sem versões** informadas
  (Quarkus, Spring, Camunda, Kafka, MongoDB; OpenAPI/Swagger, Lombok);
  **ambiente, carga, latência/throughput/CPU próprios e
  URL/disponibilidade/licença do código não são informados**. Metodologia
  **confirmada** no corpo do PDF (estudo misto com implementação + *survey* de
  14 profissionais). *(Não* usar **Quarkus 1.11.2.Final** da bibliografia como
  versão do projeto; os números de **JMeter/1–1000 usuários** pertencem a
  **Kristianto & Zahra (2021)**.)
- **[R]** **Política de versionamento/licença do artefato:** **decisão interna
  de protocolo**, não pendência bibliográfica.
- **[E]** Licenças de reuso dos itens conferidas e aprovadas conforme **decisão
  interna do projeto** (não é parecer jurídico externo).
- **[R] microservices.io — orientação interna de uso:** conceitos e diagramas
  podem ser usados no trabalho acadêmico com **atribuição** a Chris
  Richardson/microservices.io, **preferência por paráfrase** e **diagramas
  adaptados**, com **citação/legenda adequada**; **não** é parecer jurídico nem
  licença externa verificada.
- **[?]** Conferência final da fonte primária e validação pelo(a)
  orientador(a) (fluxo de `AGENTS.md`, seções 7 e 8).

---

*Documento de trabalho — não oficial. Elaborado a partir do relatório de
pesquisa recebido, sem nova busca bibliográfica e sem invenção de dados.
Sujeito a revisão pelo orquestrador e pelo(a) orientador(a).*
