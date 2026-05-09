# Infraestrutura do Servidor — Mapeamento Completo

> **Gerado em:** 2026-05-09  
> **Hostname:** DietPi  
> **IP local:** 192.168.0.100

---

## Sistema Operacional

| Campo | Valor |
|---|---|
| OS | DietPi (baseado em Debian GNU/Linux 13 "Trixie") |
| Kernel | 6.12.85+deb13-amd64 |
| Arquitetura | x86_64 (64 bits) |
| Hostname | DietPi |

---

## Hardware

### Placa-Mãe

| Campo | Valor |
|---|---|
| Fabricante | Positivo Informática SA |
| Modelo | POS-PIH81DI |
| Chipset | Intel H81 |
| Socket | LGA 1150 |
| BIOS | v0107 — 08/12/2014 |
| Slots de memória | 2 (ambos ocupados) |
| Capacidade máxima | 16 GB |

### Processador

| Campo | Valor |
|---|---|
| Modelo | Intel Core i7-4770 (Haswell) |
| Clock base | 3.40 GHz |
| Clock turbo | 3.80 GHz |
| Núcleos | 4 (8 threads com HT) |
| Cache L3 | 8 MB |

### Memória RAM

| Slot | Fabricante | Módulo | Capacidade | Tipo | Velocidade |
|---|---|---|---|---|---|
| ChannelA-DIMM0 (BANK 0) | Kingston | 99U5471-054.A00LF | 8 GB | DDR3 DIMM | 1600 MT/s |
| ChannelB-DIMM0 (BANK 2) | Kingston | 99U5471-056.A00LF | 8 GB | DDR3 DIMM | 1600 MT/s |

**Total:** 16 GB DDR3 @ 1600 MT/s — Dual Channel  
**Swap:** 2 GB  
**Uso atual:** ~3.7 GB usados de 15.6 GB disponíveis

---

## Armazenamento

| Dispositivo | Modelo/Tipo | Tamanho | FS | Ponto de Montagem | Uso | Observação |
|---|---|---|---|---|---|---|
| `/dev/sdb` | YS SSD | 238.5 GB | — | — | — | Disco do sistema (OS) |
| `└─ sdb1` | EFI | 64 MB | vfat | `/boot/efi` | 14% | Partição EFI |
| `└─ sdb2` | Sistema | 238.4 GB | ext4 | `/` | 38% (84GB/234GB) | Raiz do sistema |
| `/dev/sda` | Netac SSD 512GB | 476.9 GB | — | — | — | Armazenamento secundário |
| `└─ sda1` | Dados NTFS | 363.2 GB | ntfs | `/mnt/B0CEF741CEF6FE82` | 98% (356GB) | Mídia/torrents |
| `└─ sda2` | RAID member | 93.1 GB | — | (membro `/dev/md0`) | — | Partição RAID 1 |
| `└─ md0` | RAID 1 | 92 GB | ext4 | `/mnt/nextcloud-raid` | 1% | Dados Nextcloud |
| `/dev/sdc` | Seagate 500GB HDD | 465.8 GB | — | — | — | ⚠️ Possíveis bad sectors |
| `└─ sdc2` | Dados NTFS | 465.7 GB | ntfs | `/mnt/B8B615FAB615BA38` | 53% (245GB) | Filmes/séries |

### RAID

| Campo | Valor |
|---|---|
| Dispositivo | `/dev/md0` |
| Tipo | RAID 1 (espelhamento) |
| Membros | `/dev/sda2` (Netac SSD) |
| Tamanho útil | ~92 GB |
| Sistema de arquivos | ext4 |
| Ponto de montagem | `/mnt/nextcloud-raid` |
| Monitor | `mdmonitor.service` (systemd) |

---

## Rede

### Interfaces

| Interface | Endereço IP | Descrição |
|---|---|---|
| `eth0` | `192.168.0.100/24` | LAN local (Ethernet) |
| `tailscale0` | `100.69.59.102/32` | VPN Tailscale (IPv4) |
| `tailscale0` | `fd7a:115c:a1e0::5a36:3b66/128` | VPN Tailscale (IPv6) |
| `docker0` | `172.17.0.1/16` | Bridge Docker padrão |
| `docker_gwbridge` | `172.18.0.1/16` | Bridge Docker Swarm |
| `br-0a55fcd8755a` | `172.19.0.1/16` | npm_default (NPM) |
| `br-2e24a67ccc9d` | `172.22.0.1/16` | nextcloud_net |
| `br-efdcd4274d39` | `172.24.0.1/16` | monitor-net |
| `br-0e52c436c16c` | `172.23.0.1/16` | mosquitto_default |
| `br-9a722b79a682` | `172.25.0.1/16` | metrics-api_default |

**Gateway:** `192.168.0.1`  
**MAC (eth0):** `d8:97:ba:4d:42:31`

### Dispositivos na rede Tailscale

