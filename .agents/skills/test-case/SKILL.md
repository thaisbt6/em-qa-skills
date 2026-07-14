---
name: test-case
description: Cria ciclos/planos de testes e casos de testes no Zephyr (Jira) usando o MCP zephyr-server, a partir de um arquivo de plano de testes ou de argumentos informados pelo usuário.
---

# Importador de Casos de Teste para o Zephyr (Jira)

Esta skill capacita o agente a ler cenários de teste (em formato Gherkin/BDD ou texto estruturado) a partir de um arquivo markdown ou de argumentos diretos do usuário e criá-los automaticamente no Zephyr usando o MCP `zephyr-server`. Além disso, agrupa os casos de teste criados em um novo ciclo de testes (Test Run/Cycle).

---

## 1. Entrada de Dados Requerida

O processo de criação no Zephyr deve iniciar quando o usuário fornecer ou quando você identificar:
- **Origem dos Casos de Teste:**
  - O caminho de um arquivo local (ex: `/Users/thaisbt/Projetos/em-qa-skills/test_plan_TASK-1234.md`) que contém os cenários de teste gerados pela skill `qa-senior-analyst`.
  - Ou os próprios cenários de teste fornecidos diretamente nos argumentos do prompt.
- **Project Key do Jira:** A chave do projeto no Jira onde o Zephyr está configurado (ex: `PROJ` ou `HOM`).
- **Nome do Ciclo de Teste (Test Run):** Nome amigável para o ciclo (ex: `Ciclo de Homologação - TASK-1234`).
- **Pasta no Zephyr (Opcional):** O caminho da pasta para organizar os casos de teste (ex: `/TASK-1234`). Se a pasta não existir, ela deve ser criada.
- **Jira Issue Key / Link (Opcional):** A chave da issue a ser vinculada aos casos de teste e ao ciclo (ex: `TASK-1234`).

---

## 2. Parsing dos Cenários de Teste

Se a entrada for um arquivo markdown (ex: gerado pela skill `qa-senior-analyst`), analise o conteúdo para extrair cada cenário estruturado:
1. **Identificador e Título:** Localizar títulos que seguem o padrão `TASK-XXXX-CT-YYY – Validar...` ou `### TASK-XXXX-CT-YYY – Validar...`.
2. **Metadados:**
   - **Tipo:** Localizar o tipo do cenário (ex: `Funcional`, `Negativo`, `Regressão`).
   - **Prioridade:** Mapear a prioridade para o padrão em inglês aceito pelo Zephyr:
     - `Alta` ou `Crítica` -> `High` ou `Critical`
     - `Média` -> `Normal`
     - `Baixa` -> `Low`
3. **Cenário Gherkin (BDD):** Extrair o bloco de código markdown delimitado por `gherkin` contendo `Cenário: ... Given ... When ... Then`.

Se a entrada for por argumentos/texto direto, estruture-os de forma equivalente antes do envio.

---

## 3. Fluxo de Execução com o MCP zephyr-server

Você deve seguir rigorosamente a sequência de passos abaixo, executando as ferramentas do MCP `zephyr-server` para criar a pasta, os casos de teste e o ciclo diretamente no Zephyr:

### Passo 1: Criar a pasta para os casos de teste (Opcional/Se solicitado)
Se um caminho de pasta for fornecido (ex: `"/TASK-1234"`):
- Verifique ou chame a ferramenta `create_folder`:
  - **Servidor:** `zephyr-server`
  - **Ferramenta:** `create_folder`
  - **Argumentos:**
    ```json
    {
      "project_key": "<JIRA_PROJECT_KEY>",
      "name": "<CAMINHO_DA_PASTA>",
      "folder_type": "TEST_CASE"
    }
    ```

### Passo 2: Criar os Casos de Teste (Test Cases)
Para cada cenário extraído:
1. Chame a ferramenta `create_test_case` do servidor `zephyr-server` para criar o caso de teste no Zephyr:
   - **Servidor:** `zephyr-server`
   - **Ferramenta:** `create_test_case`
   - **Argumentos:**
     ```json
     {
       "project_key": "<JIRA_PROJECT_KEY>",
       "name": "<TASK-XXXX-CT-YYY – Título do caso de teste>",
       "objective": "Cenário de teste para validar <objetivo>",
       "folder": "<CAMINHO_DA_PASTA>",
       "priority": "<Priority_Mapeada>",
       "issue_links": ["<JIRA_ISSUE_KEY>"],
       "test_script": {
         "type": "BDD",
         "text": "<Texto_Gherkin_Original>"
       }
     }
     ```
     *Nota:* Se houver tags ou campos personalizados adicionais na issue, você pode mapeá-los em `labels` ou `custom_fields`.
2. Guarde o ID/chave retornado pelo Zephyr para cada caso de teste criado (ex: `PROJ-T123`).

### Passo 3: Criar o Ciclo de Teste (Test Run)
Após a criação bem-sucedida de todos os casos de teste:
1. Chame a ferramenta `create_test_run` do servidor `zephyr-server` para agrupar todos eles em um ciclo no Zephyr:
   - **Servidor:** `zephyr-server`
   - **Ferramenta:** `create_test_run`
   - **Argumentos:**
     ```json
     {
       "project_key": "<JIRA_PROJECT_KEY>",
       "name": "<NOME_DO_CICLO>",
       "description": "Ciclo de testes contendo os casos gerados para a issue <JIRA_ISSUE_KEY>",
       "issue_links": ["<JIRA_ISSUE_KEY>"],
       "test_case_keys": [
         "<PROJ-T123>",
         "<PROJ-T124>"
       ]
     }
     ```
2. Guarde a chave do ciclo retornado (ex: `PROJ-R12`).

---

## 4. Retorno ao Usuário e Relatório

Ao concluir o processo de criação:
1. **Apresente uma tabela consolidada** com os resultados:
   - ID Local do Cenário (ex: `CT-001`)
   - Título do Cenário
   - Chave no Zephyr (ex: `PROJ-T123`)
   - Status de Criação (Sucesso / Erro)
2. **Informe a chave do Ciclo de Testes (Test Run)** gerado (ex: `PROJ-R12`).
3. Forneça instruções sobre como o usuário pode acessar e executar estes testes no Jira/Zephyr.
