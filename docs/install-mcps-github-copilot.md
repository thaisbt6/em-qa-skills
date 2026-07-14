# Guia de instalação dos MCPs para GitHub Copilot no VS Code

## 1. Visão geral
Este guia adapta a configuração dos MCPs do MantisBT e do Zephyr para o GitHub Copilot no VS Code.

> Observação: o arquivo de configuração usado pelo Antigravity não é o mesmo usado pelo GitHub Copilot. Para o Copilot, a configuração costuma ser feita no arquivo de workspace do VS Code.

## 2. Pré-requisitos
Antes de começar, verifique se você possui:

- VS Code com suporte a GitHub Copilot/Agent
- Node.js 22 ou superior
- `npx` disponível no PATH
- Token de API e URL base do MantisBT
- Token de API e URL base do Zephyr

## 3. Criar o arquivo de configuração do VS Code
Na raiz do seu workspace, crie a pasta `.vscode` (se ainda não existir) e um arquivo chamado `mcp.json`.

```bash
mkdir -p .vscode
```

Em seguida, crie o arquivo `.vscode/mcp.json` com o conteúdo abaixo:

```json
{
  "servers": {
    "mantisbt": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@dpesch/mantisbt-mcp-server"],
      "env": {
        "MANTIS_BASE_URL": "https://seu-mantis.exemplo.com/api/rest",
        "MANTIS_API_KEY": "seu-token-de-api"
      }
    },
    "zephyr-server": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@smartbear/mcp@latest"],
      "env": {
        "ZEPHYR_BASE_URL": "https://api.zephyrscale.smartbear.com/v2",
        "ZEPHYR_API_TOKEN": "seu-token-de-api"
      }
    }
  }
}
```

## 4. Ajustar os valores reais
Substitua os valores de exemplo pelos seus dados reais:

- `MANTIS_BASE_URL`: URL completa da API do MantisBT, incluindo `/api/rest`
- `MANTIS_API_KEY`: token de acesso do MantisBT
- `ZEPHYR_BASE_URL`: URL base da API do Zephyr, incluindo `/v2`
- `ZEPHYR_API_TOKEN`: token de acesso do Zephyr

## 5. Reiniciar o VS Code
Depois de salvar o arquivo:

1. Feche e abra o VS Code novamente, ou
2. Execute o comando `Developer: Reload Window` na paleta de comandos

## 6. Validar a configuração no GitHub Copilot
Com o VS Code reaberto:

1. Abra o painel do GitHub Copilot Chat
2. Ative o modo Agent, se necessário
3. Tente usar um prompt como:

```text
Use o MCP do MantisBT para listar as issues recentes.
```

Se a configuração estiver correta, o Copilot deverá conseguir acessar os servidores MCP configurados.

## 7. Solução de problemas
Se algo não funcionar:

- Verifique se o Node.js e o `npx` estão instalados corretamente
- Confirme se os tokens ainda estão válidos
- Verifique se as URLs incluem o sufixo correto (`/api/rest` para MantisBT e `/v2` para Zephyr)
- Abra o painel de Output e procure por mensagens relacionadas a MCP ou GitHub Copilot
- Reinicie o VS Code após qualquer alteração no arquivo de configuração
