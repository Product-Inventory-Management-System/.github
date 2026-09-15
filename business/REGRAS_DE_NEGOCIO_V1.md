# Sistema de Gerenciamento de Estoque
## Regras de Negócio e Requisitos Funcionais

## 1. Objetivo do documento

Este documento descreve as regras de negócio e os requisitos funcionais iniciais do Sistema de Gerenciamento de Estoque.

Seu objetivo é transformar o contexto geral do projeto em comportamentos de negócio que possam posteriormente ser refinados, priorizados e convertidos em itens de backlog.

Esta especificação representa o entendimento atual do domínio e não deve ser considerada definitiva. O projeto possui escopo evolutivo e novas regras poderão surgir conforme os primeiros fluxos forem implementados e novas necessidades de negócio forem apresentadas.

A documentação deverá evoluir junto com o sistema.

---

# 2. Escopo inicial do domínio

O sistema será utilizado por uma empresa que possui um ou mais **almoxarifados** destinados ao armazenamento de componentes e materiais industriais.

Esses almoxarifados poderão armazenar materiais como:

- rolamentos;
- sensores;
- correias;
- parafusos;
- cabos;
- ferramentas;
- peças de reposição;
- outros componentes utilizados pela empresa.

O sistema deverá permitir que a empresa saiba quais materiais possui, onde eles estão armazenados e qual é a quantidade existente em cada almoxarifado.

O escopo inicial está concentrado em três conceitos fundamentais:

**Material**

Representa aquilo que fisicamente pode ser armazenado e posteriormente movimentado dentro do estoque.

**Almoxarifado**

Representa um local controlado no qual materiais são armazenados.

**Estoque**

Representa a existência de determinado material dentro de determinado almoxarifado e sua respectiva quantidade.

Neste primeiro momento, o objetivo não é implementar todos os processos possíveis de um sistema de estoque, mas estabelecer a base necessária para que movimentações e regras mais complexas possam surgir posteriormente.

---

# 3. Requisitos Funcionais

Requisitos funcionais descrevem **comportamentos ou capacidades que o sistema deve oferecer**.

## RF-001 — Cadastrar material

O sistema deverá permitir o cadastro de materiais que possam ser armazenados nos almoxarifados da empresa.

O material deverá possuir informações suficientes para permitir sua identificação dentro do estoque.

Detalhes adicionais sobre classificação, categoria, unidade de medida, fabricante ou outras características deverão ser definidos apenas quando houver uma necessidade de negócio que os justifique.

---

## RF-002 — Consultar materiais

O sistema deverá permitir consultar os materiais conhecidos pelo sistema.

A consulta deverá possibilitar identificar o material que será utilizado em operações relacionadas ao estoque.

Filtros e formas avançadas de pesquisa deverão ser introduzidos conforme surgirem necessidades concretas.

---

## RF-003 — Manter almoxarifados

O sistema deverá permitir representar os almoxarifados utilizados pela empresa.

Cada almoxarifado deverá possuir identificação suficiente para diferenciá-lo dos demais locais de armazenamento.

---

## RF-004 — Consultar almoxarifados

O sistema deverá permitir consultar os almoxarifados existentes.

Essa consulta deverá permitir identificar em quais locais a empresa pode armazenar materiais.

---

## RF-005 — Registrar existência de material em um almoxarifado

O sistema deverá permitir associar um material a um almoxarifado.

Essa associação representa que determinado material pode possuir estoque naquele local.

---

## RF-006 — Consultar saldo de estoque

O sistema deverá permitir consultar a quantidade existente de determinado material em determinado almoxarifado.

A consulta deverá permitir responder, no mínimo:

- qual material está sendo consultado;
- em qual almoxarifado ele está armazenado;
- qual é a quantidade registrada naquele local.

---

## RF-007 — Registrar entrada de material

O sistema deverá permitir registrar a entrada de uma quantidade de material em um almoxarifado.

A operação deverá produzir aumento correspondente no estoque daquele material no local informado.

O motivo, origem e demais informações relacionadas ao recebimento poderão ser refinados conforme o processo de entrada for aprofundado.

---

## RF-008 — Registrar saída de material

O sistema deverá permitir registrar a retirada de determinada quantidade de material de um almoxarifado.

A operação deverá reduzir o estoque daquele material no local informado.

