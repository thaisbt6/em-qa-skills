# Skills de QA para o projeto

Este repositório reúne skills para apoiar atividades de QA e testes, com foco em:

- geração de planos de teste a partir de issues;
- importação de cenários para o Zephyr/Jira.

## Estrutura do projeto

- [docs/install-mcps-antigravity.md](docs/install-mcps-antigravity.md): guia de instalação e configuração dos MCPs usados pelo Antigravity.
- [docs/install-mcps-github-copilot.md](docs/install-mcps-github-copilot.md): guia de instalação dos MCPs para o GitHub Copilot no VS Code.
- [docs/install-mcps-claude-code.md](docs/install-mcps-claude-code.md): guia de instalação dos MCPs para o Claude Code.
- [.agents/skills/qa-senior-analyst/SKILL.md](.agents/skills/qa-senior-analyst/SKILL.md): skill para criar planos de teste com base em issues do MantisBT.
- [.agents/skills/test-case/SKILL.md](.agents/skills/test-case/SKILL.md): skill para criar casos de teste no Zephyr a partir de cenários Gherkin.
- [test-plans/test_plan_TASK-70432.md](test-plans/test_plan_TASK-70432.md): exemplo de plano de testes gerado.

## Como usar as skills no workspace atual

1. Abra esta pasta no VS Code.
2. Certifique-se de que os arquivos de skill estão disponíveis em [.agents/skills](.agents/skills).
3. Use o agente com prompts como os exemplos abaixo:

### Exemplo 1: gerar um plano de teste

```text
Crie um plano de testes para a task TASK-70432 com foco em regressão e cenários negativos.
```

A skill de análise de QA vai buscar contexto, estruturar o plano e salvar o arquivo em uma pasta como [test-plans](test-plans).

### Exemplo 2: importar cenários para o Zephyr

```text
Crie os casos de teste no Zephyr a partir do arquivo de plano em test-plans/test_plan_TASK-70432.md.
```

A skill de importação usa o MCP do Zephyr para criar casos e ciclos de teste.

## Pré-requisitos

Antes de usar as skills que dependem de integrações externas, configure os MCPs de acordo com o guia mais adequado para o seu ambiente:

- [docs/install-mcps-antigravity.md](docs/install-mcps-antigravity.md) para Antigravity
- [docs/install-mcps-github-copilot.md](docs/install-mcps-github-copilot.md) para GitHub Copilot no VS Code
- [docs/install-mcps-claude-code.md](docs/install-mcps-claude-code.md) para Claude Code

Os itens principais são:

- Node.js 22 ou superior;
- acesso ao MantisBT e/ou Zephyr;
- tokens e URLs corretas configuradas no arquivo de configuração do MCP.

## Instalação dos MCPs por agente de IA

A instalação dos MCPs varia um pouco conforme o agente de IA que você estiver usando. Os passos abaixo resumem o fluxo recomendado para cada ambiente.

### 1. Verifique os pré-requisitos

No terminal, confirme se o Node.js e o `npx` estão disponíveis:

```bash
# macOS / Linux
node --version
npx --version
```

```powershell
# Windows PowerShell
node --version
npx --version
```

### 2. Configure o arquivo correto para cada agente

- Antigravity: [docs/install-mcps-antigravity.md](docs/install-mcps-antigravity.md)
- GitHub Copilot: [docs/install-mcps-github-copilot.md](docs/install-mcps-github-copilot.md)
- Claude Code: [docs/install-mcps-claude-code.md](docs/install-mcps-claude-code.md)

Os arquivos mais comuns são:

- Antigravity: `~/.gemini/config/mcp_config.json`
- GitHub Copilot: `.vscode/mcp.json`
- Claude Code: `.mcp.json`

### 3. Exemplo de uso do terminal para criar pastas de configuração

No macOS/Linux:

```bash
mkdir -p .vscode
mkdir -p .mcp
```

No Windows PowerShell:

```powershell
New-Item -ItemType Directory -Path .vscode -Force | Out-Null
New-Item -ItemType Directory -Path .mcp -Force | Out-Null
```

