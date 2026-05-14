# home-stack

Infraestrutura doméstica self-hosted — documentação completa de todos os serviços, configurações de rede, DNS, VPN e automação rodando em bare metal e Docker.

---

## Hardware

| Dispositivo | Função | SO | CPU | RAM |
|-------------|--------|----|-----|-----|
| **Servidor Principal** | Serviços, DNS primário, Docker | DietPi (Debian 13) | Intel x86_64 | 16 GB |
| **Orange Pi Zero 2W** | DNS secundário | DietPi | Allwinner H618 (ARM64) | 1 GB |

---

## Arquitetura Geral

```
Internet
    │
    ▼
[ Roteador ] 192.168.0.1
    │
    └── LAN 192.168.0.0/24
             │
             ├── 192.168.0.100  ← Servidor Principal
             │    ├── Pi-hole + Unbound (DNS)
             │    ├── Tailscale (VPN mesh)
             │    ├── Nginx Proxy Manager (proxy reverso + TLS)
             │    ├── 20+ containers Docker
             │    ├── Ollama (LLM local)
             │    └── MariaDB, Samba, FTP
             │
             └── Orange Pi Zero 2W
                  └── DNS secundário (em configuração)
```

---

## Rede Interna

### DNS

| Camada | Endereço | Serviço | Função |
|--------|----------|---------|--------|
| DNS primário | `192.168.0.100:53` | Pi-hole | Filtragem de anúncios e DNS local |
| DNS recursivo | `127.0.0.1:5335` | Unbound | Resolução direta nos servidores raiz (sem terceiros) |
| DNS secundário | Orange Pi `:53` | Pi-hole | Failover (em configuração) |

### Domínios `.lan`

| Domínio | Serviço |
|---------|---------|
| `aichat.lan` | Open WebUI (Ollama) |
| `dns.lan` | Pi-hole |
| `grafana.lan` | Grafana |
| `ha.lan` | Home Assistant |
| `home.lan` | Homepage (dashboard) |
| `nextcloud.lan` | Nextcloud |
| `portainer.lan` | Portainer |
| `plex.lan` | Plex |

> Certificados TLS gerados com **mkcert** e CA raiz própria — HTTPS válido em toda a rede interna sem depender de Let's Encrypt.

### VPN — Tailscale

Rede mesh que conecta todos os dispositivos com IPs `100.x.x.x`, permitindo acesso remoto a todos os serviços sem abrir portas no roteador.

| Dispositivo | IP Tailscale |
|-------------|-------------|
| servidorcasa | `100.69.59.102` |
| desktop-lucas | `100.78.134.77` |
| poco-x7 | `100.71.92.86` |
| tab-s10-lite | `100.72.176.9` |

---

## Servidor Principal

### Serviços bare metal

| Serviço | Função |
|---------|--------|
| Pi-hole | DNS + bloqueio de anúncios |
| Unbound | Resolver DNS recursivo |
| Tailscale | VPN mesh |
| Ollama | LLM local (modelos de IA) |
| MariaDB | Banco de dados (FiveM) |
| Samba | Compartilhamento de arquivos na rede local |
| vsftpd | Servidor FTP |

### Containers Docker

| Container | Descrição | Porta | Docs |
|-----------|-----------|-------|------|
| adwave-landing | Landing page AdWave | `8091` | [→](servidor-principal/docker/adwave-landing/) |
| cadvisor | Métricas por container | `8084` | [→](servidor-principal/docker/observability/) |
| code-server | VS Code no navegador | `8443` | [→](servidor-principal/docker/code-server/) |
| grafana | Dashboards de monitoramento | `3001` | [→](servidor-principal/docker/grafana/) |
| homeassistant | Automação residencial | host | [→](servidor-principal/docker/homeassistant/) |
| homepage | Dashboard de serviços | `3005` | [→](servidor-principal/docker/homepage/) |
| kiwix | Biblioteca offline | `18181` | [→](servidor-principal/docker/kiwix/) |
| metrics-api | API de métricas (ESP32) | `5000` | [→](servidor-principal/docker/metrics-api/) |
| minecraft | Servidor Minecraft Forge 1.16.5 | `25565` | [→](servidor-principal/docker/minecraft/) |
| mosquitto | Broker MQTT (IoT) | `1883` | [→](servidor-principal/docker/mosquitto/) |
| nextcloud | Nuvem privada | `8888` | [→](servidor-principal/docker/nextcloud/) |
| node-exporter | Métricas do host | `9100` | [→](servidor-principal/docker/observability/) |
| npm | Nginx Proxy Manager | `80/443/81` | [→](servidor-principal/docker/npm/) |
| open-webui | Interface web para Ollama | `8085` | [→](servidor-principal/docker/open-webui/) |
| pc-monitor-webapp | Monitor de hardware via MQTT | `3000` | [→](servidor-principal/docker/pc-monitor-webapp/) |
| plex | Servidor de mídia | host | [→](servidor-principal/docker/plex/) |
| portainer | Gestão visual de containers | `9000` | [→](servidor-principal/docker/portainer/) |
| prometheus | Coleta de métricas | `9090` | [→](servidor-principal/docker/observability/) |
| qbittorrent | Cliente BitTorrent | `8081` | [→](servidor-principal/docker/qbittorrent/) |
| traccar | Rastreamento GPS | `8083` | [→](servidor-principal/docker/traccar/) |
| watchtower | Atualização automática de imagens | — | [→](servidor-principal/docker/watchtower/) |

### Stack de Observabilidade

```
Host
 ├── Node Exporter (9100)   ← CPU, RAM, disco, rede, processos
 └── cAdvisor (8084)        ← métricas por container
         │
         ▼
     Prometheus (9090)      ← scrape a cada 15s · retenção 15 dias
         │
         ▼
      Grafana (3001)        ← dashboards: Node Exporter Full + cAdvisor
```

### Stack IoT

```
ESP32 / Sensores / PC
    │  MQTT (1883)
    ▼
Mosquitto
    │
    ├── Home Assistant     ← automação, dashboards, alertas
    ├── pc-monitor-webapp  ← dashboard de hardware em tempo real
    └── metrics-api        ← API REST para consumo externo
```

---

## Servidor de Jogos — FiveM

Servidor de roleplay (ESX Legacy 1.13.5) acessível via Tailscale.

| Item | Valor |
|------|-------|
| Framework | ESX Legacy 1.13.5 |
| Porta | `30120` |
| Acesso | `100.69.59.102:30120` (Tailscale) |
| Banco de dados | MariaDB — banco `fivem` |
| Diretório | `/root/FXServer` |

---

## Estrutura do Repositório

```
home-stack/
├── README.md                    ← este arquivo
├── servidor-principal/
│   ├── docker/                  ← docker-compose de todos os serviços
│   │   ├── observability/       ← Prometheus + Grafana + cAdvisor + Node Exporter
│   │   ├── nextcloud/
│   │   ├── homeassistant/
│   │   ├── mosquitto/
│   │   ├── open-webui/
│   │   ├── watchtower/
│   │   └── ...
│   ├── pihole/
│   ├── unbound/
│   ├── tailscale/
│   └── ftp/
├── orange-pi/                   ← DNS secundário
└── rede/                        ← topologia e configurações de rede
```

---

## Segurança

- Senhas e tokens **nunca** commitados — use `.env` (ver `.env.example` em cada serviço)
- Acesso externo exclusivamente via **Tailscale** — nenhuma porta exposta na internet
- DNS interno resolvido localmente pelo Unbound — sem vazamento para resolvers externos
- TLS em todos os serviços `.lan` via CA própria (mkcert)
