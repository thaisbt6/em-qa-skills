# Plano de Testes – [TASK-71590] – Emissão de Carteirinhas

## 1. Informações Gerais
- **Task:** TASK-71590
- **Tipo:** Melhoria
- **Data:** 2026-08-22
- **Caminho:** Acadêmico => Aluno e Matrícula => Matrícula => Emissões => Emissão de carteirinhas
- **Handler:** frederico.sebba
- **PR:** https://github.com/escolarmanager/em/pull/26077
- **Ambiente de homologação:** http://srv-hmg-interno/prd-M71590/colvitoria

## 2. Objetivo
Melhorar a usabilidade da tela de **Emissão de Carteirinhas**, eliminando a necessidade de uma tela auxiliar de consulta/seleção (botão "Carregar"). A tela passa a:
- Remover o botão **Carregar**, mantendo apenas o botão **Emitir**, visível desde o início.
- Carregar automaticamente **Professores** e **Outros Profissionais** ao acessar a tela, com o filtro **Somente ativo = Sim** como padrão, permitindo alternar para **Não** (recarregando a Grid).
- Carregar **Alunos** na Grid somente após o preenchimento dos filtros obrigatórios (período letivo, curso e turma), limpando a Grid quando a turma é apagada.
- Disponibilizar uma barra de pesquisa em cada Grid, filtrando apenas os registros já carregados (sem abrir nova tela).
- Permitir a seleção direta dos registros na própria Grid, considerando na emissão somente os selecionados.
- A tela sempre abre no tipo de emissão **Alunos**, com os campos limpos (não recupera filtros de emissões anteriores).
- Como o impacto foi classificado como **núcleo**, o mesmo ajuste de comportamento da modal de processamento (exibida somente quando a emissão é aceita, não aparecendo quando a validação recusa) foi replicado em outras telas de emissão do sistema, que também precisam ser regressadas.

## 3. Pontos em Aberto
- Não foi encontrado na issue o texto exato das mensagens de validação (ex.: bloqueio de emissão sem seleção de registros; bloqueio de carregamento de alunos sem filtros obrigatórios). Os cenários abaixo validam o comportamento; o texto exato da mensagem deve ser confirmado/ajustado durante a execução com o time de desenvolvimento.
- Não há informação sobre paginação ou limite de registros nas Grids de Professores/Outros Profissionais, o que pode impactar a performance da pesquisa em bases com grande volume.
- Não foi documentado se existem restrições de perfil/permissão específicas para acessar a Emissão de Carteirinhas; portanto, cenários de permissão não foram detalhados nesta rodada.
- A nota de desenvolvimento cita que a alteração da modal de processamento (exibir somente quando a emissão é aceita) foi replicada nas demais telas de emissão listadas na Matriz de Cobertura. Esta homologação cobre apenas essa regra pontual em cada tela listada, não o fluxo completo de cada emissão (que deve ser garantido pelos planos de teste específicos de cada funcionalidade).

## 4. Matriz de Cobertura

| Categoria | Qtd. de Cenários |
| :--- | :---: |
| Caminho feliz | 5 |
| Fluxos alternativos | 4 |
| Validação de campos | 3 |
| Mensagens de erro/alerta | 2 |
| Regras de negócio | 4 |
| Permissões/Perfis de acesso | 0 |
| Integração | 0 |
| Regressão | 9 |
| Casos de borda/limite | 3 |
| Usabilidade/Acessibilidade | 2 |
| **Total** | **32** |

## 5. Cenários de Teste (Gherkin)

### TASK-71590-CT-001 – Validar carregamento automático de professores ativos ao acessar a tela
**Tipo:** Funcional | **Prioridade:** Alta

```gherkin
Cenário: Carregamento automático de professores ativos
  Given que acesso a tela "Acadêmico => Aluno e Matrícula => Matrícula => Emissões => Emissão de carteirinhas"
  And seleciono o tipo de emissão "Professores"
  When a Grid for exibida
  Then o sistema deve apresentar automaticamente na Grid os professores com situação ativa
  And o filtro "Somente ativo" deve estar selecionado como "Sim" por padrão
  And o botão "Carregar" não deve estar presente na tela
```

### TASK-71590-CT-002 – Validar carregamento automático de outros profissionais ativos ao acessar a tela
**Tipo:** Funcional | **Prioridade:** Alta

