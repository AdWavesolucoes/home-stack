# Watchtower

Atualiza automaticamente as imagens Docker dos containers marcados com a label correspondente. Notifica via Telegram após cada ciclo.

---

## Quick Start

```bash
cp .env.example .env
# edite .env com o token do bot Telegram
docker compose up -d
```

## Variáveis de Ambiente

| Variável | Descrição |
|----------|-----------|
| `WATCHTOWER_NOTIFICATION_URL` | URL do Telegram no formato `telegram://TOKEN@telegram/?chats=CHAT_ID` |

---

## Como funciona

O Watchtower verifica novos releases das imagens no registry a cada **6 horas** (`POLL_INTERVAL=21600`). Quando encontra uma versão mais nova:
1. Para o container
2. Baixa a nova imagem
3. Reinicia com as mesmas configurações
4. Remove a imagem antiga (`CLEANUP=true`)

## Opt-in por label

Somente containers com a label abaixo são gerenciados:

```yaml
labels:
  com.centurylinklabs.watchtower.enable: "true"
```

## Notificações Telegram

Após cada ciclo de verificação, um relatório é enviado com containers atualizados e falhas.

---

## docker run

```bash
docker run -d \
  --name watchtower \
  --restart always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower:1.7.1
```

---

## Como funciona

O Watchtower verifica periodicamente se há novas versões das imagens no registry. Quando encontra, para o container, baixa a nova imagem e reinicia com as mesmas configurações.

## Opt-in por label

Para que o Watchtower atualize um container, adicione a label:

```bash
--label com.centurylinklabs.watchtower.enable=true
```

Containers sem essa label são ignorados.