A saída deverá respeitar as regras de disponibilidade definidas neste documento.

---

## RF-009 — Consultar movimentações de estoque

O sistema deverá permitir consultar movimentações que tenham alterado a quantidade armazenada de um material.

Inicialmente deverão ser consideradas como movimentações:

- entradas;
- saídas.

Novos tipos de movimentação poderão ser introduzidos posteriormente.

---

## RF-010 — Consultar estoque por almoxarifado

O sistema deverá permitir visualizar os materiais existentes em determinado almoxarifado e suas respectivas quantidades.

---

## RF-011 — Consultar estoque de um material

O sistema deverá permitir consultar em quais almoxarifados determinado material possui estoque e qual é sua quantidade em cada local.

---

# 4. Regras de Negócio

Regras de negócio representam **restrições, condições e invariantes que devem ser respeitadas independentemente da interface, API, banco de dados ou tecnologia utilizada**.

## RN-001 — Material deve ser identificado de forma única

Cada material controlado pelo sistema deverá possuir uma identificação que permita distingui-lo dos demais materiais.

Dois registros não deverão representar acidentalmente o mesmo material físico dentro do domínio.

A estratégia utilizada para essa identificação será definida durante a modelagem.

---

## RN-002 — Almoxarifado deve ser identificado de forma única

Cada almoxarifado deverá possuir uma identificação que permita distingui-lo dos demais locais de armazenamento da empresa.

---

## RN-003 — O saldo pertence à combinação de material e almoxarifado

A quantidade em estoque não deverá ser tratada como uma propriedade global do material.

Um mesmo material poderá possuir quantidades diferentes em diferentes almoxarifados.

Portanto, o saldo deverá sempre estar relacionado simultaneamente a:

**Material + Almoxarifado**

Exemplo:

Material: Rolamento XPTO

Almoxarifado A: 30 unidades  
Almoxarifado B: 12 unidades

O estoque total da empresa poderá ser calculado posteriormente, mas os saldos físicos deverão permanecer associados aos respectivos locais.

---

## RN-004 — Uma entrada aumenta o saldo do estoque

Quando uma entrada de material for concluída, a quantidade registrada deverá ser acrescentada ao saldo atual daquele material no almoxarifado informado.

Exemplo:

Saldo atual: 10 unidades  
Entrada: 5 unidades  
Novo saldo: 15 unidades

---

## RN-005 — Uma saída reduz o saldo do estoque

Quando uma saída de material for concluída, a quantidade retirada deverá ser descontada do saldo existente daquele material no almoxarifado informado.

Exemplo:

Saldo atual: 15 unidades  
Saída: 4 unidades  
Novo saldo: 11 unidades

---

## RN-006 — Movimentações devem possuir quantidade positiva

Operações de entrada ou saída deverão informar uma quantidade maior que zero.

Valores negativos não deverão ser utilizados para inverter semanticamente uma operação.

Portanto:

- entrada de `-10` unidades não representa uma saída;
- saída de `-10` unidades não representa uma entrada.

Cada operação deverá possuir seu significado explícito dentro do domínio.

---

## RN-007 — O estoque não poderá possuir saldo negativo

Uma operação não poderá resultar em quantidade física negativa para determinado material em determinado almoxarifado.

Se houver 10 unidades disponíveis, uma saída de 11 unidades não deverá ser concluída.

A forma técnica utilizada para garantir essa regra será definida posteriormente.

---

## RN-008 — Uma movimentação deve indicar o material afetado

Toda entrada ou saída deverá estar relacionada a um material conhecido pelo sistema.

Não deverá existir movimentação de estoque sem identificação do material movimentado.

---

## RN-009 — Uma movimentação deve indicar o almoxarifado afetado

Toda entrada ou saída deverá indicar em qual almoxarifado a alteração física ocorreu.

Não deverá existir movimentação sem definição do local de estoque afetado.

---

## RN-010 — Movimentações concluídas devem ser rastreáveis

Alterações de saldo deverão possuir registro suficiente para permitir identificar posteriormente que uma movimentação ocorreu.

A consulta do saldo atual, isoladamente, não deverá ser a única evidência das alterações realizadas anteriormente.

A profundidade do histórico e os dados necessários para auditoria deverão evoluir conforme novas necessidades forem introduzidas.