```gherkin
Cenário: Carregamento automático de outros profissionais ativos
  Given que acesso a tela "Acadêmico => Aluno e Matrícula => Matrícula => Emissões => Emissão de carteirinhas"
  And seleciono o tipo de emissão "Outros Profissionais"
  When a Grid for exibida
  Then o sistema deve apresentar automaticamente na Grid os outros profissionais com situação ativa
  And o filtro "Somente ativo" deve estar selecionado como "Sim" por padrão
  And o botão "Carregar" não deve estar presente na tela
```

### TASK-71590-CT-003 – Validar carregamento de alunos na Grid após preenchimento dos filtros obrigatórios
**Tipo:** Funcional | **Prioridade:** Alta

```gherkin
Cenário: Carregamento de alunos após preenchimento dos filtros
  Given que acesso a tela "Acadêmico => Aluno e Matrícula => Matrícula => Emissões => Emissão de carteirinhas"
  And seleciono o tipo de emissão "Alunos"
  When preencho os filtros "Período letivo", "Curso" e "Turma"
  Then o sistema deve carregar automaticamente na Grid os alunos correspondentes aos filtros informados
```

### TASK-71590-CT-004 – Validar emissão de carteirinhas para professores selecionados na Grid
**Tipo:** Funcional | **Prioridade:** Alta

```gherkin
Cenário: Emissão de carteirinhas de professores selecionados
  Given que a Grid de "Professores" está carregada na tela de Emissão de Carteirinhas
  And seleciono um ou mais professores na Grid
  When clico no botão "Emitir"
  Then o sistema deve emitir as carteirinhas somente para os professores selecionados
```

### TASK-71590-CT-005 – Validar emissão de carteirinhas para alunos selecionados na Grid
**Tipo:** Funcional | **Prioridade:** Alta

```gherkin
Cenário: Emissão de carteirinhas de alunos selecionados
  Given que a Grid de "Alunos" está carregada com os registros correspondentes aos filtros informados
  And seleciono um ou mais alunos na Grid
  When clico no botão "Emitir"
  Then o sistema deve emitir as carteirinhas somente para os alunos selecionados
```

### TASK-71590-CT-006 – Validar alternância do filtro "Somente ativo" para "Não" na consulta de professores
**Tipo:** Funcional | **Prioridade:** Média

```gherkin
Cenário: Alteração do filtro de situação para professores
  Given que a Grid de "Professores" está carregada com o filtro "Somente ativo" igual a "Sim"
  When altero o filtro "Somente ativo" para "Não"
  Then a Grid deve ser recarregada
  And deve apresentar todos os professores, ativos e não ativos
```

### TASK-71590-CT-007 – Validar alternância do filtro "Somente ativo" para "Não" na consulta de outros profissionais
**Tipo:** Funcional | **Prioridade:** Média

```gherkin
Cenário: Alteração do filtro de situação para outros profissionais
  Given que a Grid de "Outros Profissionais" está carregada com o filtro "Somente ativo" igual a "Sim"
  When altero o filtro "Somente ativo" para "Não"
  Then a Grid deve ser recarregada
  And deve apresentar todos os outros profissionais, ativos e não ativos
```

### TASK-71590-CT-008 – Validar pesquisa de professor pela barra de pesquisa entre os registros carregados
**Tipo:** Funcional | **Prioridade:** Média

```gherkin
Cenário: Pesquisa de professor na Grid já carregada
  Given que a Grid de "Professores" está carregada com múltiplos registros
  When digito na barra de pesquisa o nome de um professor presente na Grid
  Then o sistema deve filtrar e exibir somente os registros correspondentes à pesquisa
  And nenhuma nova tela deve ser aberta durante a pesquisa
```

### TASK-71590-CT-009 – Validar pesquisa de aluno pela barra de pesquisa entre os registros carregados
**Tipo:** Funcional | **Prioridade:** Média

```gherkin
Cenário: Pesquisa de aluno na Grid já carregada
  Given que a Grid de "Alunos" está carregada com múltiplos registros após o preenchimento dos filtros
  When digito na barra de pesquisa o nome de um aluno presente na Grid
  Then o sistema deve filtrar e exibir somente os registros correspondentes à pesquisa
  And nenhuma nova tela deve ser aberta durante a pesquisa
```

