# Open WebUI

Interface web para interagir com modelos de linguagem locais via Ollama.

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `https://aichat.lan` | Acesso via proxy reverso (NPM) |
| `http://IP:8085` | Acesso direto |

---

## Quick Start

```bash
docker compose up -d
```

---

## Integração com Ollama

O Ollama roda como serviço systemd no host (`ollama.service`) na porta `11434`. O Open WebUI se conecta via `http://172.17.0.1:11434` (IP do host na bridge Docker).

Modelos disponíveis:
```bash
ollama list
```

---

## Observações

- Autenticação habilitada (`WEBUI_AUTH=true`) — primeiro acesso cria a conta admin
- Volume `open-webui` persiste histórico de conversas, configurações e usuários
