# Guia de Instalação e Configuração MCP no Antigravity

## 1. Visão Geral
Este documento explica como configurar um MCP (Model Connector Provider) para uso no Antigravity. O exemplo a seguir usa a integração com o MantisBT via um MCP server Node.js.

## 2. Local do arquivo de configuração
Edite o arquivo de configuração local do Antigravity:

```bash
~/.gemini/config/mcp_config.json
```

> Se o arquivo não existir, crie-o.

## 3. Configuração para MantisBT
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

Se a configuração estiver correta, o MCP do MantisBT deve ser listado ou iniciado sem erros.

## 5. Dicas e solução de problemas
- Verifique se o `npx` está instalado e disponível no PATH.
- Confirme se o token do MantisBT ainda está ativo.
- Use a URL completa da API do MantisBT, incluindo `/api/rest`.
- Se houver erro no JSON, valide o arquivo em um validador JSON online ou com `python -m json.tool`.