### TASK-71590-CT-010 – Validar bloqueio de carregamento de alunos sem preenchimento dos filtros obrigatórios
**Tipo:** Negativo | **Prioridade:** Alta

```gherkin
Cenário: Tentativa de carregar alunos sem preencher os filtros obrigatórios
  Given que acesso a tela de Emissão de Carteirinhas com o tipo de emissão "Alunos"
  When deixo de preencher um dos filtros obrigatórios ("Período letivo", "Curso" ou "Turma")
  Then a Grid de alunos não deve ser carregada
  And o sistema deve indicar que o preenchimento dos filtros é necessário
```

### TASK-71590-CT-011 – Validar limpeza da Grid de alunos ao remover o filtro de turma
**Tipo:** Funcional | **Prioridade:** Média

```gherkin
Cenário: Limpeza da Grid ao apagar o filtro de turma
  Given que a Grid de "Alunos" está carregada com registros após o preenchimento de período letivo, curso e turma
  When apago o campo "Turma"
  Then a Grid de alunos deve ser limpa
```

### TASK-71590-CT-012 – Validar seleção múltipla de registros na Grid antes da emissão
**Tipo:** Funcional | **Prioridade:** Alta

```gherkin
Cenário: Seleção de múltiplos registros na Grid
  Given que a Grid de "Professores" está carregada com múltiplos registros
  When seleciono mais de um professor na Grid
  Then todos os registros selecionados devem permanecer marcados
  And o botão "Emitir" deve permanecer disponível para acionamento
```

### TASK-71590-CT-013 – Validar mensagem ao tentar emitir carteirinhas sem selecionar nenhum registro
**Tipo:** Negativo | **Prioridade:** Alta

```gherkin
Cenário: Tentativa de emissão sem seleção de registros
  Given que a Grid de "Professores" está carregada
  And nenhum registro está selecionado na Grid
  When clico no botão "Emitir"
  Then o sistema não deve permitir a emissão
  And deve exibir uma mensagem informando que é necessário selecionar ao menos um registro
```

### TASK-71590-CT-014 – Validar comportamento ao pesquisar termo sem correspondência na Grid
**Tipo:** Borda | **Prioridade:** Média

```gherkin
Cenário: Pesquisa sem resultados correspondentes
  Given que a Grid de "Outros Profissionais" está carregada com registros
  When digito na barra de pesquisa um termo que não corresponde a nenhum registro carregado
  Then a Grid deve ser exibida sem registros
  And o sistema não deve exibir erro, apenas indicar a ausência de resultados
```

### TASK-71590-CT-015 – Validar remoção do botão "Carregar" e exibição fixa do botão "Emitir"
**Tipo:** Regressão | **Prioridade:** Alta

```gherkin
Cenário: Barra de ferramentas da tela de Emissão de Carteirinhas
  Given que acesso a tela de Emissão de Carteirinhas
  When a tela for exibida, independentemente do tipo de emissão selecionado
  Then o botão "Carregar" não deve estar presente
  And o botão "Emitir" deve estar visível desde o início
```

### TASK-71590-CT-016 – Validar abertura da tela sempre no tipo de emissão "Alunos" com campos limpos
**Tipo:** Regressão | **Prioridade:** Média

```gherkin
Cenário: Estado inicial da tela ao reabrir a Emissão de Carteirinhas
  Given que realizei uma emissão de carteirinhas para o tipo "Professores" com o filtro "Somente ativo" igual a "Não"
  When saio da tela e acesso novamente a Emissão de Carteirinhas
  Then a tela deve abrir no tipo de emissão "Alunos"
  And os campos de filtro devem estar limpos, sem recuperar os valores da emissão anterior
```

### TASK-71590-CT-017 – Validar que a emissão considera somente os registros selecionados na Grid da própria tela
**Tipo:** Funcional | **Prioridade:** Alta

```gherkin
Cenário: Emissão restrita aos registros selecionados na Grid atual
  Given que a Grid de "Alunos" está carregada com 10 registros
  And seleciono apenas 2 desses registros
  When clico no botão "Emitir"
  Then o sistema deve emitir as carteirinhas apenas para os 2 registros selecionados
  And os demais 8 registros não selecionados não devem ser incluídos na emissão
```

