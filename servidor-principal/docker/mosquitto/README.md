# Mosquitto

Broker MQTT leve (Eclipse Mosquitto) para comunicação entre dispositivos IoT.

---

## Acesso

| Protocolo | Porta | Descrição |
|-----------|-------|-----------|
| MQTT | `1883` | Conexão padrão |
| WebSocket | `9001` | Para clientes web |

---

## Quick Start

```bash
docker compose up -d
```

---

## Clientes conectados

| Cliente | Tópico(s) | Descrição |
|---------|-----------|-----------|
| ESP32 | `sensor/#` | Métricas de hardware |
| PC Monitor | `pc/#` | CPU, RAM, temperatura |
| Home Assistant | `#` | Recebe todos os eventos |
| pc-monitor-webapp | `pc/#` | Dashboard em tempo real |

---

## Configuração

Os arquivos de configuração ficam em `./config/mosquitto.conf`. Para habilitar autenticação:

```
allow_anonymous false
password_file /mosquitto/config/passwd
```

Criar usuário:
```bash
docker exec -it mosquitto mosquitto_passwd -c /mosquitto/config/passwd <usuario>
```
