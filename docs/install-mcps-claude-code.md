# Guia de instalação dos MCPs para Claude Code

## 1. Visão geral
Este guia mostra como configurar servidores MCP para uso no Claude Code com integrações do MantisBT e do Zephyr.

O Claude Code usa um arquivo chamado `.mcp.json` para configurar servidores MCP no escopo do projeto. Para servidores locais baseados em stdio, a configuração costuma ser feita nesse arquivo na raiz do workspace.

## 2. Pré-requisitos
Antes de começar, verifique se você possui:

- Claude Code instalado e funcionando
- Node.js 22 ou superior
- `npx` disponível no PATH
- Token de API e URL base do MantisBT
- Token de API e URL base do Zephyr

## 3. Criar o arquivo de configuração do projeto
Na raiz do seu workspace, crie ou edite o arquivo `.mcp.json` com o conteúdo abaixo:

```json
{
  "mcpServers": {
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

## 5. Reiniciar ou recarregar o Claude Code
Depois de salvar o arquivo:

1. Feche e abra o Claude Code novamente, ou
2. Execute o comando `/reload` se disponível na sessão atual

## 6. Validar a configuração
Com o Claude Code aberto, use os seguintes comandos:

```bash
claude mcp list
```

Ou dentro da sessão do Claude Code:

```text
/mcp
```

Se a configuração estiver correta, os servidores MCP do MantisBT e do Zephyr devem aparecer como conectados ou prontos para uso.

## 7. Aprovação em projetos novos
Se o workspace for novo ou não estiver confiável, o Claude Code pode pedir aprovação antes de conectar os servidores definidos em `.mcp.json`.

Nesses casos:

- aceite a confirmação de confiança do workspace, se necessário;
- confirme a aprovação dos servidores MCP;
- tente novamente o comando `/mcp` ou `claude mcp list`.

## 8. Solução de problemas
Se algo não funcionar:

- verifique se o Node.js e o `npx` estão instalados corretamente;
- confirme se os tokens ainda estão válidos;
- confira se as URLs incluem o sufixo correto (`/api/rest` para MantisBT e `/v2` para Zephyr);
- valide se o arquivo `.mcp.json` está em formato JSON correto;
- verifique se o Claude Code está sendo executado dentro da pasta correta do projeto;
- reinicie o Claude Code após qualquer alteração no arquivo de configuração.
