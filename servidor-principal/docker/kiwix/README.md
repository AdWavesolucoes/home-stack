# Kiwix

Servidor de conteúdo offline. Serve arquivos `.zim` (Wikipedia, Stack Overflow, Project Gutenberg etc.) sem necessidade de internet.

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `http://IP:18181` | Interface web |

---

## Quick Start

1. Baixe um arquivo `.zim` em [library.kiwix.org](https://library.kiwix.org)
2. Coloque em `/opt/kiwix/zim/`
3. Suba o container:

```bash
docker compose up -d
```

O container detecta automaticamente os arquivos `.zim` e os serve.

---

## Arquivos ZIM disponíveis

| Conteúdo | Tamanho aprox. |
|----------|----------------|
| Wikipedia PT | ~90 GB |
| Stack Overflow | ~50 GB |
| Project Gutenberg | ~60 GB |

> Os arquivos ficam em `/opt/kiwix/zim` (somente leitura no container).
