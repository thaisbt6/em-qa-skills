# Guia de Instalação e Configuração MCP no Antigravity

## 1. Visão Geral
Este documento explica como configurar MCPs (Model Connector Providers) para uso no Antigravity. Os exemplos a seguir usam integrações com o MantisBT e o Zephyr via servidores MCP Node.js.

## 2. Local do arquivo de configuração
Edite o arquivo de configuração local do Antigravity:

```bash
~/.gemini/config/mcp_config.json
```

> Se o arquivo não existir, crie-o.

## 3. Configuração para MantisBT e Zephyr

### 3.1 MantisBT
1. Gere um token de acesso no MantisBT.
2. Copie a URL base da API do MantisBT.
3. Atualize o arquivo `mcp_config.json` com os valores corretos de `MANTIS_BASE_URL` e `MANTIS_API_KEY`.

Exemplo de configuração:

```json
{
  "mcpServers": {
    "mantisbt": {
      "command": "npx",
      "args": ["-y", "@dpesch/mantisbt-mcp-server"],
      "env": {
        "MANTIS_BASE_URL": "https://seu-mantis.exemplo.com/api/rest",
        "MANTIS_API_KEY": "seu-token-de-api"
      }
    }
  }
}
```

### 3.2 Zephyr
1. Gere um token de acesso no Zephyr.
2. Copie a URL base da API do Zephyr.
3. Atualize o arquivo `mcp_config.json` com os valores corretos de `ZEPHYR_BASE_URL` e `ZEPHYR_API_TOKEN`.

Exemplo de configuração:

```json
{
  "mcpServers": {
    "zephyr-server": {
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

### 3.3 Exemplo completo com os dois MCPs

```json
{
  "mcpServers": {
    "mantisbt": {
      "command": "npx",
      "args": ["-y", "@dpesch/mantisbt-mcp-server"],
      "env": {
        "MANTIS_BASE_URL": "https://seu-mantis.exemplo.com/api/rest",
        "MANTIS_API_KEY": "seu-token-de-api"
      }
    },
    "zephyr-server": {
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

### Campos importantes
- `command`: comando usado para iniciar o servidor MCP.
- `args`: argumentos passados ao comando.
- `env`: variáveis de ambiente necessárias para autenticação e conexão.

## 4. Testando a configuração
1. Feche o terminal ou IDE se o `agy` já estiver aberto.
2. Abra o `agy` novamente.
3. Digite o comando:

```text
/mcp
```

Se a configuração estiver correta, os MCPs do MantisBT e do Zephyr devem ser listados ou iniciados sem erros.

## 5. Dicas e solução de problemas
- User o Node.js versão 22 ou superior
- Verifique se o `npx` está instalado e disponível no PATH.
- Confirme se os tokens do MantisBT e do Zephyr ainda estão ativos.
- Use a URL completa da API do MantisBT, incluindo `/api/rest`, e do Zephyr, incluindo `/v2`.
- Se houver erro no JSON, valide o arquivo em um validador JSON online ou com `python -m json.tool`.


