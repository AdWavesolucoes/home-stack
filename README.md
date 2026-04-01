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
  Docker (9 containers)
  FTP (vsftpd)
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
| **Docker** | — | Orquestração dos demais serviços |

### Containers Docker

| Container | Imagem | Porta | Status | Descrição |
|-----------|--------|-------|--------|-----------|
| `cadvisor` | `gcr.io/cadvisor/cadvisor` | 8080 | Ativo | Métricas dos containers |
| `grafana` | `grafana/grafana` | 3000 | Ativo | Dashboards e visualização |
| `node-exporter` | `prom/node-exporter` | — | Ativo | Métricas do host |
| `npm` | `jc21/nginx-proxy-manager` | 80/443/81 | Ativo | Proxy reverso |
| `portainer` | `portainer/portainer-ce` | 9000/9443 | Ativo | Gestão de containers |
| `prometheus` | `prom/prometheus` | 9090 | Ativo | Coleta de métricas |
| `watchtower` | `containrrr/watchtower` | — | Ativo | Atualização automática de imagens |
| `plex` | `lscr.io/linuxserver/plex` | — | Pausado | Servidor de mídia |
| `qbittorrent` | `lscr.io/linuxserver/qbittorrent` | 8081 | Pausado | Cliente BitTorrent |

---

## Estrutura do Repositório

```
home-stack/
├── README.md
├── servidor-principal/
│   ├── README.md
│   ├── pihole/
│   │   └── README.md
│   ├── unbound/
│   │   ├── README.md
│   │   └── pi-hole.conf
│   ├── tailscale/
│   │   └── README.md
│   ├── ftp/
│   │   └── README.md
│   └── docker/
│       ├── README.md
│       ├── cadvisor/
│       ├── grafana/
│       ├── node-exporter/
│       ├── npm/
│       ├── plex/
│       ├── portainer/
│       ├── prometheus/
│       ├── qbittorrent/
│       └── watchtower/
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
- Serviços críticos (Pi-hole, Unbound, Tailscale, FTP) rodam em bare metal para maior estabilidade e disponibilidade
