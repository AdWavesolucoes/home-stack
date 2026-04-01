# Docker — Serviços em Container

Todos os serviços orquestrados via Docker Compose. Alguns serviços têm compose próprio, outros estão no compose central.

---

## Instalação do Docker

```bash
curl -fsSL https://get.docker.com | sh

# Adicionar usuário ao grupo docker (evita sudo)
usermod -aG docker $USER

# Habilitar e iniciar
systemctl enable docker
systemctl start docker
```

### Verificar instalação

```bash
docker --version
docker compose version
```

---

## Estrutura dos serviços

```
docker/
├── docker-compose.yml      ← Serviços centrais (monitoramento, infra)
├── .env.example            ← Variáveis de ambiente (copiar para .env)
├── grafana/
│   └── provisioning/       ← Dashboards e datasources pré-configurados
├── homepage/
│   └── config/             ← Configuração do dashboard
├── mosquitto/
│   └── mosquitto.conf      ← Config do broker MQTT
├── npm/                    ← Nginx Proxy Manager (dados persistidos em volume)
└── prometheus/
    └── prometheus.yml      ← Targets de coleta de métricas
```

---

## Configuração inicial

### 1. Criar arquivo de variáveis de ambiente

```bash
cp .env.example .env
nano .env  # Preencher com seus valores reais
```

### 2. Criar rede Docker compartilhada

```bash
docker network create homelab
```

### 3. Subir todos os serviços

```bash
docker compose up -d
```

### 4. Verificar status

```bash
docker compose ps
docker compose logs -f
```

---

## Serviços e portas

| Container | Porta | Descrição | URL |
|-----------|-------|-----------|-----|
| `npm` | 80, 443, 81 | Nginx Proxy Manager | `http://IP:81` |
| `portainer` | 9000, 9443 | Gestão de containers | `http://IP:9000` |
| `grafana` | 3000 | Dashboards | `http://IP:3000` |
| `prometheus` | 9090 | Métricas | `http://IP:9090` |
| `cadvisor` | 8080 | Métricas de containers | `http://IP:8080` |
| `homepage` | 3005 | Dashboard de serviços | `http://IP:3005` |
| `navidrome` | 4533 | Servidor de música | `http://IP:4533` |
| `plex` | 32400 | Servidor de mídia | `http://IP:32400/web` |
| `qbittorrent` | 8090 | Cliente BitTorrent | `http://IP:8090` |
| `mosquitto` | 1883, 9001 | Broker MQTT | — |
| `postgres` | 5432 | Banco de dados | — |
| `node-exporter` | 9100 | Métricas do host | — |
| `kiwix` | 8888 | Conteúdo offline | `http://IP:8888` |
| `minecraft` | 25565 | Servidor Minecraft | — |
| `watchtower` | — | Atualização automática | — |

> Portas internas não precisam estar abertas no firewall se acessadas via NPM.

---

## Nginx Proxy Manager

Após subir os containers, configurar o NPM para rotear domínios locais:

1. Acessar `http://IP:81`
2. Login padrão: `admin@example.com` / `changeme` (trocar no primeiro acesso)
3. Criar Proxy Hosts para cada serviço

Exemplo de configuração de proxy host:
- **Domain:** `grafana.home`
- **Forward Hostname:** `grafana`
- **Forward Port:** `3000`
- **Websockets:** Enabled (para Grafana)

---

## Volumes e dados persistentes

Os dados são persistidos em `/opt/homelab/` no host:

```bash
mkdir -p /opt/homelab/{grafana,prometheus,portainer,postgres,npm,navidrome,plex,qbittorrent,mosquitto,kiwix,homepage,minecraft}
```

---

## Comandos úteis

```bash
# Subir todos
docker compose up -d

# Parar todos
docker compose down

# Ver logs de um serviço
docker compose logs -f grafana

# Reiniciar um serviço
docker compose restart grafana

# Atualizar imagens
docker compose pull
docker compose up -d

# Ver uso de recursos
docker stats

# Remover imagens não usadas
docker image prune -a
```

---

## Backup dos volumes

```bash
# Backup completo dos dados
tar -czf homelab-backup-$(date +%Y%m%d).tar.gz /opt/homelab/

# Backup individual (ex: Grafana)
tar -czf grafana-backup-$(date +%Y%m%d).tar.gz /opt/homelab/grafana/
```
