# PC Monitor Webapp

Dashboard em tempo real que exibe métricas de hardware do PC (CPU, RAM, temperatura, GPU) recebidas via MQTT.

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `http://IP:3000` | Interface web |

---

## Quick Start

```bash
cp .env.example .env
# edite .env com as credenciais MQTT
docker compose up -d
```

## Variáveis de Ambiente

| Variável | Descrição |
|----------|-----------|
| `MQTT_USER` | Usuário do broker Mosquitto |
| `MQTT_PASS` | Senha do broker Mosquitto |

---

## Arquitetura

```
PC Windows
    │  agente de coleta (ESP32 ou software)
    ▼
Mosquitto (MQTT)
    │  tópico: pc/#
    ▼
pc-monitor-webapp   ← atualiza o dashboard via WebSocket
```

---

## Dependências

- Requer o container `mosquitto` rodando na rede `monitor-net`
- Imagem construída localmente a partir do `Dockerfile` em `pc-monitor-webapp/`
