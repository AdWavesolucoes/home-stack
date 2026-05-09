# home-stack

Documentação da infraestrutura doméstica — DNS, VPN, proxy reverso, monitoramento e serviços de mídia rodando em bare metal e Docker.

---

## Visão Geral da Arquitetura

```
Internet
    │
    ▼
[ Roteador ]
    │
    ├─────────────────────────────────────┐
    ▼                                     ▼
[ Servidor Principal ]           [ Orange Pi Zero 2W ]
  DietPi (Debian 13)               DietPi
  DNS Primário                     DNS Secundário (em configuração)
  Pi-hole + Unbound                Pi-hole + Unbound
  Tailscale                        Tailscale
  Docker (10+ containers)
  FTP (vsftpd)
  Samba
  Ollama (LLM local)
```

### Fluxo DNS

```
Dispositivos da rede
        │
        ▼
   Pi-hole :53          ← bloqueia anúncios e trackers
        │
        ▼
   Unbound :5335        ← resolve recursivamente sem terceiros
        │
        ▼
  Servidores Raiz DNS   ← resolução privada e completa
```

---

## Servidor Principal

| Componente | Tipo | Função |
|------------|------|--------|
| **Pi-hole** | bare metal | DNS primário + bloqueio de anúncios |
| **Unbound** | bare metal | Resolver DNS recursivo (porta 5335) |
| **Tailscale** | bare metal | VPN mesh para acesso remoto |
| **vsftpd** | bare metal | Servidor FTP local |
| **Samba** | bare metal | Compartilhamento de arquivos (SMB) |
| **Ollama** | bare metal | Servidor LLM local (porta 11434) |
| **Docker** | — | Orquestração dos demais serviços |

### Containers Docker Ativos

| Container | Imagem | Porta | Descrição |
|-----------|--------|-------|-----------|
| `open-webui` | `ghcr.io/open-webui/open-webui` | 8085 | Interface web para LLMs |
| `nextcloud` | `nextcloud:latest` | 8888 | Armazenamento em nuvem privada |
| `nextcloud-db` | `mariadb:11` | interno | Banco de dados Nextcloud |
| `homeassistant` | `ghcr.io/home-assistant/home-assistant` | 8123 (host) | Automação residencial |
| `npm` | `jc21/nginx-proxy-manager` | 80/443/81 | Proxy reverso |
| `portainer` | `portainer/portainer-ce` | 9000/9443 | Gestão de containers |
| `mosquitto` | `eclipse-mosquitto:2` | 1883/9001 | Broker MQTT |
| `metrics-api` | build local | 5000 | API de métricas para ESP32 |
| `pc-monitor-webapp` | build local | 3000 | Dashboard de monitoramento via MQTT |
| `plex` | `lscr.io/linuxserver/plex` | 32400 (host) | Servidor de mídia |

### Stacks Docker Compose

| Stack | Localização | Serviços | Status |
|-------|-------------|----------|--------|
| **n8n-stack** | `/root/n8n-stack/` | n8n, n8n-worker, Evolution API, PostgreSQL, Redis | Ativo |
| Nextcloud | `/opt/nextcloud/` | nextcloud, mariadb | Ativo |
| Home Assistant | `/opt/stacks/homeassistant/` | homeassistant | Ativo |
| Watchtower | `/opt/stacks/watchtower/` | watchtower | Ativo |
| Mosquitto | `/root/docker/mosquitto/` | mosquitto | Ativo |
| Plex | `/opt/plex/` | plex | Ativo |
| stack-automacao | `/root/stack-automacao/` | n8n, evolution, postgres, redis | Legada/testes |
| qbittorrent | `/root/stacks/qbittorrent/` | qbittorrent | Parado |

### Containers Parados / Stacks Inativas

`qbittorrent`, `paperclip`, `homepage`, `traccar`, `kiwix`, `spotify-embed`, `mkcert-web`, `code-server`, `minecraft`, `prometheus`, `grafana`

---

## Estrutura do Repositório

```
home-stack/
├── README.md
├── servidor-principal/
│   ├── README.md          ← resumo, acesso rápido, segurança
│   ├── INFRA.md           ← hardware, rede, armazenamento, diagrama
│   ├── SERVICOS.md        ← todos os serviços, containers, stacks
│   ├── pihole/
│   ├── unbound/
│   ├── tailscale/
│   ├── ftp/
│   └── docker/
│       ├── npm/
│       ├── plex/
│       ├── portainer/
│       ├── watchtower/
│       └── ...
├── orange-pi/
│   └── README.md
└── rede/
    └── README.md
```

---

## Notas

- Toda a documentação está em português
- Senhas e dados sensíveis **nunca** são commitados
- Use `.env` para variáveis de ambiente e adicione ao `.gitignore`
- Serviços críticos (Pi-hole, Unbound, Tailscale, FTP, Samba, Ollama) rodam em bare metal para maior estabilidade
- Ver [`servidor-principal/SERVICOS.md`](servidor-principal/SERVICOS.md) para lista completa de serviços
- Ver [`servidor-principal/INFRA.md`](servidor-principal/INFRA.md) para detalhes de hardware e rede
