# AdWave Landing Page

Site institucional da AdWave servido via Nginx Alpine.

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `http://IP:8091` | Acesso direto |

---

## Quick Start

```bash
docker compose up -d
```

---

## Estrutura

Os arquivos do site ficam na raiz do projeto e são montados como volume somente leitura no container Nginx. Qualquer alteração nos arquivos é refletida instantaneamente sem reiniciar o container.

```
adwave-landing/
├── index.html
├── docker-compose.yml
└── ...
```