No Windows CMD:

```bat
mkdir .vscode
mkdir .mcp
```

## Como instalar as skills nos agentes

A pasta [.agents](.agents) deste repositório pode ser usada por diferentes agentes. Abaixo estão exemplos práticos de instalação.

### Antigravity

Exemplo assumindo que o Antigravity/CLI aceite agentes a partir de `~/.gemini/agents`.

No macOS/Linux:

```bash
mkdir -p ~/.gemini/agents
ln -s /caminho/real/para/em-qa-skills/.agents ~/.gemini/agents/qa-skills
```

No Windows PowerShell:

```powershell
New-Item -ItemType Directory -Path "$HOME\.gemini\agents" -Force | Out-Null
New-Item -ItemType SymbolicLink -Path "$HOME\.gemini\agents\qa-skills" -Target "C:\caminho\real\para\em-qa-skills\.agents" -Force
```

Se o link simbólico não for suportado no seu ambiente Windows, faça uma cópia manual da pasta.

### Claude Code

Exemplo assumindo que o Claude Code use uma pasta local de agentes em `~/.claude/agents`.

No macOS/Linux:

```bash
mkdir -p ~/.claude/agents
cp -R /caminho/real/para/em-qa-skills/.agents ~/.claude/agents/qa-skills
```

No Windows PowerShell:

```powershell
New-Item -ItemType Directory -Path "$HOME\.claude\agents" -Force | Out-Null
Copy-Item -Path "C:\caminho\real\para\em-qa-skills\.agents" -Destination "$HOME\.claude\agents\qa-skills" -Recurse -Force
```

### GitHub Copilot

Exemplo usando a pasta global de agentes do Copilot.

No macOS/Linux:

```bash
mkdir -p ~/.config/github-copilot
ln -s /caminho/real/para/em-qa-skills/.agents ~/.config/github-copilot/agents
```

No Windows PowerShell:

```powershell
New-Item -ItemType Directory -Path "$HOME\.config\github-copilot" -Force | Out-Null
New-Item -ItemType SymbolicLink -Path "$HOME\.config\github-copilot\agents" -Target "C:\caminho\real\para\em-qa-skills\.agents" -Force
```

> Ajuste os caminhos para o diretório real do seu projeto, se necessário.

## Instalação global das skills

Se você quiser deixar essas skills disponíveis para outros workspaces ou para o seu ambiente de forma mais permanente, siga um destes caminhos:

### Opção 1: link simbólico

No macOS/Linux, você pode criar um link simbólico para a pasta de skills:

```bash
mkdir -p ~/.config/github-copilot
ln -s /caminho/real/para/em-qa-skills/.agents ~/.config/github-copilot/agents
```

No Windows PowerShell:

```powershell
New-Item -ItemType Directory -Path "$HOME\.config\github-copilot" -Force | Out-Null
New-Item -ItemType SymbolicLink -Path "$HOME\.config\github-copilot\agents" -Target "C:\caminho\real\para\em-qa-skills\.agents" -Force
```

### Opção 2: cópia manual

1. Copie a pasta [.agents](.agents) para o diretório de configuração global do seu ambiente.
2. Reinicie o editor ou a sessão do agente.
3. Confirme que as skills aparecem para uso em novos workspaces.

## Fluxo recomendado

1. Configure os MCPs usando o guia correspondente ao seu ambiente: [docs/install-mcps-antigravity.md](docs/install-mcps-antigravity.md), [docs/install-mcps-github-copilot.md](docs/install-mcps-github-copilot.md) ou [docs/install-mcps-claude-code.md](docs/install-mcps-claude-code.md).
2. Use a skill de análise para gerar um plano de testes.
3. Use a skill de importação para criar os casos no Zephyr.
4. Revise os arquivos gerados em [test-plans](test-plans).

## Dicas

- Mantenha os arquivos gerados em [test-plans](test-plans) para fácil rastreio.
- Se a skill não encontrar informações na issue, forneça o contexto manualmente no prompt.
- Valide sempre os tokens, URLs e permissões antes de executar integrações externas.