---

## RN-011 — O saldo deverá refletir movimentações válidas

Somente operações de negócio consideradas válidas e concluídas poderão produzir alteração definitiva no saldo de estoque.

Uma solicitação inválida ou rejeitada não deverá alterar a quantidade armazenada.

---

# 5. Regras ainda não definidas

Os seguintes assuntos pertencem ao domínio previsto do projeto, mas **não possuem regras suficientemente definidas para fazer parte do escopo funcional inicial**:

- reserva de materiais;
- expiração de reservas;
- cancelamento de reservas;
- solicitação de materiais;
- transferência entre almoxarifados;
- separação de materiais;
- despacho;
- transporte;
- recebimento de transferências;
- ajustes manuais de estoque;
- aprovação de movimentações;
- inventário;
- limites mínimos ou máximos de estoque;
- notificações;
- integrações externas;
- processamento assíncrono;
- tratamento de falhas e reprocessamento.

Essas capacidades não estão descartadas.

Elas deverão ser introduzidas progressivamente quando uma nova necessidade de negócio justificar sua existência.

---

# 6. Fora do escopo inicial

Neste estágio, não deverá ser assumido como requisito de negócio:

- arquitetura de microserviços;
- utilização de Apache Kafka;
- quantidade de tópicos ou partições;
- utilização de máquina de estados;
- estratégia de locking;
- estratégia de idempotência;
- Transactional Outbox;
- retry ou Dead Letter Queue;
- banco de dados específico;
- infraestrutura AWS;
- quantidade de aplicações ou repositórios.

Esses elementos são decisões técnicas ou arquiteturais e deverão ser introduzidos somente quando um requisito ou problema do domínio justificar sua utilização.

---

# 7. Relação entre requisito funcional e regra de negócio

Os requisitos funcionais descrevem aquilo que o sistema permite realizar.

As regras de negócio determinam as condições sob as quais essas funcionalidades podem acontecer.

Por exemplo:

**RF-008 — Registrar saída de material**

O sistema permite solicitar a retirada de determinada quantidade de um material.

Essa funcionalidade é limitada por:

**RN-006 — Movimentações devem possuir quantidade positiva**

e

**RN-007 — O estoque não poderá possuir saldo negativo**

Portanto, possuir uma funcionalidade de saída não significa que qualquer saída solicitada deverá ser aceita.

Essa separação deverá ser preservada ao longo da evolução da documentação.

---

# 8. Utilização deste documento para construção do backlog

Este documento será uma das fontes utilizadas para criação e refinamento do backlog do projeto.

Entretanto, um requisito funcional não deverá ser automaticamente convertido em uma única tarefa técnica.

Um requisito poderá originar:

- uma ou mais histórias;
- critérios de aceite;
- decisões de modelagem;
- experimentos técnicos;
- tarefas de infraestrutura;
- testes;
- documentação;
- investigações arquiteturais.

Da mesma forma, uma regra de negócio poderá afetar vários requisitos simultaneamente.

Exemplo:

A regra que impede saldo negativo poderá futuramente exigir análise específica de concorrência quando duas requisições de saída ocorrerem simultaneamente.

Nesse momento, o problema deverá primeiro ser observado e compreendido para depois justificar a estratégia técnica utilizada para resolvê-lo.

---

# 9. Critério para evolução da especificação

Uma nova regra ou requisito deverá ser acrescentado quando existir uma necessidade de negócio suficientemente compreendida.

A evolução deverá preferencialmente responder às seguintes perguntas:

1. Qual nova necessidade apareceu?
2. Como o sistema funciona atualmente?
3. O que a solução atual não consegue representar ou garantir?
4. Qual novo comportamento o sistema precisa oferecer?
5. Existe alguma nova restrição de negócio?
6. Quais requisitos existentes são afetados?

Somente depois desse entendimento deverão ser discutidas as alterações arquiteturais e técnicas necessárias.

---

# 10. Estado atual da especificação

Esta versão estabelece somente o núcleo inicial do domínio:

**Material → Almoxarifado → Estoque → Entrada/Saída → Histórico**

Esse núcleo deverá servir como ponto inicial para implementação e descoberta de novas necessidades.

Capacidades mais complexas serão incorporadas quando o próprio domínio criar problemas que exijam sua existência.