### TASK-71590-CT-018 – Validar que a pesquisa é aplicada somente sobre os registros já carregados na Grid
**Tipo:** Regressão | **Prioridade:** Média

```gherkin
Cenário: Pesquisa restrita aos registros já carregados
  Given que a Grid de "Alunos" está carregada com os registros de uma turma específica
  When pesquiso pelo nome de um aluno que pertence a outra turma não carregada
  Then o sistema não deve retornar esse aluno na pesquisa
  And nenhuma nova tela ou nova consulta ao servidor deve ser aberta para localizá-lo
```

### TASK-71590-CT-019 – Validar exibição da modal de processamento apenas quando a Emissão de boletos e carnês é aceita
**Tipo:** Regressão | **Prioridade:** Alta

```gherkin
Cenário: Modal de processamento na Emissão de boletos e carnês
  Given que acesso a tela "Financeiro => Contas a receber => Boleto => Emissões => Emissão de boletos e carnês"
  When realizo uma emissão que é recusada pela validação
  Then a modal de processamento não deve ser exibida
  When realizo uma emissão que é aceita pela validação
  Then a modal de processamento deve ser exibida
```

### TASK-71590-CT-020 – Validar exibição da modal de processamento apenas quando a Emissão de avisos e cartas de cobrança é aceita
**Tipo:** Regressão | **Prioridade:** Alta

```gherkin
Cenário: Modal de processamento na Emissão de avisos e cartas de cobrança
  Given que acesso a tela "Financeiro => Contas a receber => Duplicata => Emissões => Emissão de avisos e cartas de cobrança"
  When realizo uma emissão que é recusada pela validação
  Then a modal de processamento não deve ser exibida
  When realizo uma emissão que é aceita pela validação
  Then a modal de processamento deve ser exibida
```

### TASK-71590-CT-021 – Validar exibição da modal de processamento apenas quando a Emissão de ofício de estágio é aceita
**Tipo:** Regressão | **Prioridade:** Alta

```gherkin
Cenário: Modal de processamento na Emissão de ofício de estágio
  Given que acesso a tela "Acadêmico => Aluno e matrícula => Estágio => Emissões => Emissão de ofício de estágio"
  When realizo uma emissão que é recusada pela validação
  Then a modal de processamento não deve ser exibida
  When realizo uma emissão que é aceita pela validação
  Then a modal de processamento deve ser exibida
```

### TASK-71590-CT-022 – Validar exibição da modal de processamento apenas quando a Emissão de fechamento de caixa é aceita
**Tipo:** Regressão | **Prioridade:** Alta

```gherkin
Cenário: Modal de processamento na Emissão de fechamento de caixa
  Given que acesso a tela "Financeiro => Caixa => Emissões => Emissão de fechamento de caixa"
  When realizo uma emissão que é recusada pela validação
  Then a modal de processamento não deve ser exibida
  When realizo uma emissão que é aceita pela validação
  Then a modal de processamento deve ser exibida
```

### TASK-71590-CT-023 – Validar exibição da modal de processamento apenas quando a Impressão de NFS-e geradas é aceita
**Tipo:** Regressão | **Prioridade:** Alta

```gherkin
Cenário: Modal de processamento na Impressão de NFS-e geradas
  Given que acesso a tela "Financeiro => Nota fiscal => Nota Fiscal de Serviço (NFS-e - Município) => Emissões => Impressão de NFS-e geradas"
  When realizo uma emissão que é recusada pela validação
  Then a modal de processamento não deve ser exibida
  When realizo uma emissão que é aceita pela validação
  Then a modal de processamento deve ser exibida
```

### TASK-71590-CT-024 – Validar exibição da modal de processamento apenas quando a Impressão de NF-e modelo 55 geradas é aceita
**Tipo:** Regressão | **Prioridade:** Alta

```gherkin
Cenário: Modal de processamento na Impressão de NF-e modelo 55 geradas
  Given que acesso a tela "Financeiro => Nota fiscal => Nota fiscal eletrônica (NF-e modelo 55 - SEFAZ) => Emissões => Impressão de NF-e modelo 55 geradas"
  When realizo uma emissão que é recusada pela validação
  Then a modal de processamento não deve ser exibida
  When realizo uma emissão que é aceita pela validação
  Then a modal de processamento deve ser exibida
```

