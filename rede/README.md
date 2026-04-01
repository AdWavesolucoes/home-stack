# Rede

Documentação da configuração de rede da homelab.

---

## Mapa de IPs

| Dispositivo | IP Local | IP Tailscale | Função |
|-------------|----------|--------------|--------|
| Roteador | 192.168.1.1 | — | Gateway |
| Servidor Principal | 192.168.1.10 | 100.x.x.x | DNS primário, Docker, Proxy |
| Orange Pi Zero 2W | 192.168.1.11 | 100.x.x.x | DNS secundário |

> Atualizar com seus IPs reais.

---

## Fluxo DNS

```
Dispositivos da rede local
        │
        ▼
   Roteador DHCP
   DNS: 192.168.1.10 (primário)
         192.168.1.11 (secundário)
        │
        ▼
   Pi-hole :53 (192.168.1.10)
   ├── Domínio bloqueado? → NXDOMAIN
   └── Domínio permitido?
           │
           ▼
      Unbound :5335 (127.0.0.1)
           │
           ▼
      Servidores Raiz DNS
      (resolução recursiva)
```

---

## Fluxo de tráfego HTTP/HTTPS

```
Dispositivo
    │
    ▼
Nginx Proxy Manager :80/:443 (192.168.1.10)
    │
    ├── grafana.home     → grafana:3000
    ├── portainer.home   → portainer:9000
    ├── prometheus.home  → prometheus:9090
    ├── pihole.home      → localhost:80 (lighttpd)
    ├── homepage.home    → homepage:3000
    ├── navidrome.home   → navidrome:4533
    ├── qbittorrent.home → qbittorrent:8090
    └── ...
```

---

## Acesso remoto via Tailscale

```
Internet
    │
    ▼
Tailscale (100.x.x.x)
    │
    ├── Acesso SSH ao servidor principal
    ├── Acesso SSH ao Orange Pi
    ├── DNS do Pi-hole disponível remotamente
    └── Acesso a todos os serviços via IP Tailscale
```

Para usar o Pi-hole remotamente via Tailscale, configurar no painel do Tailscale:
- DNS → Global nameservers → IP Tailscale do servidor principal

---

## Configuração do roteador

### DNS para dispositivos da rede

Na configuração DHCP do roteador (ou do Pi-hole se for o servidor DHCP):

```
DNS 1: 192.168.1.10
DNS 2: 192.168.1.11
```

### Regras de firewall recomendadas

Portas que podem precisar ser abertas dependendo do uso:

| Porta | Protocolo | Serviço | Necessário? |
|-------|-----------|---------|-------------|
| 80 | TCP | HTTP (NPM) | Se acessar de fora |
| 443 | TCP | HTTPS (NPM) | Se acessar de fora |
| 53 | TCP/UDP | DNS | Não expor à internet |
| 25565 | TCP | Minecraft | Se servidor público |
| 6881 | TCP/UDP | qBittorrent | Para seeds |
| 41641 | UDP | Tailscale | Recomendado |

> Regra geral: não expor serviços diretamente. Use o Tailscale para acesso remoto.

---

## Certificados SSL locais (NPM)

Para HTTPS nos domínios `.home` sem CA pública:

### Opção 1: Let's Encrypt com DNS Challenge

Usar um domínio real com DNS challenge (sem abrir portas):

1. Comprar domínio (ex: `meudominio.com.br`)
2. No NPM: SSL Certificates → Add → Let's Encrypt → DNS Challenge
3. Configurar registros A no DNS público apontando para IPs internos
4. Criar wildcard cert: `*.home.meudominio.com.br`

### Opção 2: Self-signed + CA local

```bash
# Criar CA local
openssl genrsa -out ca.key 4096
openssl req -new -x509 -days 3650 -key ca.key -out ca.crt \
  -subj "/CN=Homelab CA/O=Homelab/C=BR"

# Instalar CA nos dispositivos da rede para confiança automática
```

---

## Monitoramento de rede

O Prometheus + Grafana coleta:
- Métricas do host (CPU, RAM, disco, rede) via Node Exporter
- Métricas de containers via cAdvisor
- Uptime e latência de serviços

Dashboards recomendados no Grafana:
- **1860** — Node Exporter Full
- **893** — Docker and system monitoring
- **14900** — Pi-hole Exporter
