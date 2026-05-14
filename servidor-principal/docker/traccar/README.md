# Traccar

Plataforma de rastreamento GPS open-source. Suporta centenas de protocolos de dispositivos GPS.

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `http://IP:8083` | Interface web |
| `IP:5055` | Porta de dados GPS (protocolo OsmAnd) |

---

## Quick Start

```bash
docker compose up -d
```

---

## Configuração

As configurações ficam em `./data/conf/traccar.xml`. Para banco de dados externo (MySQL/PostgreSQL), edite esse arquivo antes de subir o container.

---

## Protocolos suportados

O Traccar suporta mais de 200 protocolos. A porta `5055` é o protocolo OsmAnd, compatível com o app [OsmAnd](https://osmand.net/) para rastreamento via smartphone.
