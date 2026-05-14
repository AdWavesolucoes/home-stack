# Home Assistant

Plataforma de automação residencial open-source. Integra dispositivos IoT, sensores, câmeras e serviços externos.

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `https://ha.lan` | Acesso via proxy reverso (NPM) |
| `http://IP:8123` | Acesso direto (modo host) |

---

## Quick Start

```bash
docker compose up -d
```

---

## Integrações ativas

- **MQTT / Mosquitto** — sensores ESP32, PC monitor
- **Ollama** — integração com LLM local
- **Bluetooth** — dispositivos BLE via host

---

## Observações

- `network_mode: host` — necessário para descoberta de dispositivos na rede local (mDNS, UPnP, Bluetooth)
- `privileged: true` — acesso ao hardware do host
- As configurações ficam em `./config`, persistidas entre atualizações
