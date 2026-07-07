# Plano de Testes – [TASK-70432] – Favoritar cores no seletor de cores dos Grupos de Observações do Calendário Escolar

## 1. Informações Gerais
- **Task:** TASK-70432
- **Tipo:** Melhoria
- **Data:** 2026-07-06

## 2. Objetivo
Esta melhoria visa permitir que os usuários (coordenador(a), secretária escolar ou administrador) favoritem cores no seletor de cores (ColorPicker) dos Grupos de Observações do Calendário Escolar. O objetivo principal é agilizar o cadastro de novos grupos, mantendo a padronização das tonalidades de cores utilizadas na instituição e eliminando a necessidade de anotações externas de códigos hexadecimais de cores. As cores favoritas devem ser salvas de forma individual por usuário.

## 3. Pontos em Aberto
- **Ação "Acadêmico => Organizar favoritos => Nova pasta":** O desenvolvedor indicou no questionário de melhoria que esta funcionalidade deve ser homologada. Contudo, na descrição da funcionalidade do Mantis, não há menção à criação de pastas para favoritos de cores. É necessário confirmar se este recurso de "Organizar favoritos" possui alguma relação com a melhoria do seletor de cores ou se faz parte de outra modificação enviada sob o mesmo Pull Request.
- **Forma de Armazenamento:** É necessário confirmar se as cores favoritas estão sendo gravadas no banco de dados (persistindo entre dispositivos/navegadores do mesmo usuário) ou apenas no Local Storage do navegador atual.

## 4. Matriz de Cobertura

| Categoria | Qtd. de Cenários |
| :--- | :---: |
| Caminho feliz | 2 |
| Fluxos alternativos | 1 |
| Validação de campos | 1 |
| Mensagens de erro/alerta | 1 |
| Regras de negócio | 1 |
| Permissões/Perfis de acesso | 1 |
| Integração | 2 |
| Regressão | 1 |
| Casos de borda/limite | 2 |
| Usabilidade/Acessibilidade | 0 |
| **Total** | **12** |

## 5. Cenários de Teste (Gherkin)

### TASK-70432-CT-001 – Validar favoritação de uma cor pelo seletor
**Tipo:** Funcional | **Prioridade:** Alta

```gherkin
Cenário: Adicionar cor aos favoritos com sucesso
  Given que acesso a tela de Calendário Escolar no módulo "Acadêmico => Calendário e horário => Calendário escolar"
  And clico em cadastrar ou editar um "Grupo de Observações"
  And visualizo o seletor de cores (ColorPicker)
  When seleciono uma cor que não está favoritada
  And clico no ícone de estrela junto ao seletor de cores
  Then o ícone da estrela deve mudar para o estado preenchido/marcado
  And a cor selecionada deve ser exibida imediatamente no painel "Favoritas" abaixo do seletor
```

---

### TASK-70432-CT-002 – Validar seleção de cor a partir do painel de favoritas
**Tipo:** Funcional | **Prioridade:** Alta

```gherkin
Cenário: Selecionar cor favoritada no painel
  Given que acesso a tela de Calendário Escolar no módulo "Acadêmico => Calendário e horário => Calendário escolar"
  And clico em cadastrar ou editar um "Grupo de Observações"
  And possuo cores salvas no painel "Favoritas"
  When clico em uma das amostras de cor no painel "Favoritas"
  Then o seletor de cores deve atualizar imediatamente a cor ativa para a cor clicada
  And o ícone da estrela correspondente deve ser exibido como preenchido/marcado para essa cor
```

---

### TASK-70432-CT-003 – Validar desfavoritação de uma cor pelo ícone de estrela
**Tipo:** Funcional | **Prioridade:** Alta

```gherkin
Cenário: Remover cor dos favoritos
  Given que acesso a tela de Calendário Escolar no módulo "Acadêmico => Calendário e horário => Calendário escolar"
  And clico em cadastrar ou editar um "Grupo de Observações"
  And possuo a cor atual favoritada com o ícone de estrela preenchido
  When clico novamente no ícone de estrela para desfavoritar
  Then o ícone da estrela deve retornar ao estado original de estrela vazia
  And a respectiva cor deve ser removida do painel "Favoritas"
```

---

### TASK-70432-CT-004 – Validar isolamento das cores favoritas por usuário
**Tipo:** Permissão | **Prioridade:** Alta

```gherkin
Cenário: Armazenamento individual de preferências de cores
  Given que estou logado com o usuário "Coordenador A"
  And acessei a tela de cadastro de "Grupo de Observações"
  And favoritei a cor "#FF0000" (Vermelho)
  When o usuário "Secretária B" efetua login no sistema no mesmo ambiente
  And acessa a tela de cadastro de "Grupo de Observações"
  Then o usuário "Secretária B" não deve visualizar a cor "#FF0000" no seu painel "Favoritas"
  And as preferências do "Coordenador A" não devem ser alteradas ou vistas pela "Secretária B"
```

