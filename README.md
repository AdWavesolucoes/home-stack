# 🏠 home-stack

Documentação da minha infraestrutura doméstica — servidores, containers, DNS, VPN e tudo que mantém a casa funcionando.

---

## 📐 Visão Geral da Arquitetura

```
Internet
    │
    ▼
[ Roteador ]
    │
    ├──────────────────────────────────┐
    ▼                                  ▼
[ Servidor Principal ]         [ Orange Pi Zero 2W ]
  DNS Primário                   DNS Secundário
  Pi-hole + Unbound              Pi-hole + Unbound
  Tailscale                      Tailscale
  Docker (15+ serviços)          Gravity Sync
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

## 🖥️ Servidor Principal

| Componente | Função |
|------------|--------|
| **Pi-hole** | DNS primário + bloqueio de anúncios |
| **Unbound** | Resolver DNS recursivo (porta 5335) |
| **Tailscale** | VPN mesh para acesso remoto |
| **Docker** | Orquestração de containers |

### 🐳 Containers Docker

| Container | Imagem | Porta | Descrição |
|-----------|--------|-------|-----------|
| `cadvisor` | `gcr.io/cadvisor/cadvisor:latest` | 8080 | Monitoramento de containers |
| `grafana` | `grafana/grafana:latest` | 3000 | Dashboards e visualização |
| `homepage` | `ghcr.io/gethomepage/homepage:latest` | 3005 | Dashboard de serviços |
| `kiwix` | `ghcr.io/kiwix/kiwix-tools:latest` | - | Servidor de conteúdo offline |
| `minecraft` | `itzg/minecraft-server:java8` | - | Servidor Minecraft |
| `mosquitto` | `eclipse-mosquitto:latest` | - | Broker MQTT |
| `navidrome` | `deluan/navidrome:latest` | - | Servidor de música |
| `node-exporter` | `prom/node-exporter:latest` | - | Métricas do sistema |
| `npm` | `jc21/nginx-proxy-manager:latest` | 80/443 | Proxy reverso |
| `plex` | `lscr.io/linuxserver/plex:latest` | - | Servidor de mídia |
| `portainer` | `portainer/portainer-ce:latest` | 9000/9443 | Gestão de containers |
| `postgres` | `postgres:15` | - | Banco de dados |
| `prometheus` | `prom/prometheus:latest` | 9090 | Coleta de métricas |
| `qbittorrent` | `lscr.io/linuxserver/qbittorrent:latest` | - | Cliente BitTorrent |
| `watchtower` | `containrrr/watchtower:1.7.1` | - | Atualização automática de imagens |

---

## 🍊 Orange Pi Zero 2W — DNS Secundário

Funciona como redundância total de DNS. Se o servidor principal ficar indisponível, o Orange Pi assume automaticamente.

| Componente | Função |
|------------|--------|
| **SO** | DietPi |
| **Pi-hole** | DNS secundário + bloqueio de anúncios |
| **Unbound** | Resolver DNS recursivo (porta 5335) |
| **Tailscale** | VPN mesh |
| **Gravity Sync** | Sincronização com o Pi-hole primário |

### Sincronização com Gravity Sync

A sincronização acontece automaticamente do primário → secundário, mantendo em paridade:

- ✅ Listas de bloqueio (Gravity)
- ✅ Whitelist / Blacklist
- ✅ Grupos e clientes
- ✅ Regex filters
- ❌ Configuração do Unbound (local em cada instância)
- ❌ Senha do painel (independente em cada instância)

---

## 🔒 VPN — Tailscale

Ambos os servidores estão conectados via Tailscale, permitindo:

- Acesso remoto seguro a todos os serviços
- DNS do Pi-hole disponível fora de casa
- Comunicação criptografada entre os nós

---

## 📁 Estrutura do Repositório

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
│   └── docker/
│       ├── README.md
│       ├── grafana/
│       ├── homepage/
│       ├── npm/
│       ├── prometheus/
│       └── ...
├── orange-pi/
│   ├── README.md
│   ├── pihole/
│   │   └── README.md
│   ├── unbound/
│   │   ├── README.md
│   │   └── pi-hole.conf
│   ├── tailscale/
│   │   └── README.md
│   └── gravity-sync/
│       └── README.md
└── rede/
    └── README.md
```

---

## 🚀 Stack de Monitoramento

| Serviço | Função |
|---------|--------|
| **Prometheus** | Coleta de métricas |
| **Node Exporter** | Métricas do host |
| **cAdvisor** | Métricas dos containers |
| **Grafana** | Visualização e dashboards |

---

## 📝 Notas

- Toda a documentação está em português
- Senhas e dados sensíveis **nunca** são commitados
- Use `.env` para variáveis de ambiente e adicione ao `.gitignore`

---

*Documentação em constante evolução conforme a infra cresce.*