### TASK-71590-CT-025 – Validar exibição da modal de processamento apenas quando a Impressão de NFC-e geradas é aceita
**Tipo:** Regressão | **Prioridade:** Alta

```gherkin
Cenário: Modal de processamento na Impressão de NFC-e geradas
  Given que acesso a tela "Financeiro => Nota fiscal => Cupom fiscal (NFC-e - SEFAZ) => Impressão de NFC-e geradas"
  When realizo uma emissão que é recusada pela validação
  Then a modal de processamento não deve ser exibida
  When realizo uma emissão que é aceita pela validação
  Then a modal de processamento deve ser exibida
```

### TASK-71590-CT-026 – Validar exibição da modal de processamento apenas quando a Emissão de consultas específicas é aceita
**Tipo:** Regressão | **Prioridade:** Média

```gherkin
Cenário: Modal de processamento na Emissão de consultas específicas
  Given que acesso a tela "Outros => Consulta específica => Emissão de consultas específicas"
  When realizo uma emissão que é recusada pela validação
  Then a modal de processamento não deve ser exibida
  When realizo uma emissão que é aceita pela validação
  Then a modal de processamento deve ser exibida
```

### TASK-71590-CT-027 – Validar exibição da modal de processamento apenas quando a Emissão de recibos da biblioteca é aceita
**Tipo:** Regressão | **Prioridade:** Média

```gherkin
Cenário: Modal de processamento na Emissão de recibos da biblioteca
  Given que acesso a tela "Biblioteca => Emissões => Recibos"
  When realizo uma emissão que é recusada pela validação
  Then a modal de processamento não deve ser exibida
  When realizo uma emissão que é aceita pela validação
  Then a modal de processamento deve ser exibida
```

### TASK-71590-CT-028 – Validar comportamento da Grid quando não existem professores ativos cadastrados
**Tipo:** Borda | **Prioridade:** Baixa

```gherkin
Cenário: Grid de professores sem registros ativos
  Given que não existem professores com situação ativa cadastrados na base
  When acesso a tela de Emissão de Carteirinhas com o tipo de emissão "Professores"
  Then a Grid deve ser exibida vazia
  And o sistema não deve apresentar erro ao carregar a tela
```

### TASK-71590-CT-029 – Validar comportamento da Grid quando não existem alunos correspondentes aos filtros informados
**Tipo:** Borda | **Prioridade:** Baixa

```gherkin
Cenário: Grid de alunos sem registros correspondentes aos filtros
  Given que acesso a tela de Emissão de Carteirinhas com o tipo de emissão "Alunos"
  When preencho período letivo, curso e turma que não possuem alunos matriculados
  Then a Grid deve ser exibida vazia
  And o sistema não deve apresentar erro ao aplicar os filtros
```

### TASK-71590-CT-030 – Validar pesquisa com caracteres especiais e acentuação na barra de pesquisa
**Tipo:** Borda | **Prioridade:** Baixa

```gherkin
Cenário: Pesquisa com acentuação na Grid de alunos
  Given que a Grid de "Alunos" está carregada com registros que possuem nomes acentuados
  When pesquiso utilizando o nome com e sem acentuação
  Then o sistema deve localizar corretamente o registro correspondente em ambos os casos
```

### TASK-71590-CT-031 – Validar exibição e usabilidade da barra de pesquisa na Grid
**Tipo:** Usabilidade | **Prioridade:** Média

```gherkin
Cenário: Usabilidade da barra de pesquisa
  Given que acesso a tela de Emissão de Carteirinhas com qualquer tipo de emissão selecionado
  When a Grid de registros for exibida
  Then a barra de pesquisa deve estar visível e de fácil identificação próxima à Grid
  And deve ser possível limpar a pesquisa e retornar à listagem completa dos registros carregados
```

### TASK-71590-CT-032 – Validar que o botão "Emitir" permanece visível e acessível desde o início da tela
**Tipo:** Usabilidade | **Prioridade:** Média

```gherkin
Cenário: Visibilidade do botão Emitir
  Given que acesso a tela de Emissão de Carteirinhas
  When a tela for carregada, antes mesmo de qualquer seleção de registro
  Then o botão "Emitir" deve estar visível na barra de ferramentas
  And seu estado (habilitado/desabilitado) deve refletir claramente se há ou não registros selecionados na Grid
```