---

### TASK-70432-CT-005 – Validar persistência de cores favoritas após logout/login
**Tipo:** Regras de negócio | **Prioridade:** Alta

```gherkin
Cenário: Persistência de favoritos do usuário
  Given que estou logado no sistema
  And favoritei as cores "#00FF00" e "#0000FF" na tela de "Grupo de Observações"
  When realizo o logout do sistema
  And efetuo o login novamente com as mesmas credenciais
  And acesso a tela de cadastro de "Grupo de Observações"
  Then o painel "Favoritas" deve continuar exibindo as cores "#00FF00" e "#0000FF"
  And o ícone de estrela deve estar preenchido ao selecionar qualquer uma destas duas cores
```

---

### TASK-70432-CT-006 – Validar comportamento com painel de favoritas vazio
**Tipo:** Borda | **Prioridade:** Média

```gherkin
Cenário: Seletor sem cores favoritadas
  Given que acesso a tela de cadastro de "Grupo de Observações"
  And não possuo nenhuma cor favoritada no meu histórico de usuário
  When visualizo o seletor de cores
  Then o painel "Favoritas" não deve exibir amostras de cores
  And deve se manter vazio sem quebrar o layout da tela de cadastro
```

---

### TASK-70432-CT-007 – Validar alteração dinâmica e sincronizada do ColorPicker
**Tipo:** Validação de campos | **Prioridade:** Média

```gherkin
Cenário: Sincronização em tempo real do seletor
  Given que acesso a tela de cadastro de "Grupo de Observações"
  When movo o ponteiro no ColorPicker ou digito um código hexadecimal
  Then o componente de visualização da cor ativa deve ser atualizado imediatamente de forma sincronizada
  And a cor aplicada na simulação do Grupo de Observações deve mudar instantaneamente
```

---

### TASK-70432-CT-008 – Validar contraste e layout no Tema Escuro
**Tipo:** Mensagens de erro/alerta | **Prioridade:** Alta

```gherkin
Cenário: Legibilidade do texto explicativo e borda no Tema Escuro
  Given que o tema do sistema está configurado como "Tema Escuro" em "Configurações => Tema"
  And acesso a tela de manutenção de Observações do Calendário Escolar
  When visualizo a tela sem nenhuma data selecionada
  Then o texto explicativo "Nenhuma data selecionada. Selecione uma data e clique em \"+ Data\"." deve ser perfeitamente legível com alto contraste
  And o seletor da cor ativa selecionada deve possuir uma borda fina destacando-a conforme o tema escuro
```

---

### TASK-70432-CT-009 – Validar comportamento visual no Tema Claro
**Tipo:** Regressão | **Prioridade:** Média

```gherkin
Cenário: Borda fina da cor ativa no Tema Claro
  Given que o tema do sistema está configurado como "Tema Claro" em "Configurações => Tema"
  When acesso a tela de manutenção de Observações do Calendário Escolar
  Then o seletor da cor ativa selecionada deve apresentar a borda fina de destaque
  And o painel de cores favoritas deve se adaptar visualmente de acordo com a paleta do Tema Claro
```

---

### TASK-70432-CT-010 – Validar emissão do calendário escolar
**Tipo:** Integração | **Prioridade:** Alta

```gherkin
Cenário: Emissão de calendário com cores corretas cadastradas
  Given que cadastrei um "Grupo de Observações" associando a cor favoritada "#FF00FF"
  And vinculei este grupo a datas específicas do calendário escolar
  When acesso o caminho "Acadêmico => Calendário e horário => Emissões => Emissão de calendário escolar"
  And realizo a emissão do documento do calendário escolar
  Then o calendário emitido deve exibir as observações na cor cadastrada "#FF00FF"
  And o layout do relatório final/pdf gerado deve manter a formatação correta sem quebras
```

---

### TASK-70432-CT-011 – Validar emissão de calendário pela Matriz Curricular
**Tipo:** Integração | **Prioridade:** Alta

```gherkin
Cenário: Emissão de calendário escolar via Matriz Curricular
  Given que cadastrei um "Grupo de Observações" associando a cor favoritada "#FF00FF"
  And vinculei este grupo a datas específicas do calendário escolar
  When acedo a tela "Acadêmico => Matriz Curricular => Calendário e horário => Emissões => Emissão de calendário"
  And realizo a emissão do calendário
  Then o documento gerado deve carregar a observação com a cor correspondente de forma adequada
```

---

### TASK-70432-CT-012 – Validar comportamento com grande volume de favoritas (Estresse)
**Tipo:** Borda | **Prioridade:** Baixa

```gherkin
Cenário: Adicionar grande volume de cores favoritas
  Given que acesso a tela de cadastro de "Grupo de Observações"
  When favorito 25 cores distintas sequencialmente
  Then o painel "Favoritas" deve comportar todas as amostras
  And o layout do painel deve quebrar linha ou disponibilizar rolagem mantendo a integridade visual da tela
```