| Hostname | IP Tailscale | SO | Status |
|---|---|---|---|
| servidorcasa | 100.69.59.102 | Linux | Online |
| desktop-fibaaq7 | 100.127.178.97 | Windows | Visto há ~9h |
| desktop-lucas | 100.78.134.77 | Windows | Visto há ~24d |
| laptop-thassio | 100.100.159.22 | Windows | Visto há ~2d |
| poco-x7 | 100.71.92.86 | Android | Visto há ~1d |
| tab-s10-lite | 100.72.176.9 | Android | Visto há ~32d |

### Portas em escuta (principais)

| Porta | Protocolo | Serviço | Processo |
|---|---|---|---|
| 21 | TCP | FTP | vsftpd |
| 22 | TCP | SSH | Dropbear |
| 53 | TCP/UDP | DNS | Pi-hole FTL |
| 80 | TCP | HTTP | Nginx Proxy Manager |
| 81 | TCP | NPM Admin | Nginx Proxy Manager |
| 139 / 445 | TCP | SMB | Samba |
| 443 | TCP | HTTPS | Nginx Proxy Manager |
| 1883 | TCP | MQTT | Mosquitto |
| 3000 | TCP | PC Monitor WebApp | Docker |
| 4443 | TCP | Pi-hole HTTPS | Pi-hole FTL |
| 5000 | TCP | Metrics API ESP32 | Docker |
| 8082 | TCP | Pi-hole Admin | Pi-hole FTL |
| 8085 | TCP | Open WebUI | Docker |
| 8123 | TCP | Home Assistant | Docker (host mode) |
| 8888 | TCP | Nextcloud | Docker |
| 9000 | TCP | Portainer HTTP | Docker |
| 9001 | TCP | MQTT WebSocket | Mosquitto |
| 9443 | TCP | Portainer HTTPS | Docker |
| 11434 | TCP | Ollama API | Ollama (systemd) |
| 32400 | TCP | Plex Media Server | Docker (host mode) |

---

## Diagrama de Arquitetura

```
Internet
    │
    └── Nginx Proxy Manager (80/443)
            ├── nextcloud.lan  → :8888  (Nextcloud)
            ├── portainer.lan  → :9443  (Portainer)
            ├── aichat.lan     → :8085  (Open WebUI)
            ├── proxy.lan      → :81    (NPM Admin)
            ├── dns.lan        → :8082  (Pi-hole)
            ├── home.lan       → :3005  (Homepage)
            ├── plex.lan       → :32400 (Plex)
            ├── torrent.lan    → :8081  (qBittorrent)
            ├── traccar.lan    → :8083  (Traccar)
            └── ...outros

Rede LAN (192.168.0.0/24)
    ├── Pi-hole DNS (:53)
    │       └── Unbound recursivo (:5335)
    ├── SSH Dropbear (:22)
    ├── FTP vsftpd (:21)
    ├── Samba SMB (:445) → raiz do servidor
    ├── Tailscale VPN (100.69.59.102)
    ├── MQTT Mosquitto (:1883/:9001)
    ├── Home Assistant (:8123, host mode)
    ├── Ollama API (:11434)
    ├── Telegram Bot (systemd, saída)
    ├── Metrics API ESP32 (:5000)
    └── PC Monitor WebApp (:3000)

Armazenamento
    ├── /dev/sdb2 (238GB SSD) → / (sistema)
    ├── /dev/sda1 (363GB SSD) → mídia/torrents
    ├── /dev/sdc2 (466GB HDD) → filmes/séries ⚠️
    └── /dev/md0  (RAID1 92GB) → Nextcloud data
```

---

## Domínios .lan (via Pi-hole + Nginx PM)

| Domínio | Serviço | Porta interna |
|---|---|---|
| `aichat.lan` | Open WebUI | 8085 |
| `nextcloud.lan` | Nextcloud | 8888 |
| `portainer.lan` | Portainer | 9443 |
| `proxy.lan` | NPM Admin | 81 |
| `dns.lan` | Pi-hole Admin | 8082 |
| `home.lan` | Homepage | 3005 |
| `plex.lan` | Plex | 32400 |
| `torrent.lan` | qBittorrent | 8081 |
| `traccar.lan` | Traccar | 8083 |
| `grafana.lan` | Grafana | (volumes existem) |
| `prometheus.lan` | Prometheus | (volumes existem) |

---

## Compartilhamento Samba

| Campo | Valor |
|---|---|
| Share | `\\192.168.0.100\servidor` |
| Caminho | `/` (raiz completa do servidor) |
| Acesso | Leitura e escrita |
| Protocolo | SMB/CIFS |

**Windows:** `\\192.168.0.100\servidor`  
**Linux/Mac:** `smb://192.168.0.100/servidor`

---

## Cron Jobs

| Schedule | Script | Descrição |
|---|---|---|
| `47 5 4 5 *` | `/tmp/check_ha_ram.sh` | Verificação temporária de RAM do HA |
| `0 */12 * * *` | `/usr/local/bin/pihole-monitor.sh` | Monitor Pi-hole a cada 12h |
