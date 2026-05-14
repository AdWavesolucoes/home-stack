# Rede

Visão geral da estrutura de rede interna.

---

## Topologia

```
Internet
    │
    ▼
[ Roteador ]  192.168.0.1
    │
    └──── LAN 192.168.0.0/24
              │
              ├── 192.168.0.100   Servidor Principal (DietPi x86_64)
              │                    Pi-hole, Unbound, Tailscale, Docker
              │
              └── Orange Pi Zero 2W  (IP a definir)
                                   DNS Secundário (em configuração)
```

---

## DNS

| Papel | Endereço | Serviço |
|-------|----------|---------|
| DNS Primário | `192.168.0.100:53` | Pi-hole |
| DNS Secundário | IP Orange Pi `:53` | Pi-hole (em configuração) |
| Resolver recursivo | `127.0.0.1:5335` | Unbound (local, não exposto) |

---

## Domínios Internos (.lan)

Os domínios `.lan` são resolvidos pelo Pi-hole via registros DNS locais e roteados pelo Nginx Proxy Manager para cada serviço:

| Domínio | Serviço | Porta interna |
|---------|---------|---------------|
| `aichat.lan` | Open WebUI (Ollama) | `8085` |
| `adwavepaper.lan` | AdWave Landing Page | `8091` |
| `casa.lan` | Home Assistant | `8123` |
| `dns.lan` | Pi-hole | `8082` |
| `evolution.lan` | Evolution API | `8080` |
| `grafana.lan` | Grafana | `3001` |
| `mesh.lan` | Nginx Proxy Manager (HTTPS) | `443` |
| `n8n.lan` | n8n | `5678` |
| `nextcloud.lan` | Nextcloud | `8888` |
| `plex.lan` | Plex | `32400` |
| `portainer.lan` | Portainer | `9000` |
| `proxy.lan` | NPM — painel admin | `81` |

---

## TLS Interno

Os certificados `.lan` são gerados com **mkcert** usando uma CA raiz própria (`rootCA.cer`). Para que os navegadores confiem nos certificados, instale o `rootCA.cer` nos dispositivos da rede.

---

## VPN

O **Tailscale** conecta todos os dispositivos em uma rede mesh (`100.x.x.x`), permitindo acesso remoto seguro a todos os serviços sem abrir portas no roteador.

| Dispositivo | IP Tailscale |
|-------------|-------------|
| servidorcasa | `100.69.59.102` |
| desktop-lucas | `100.78.134.77` |
| poco-x7 | `100.71.92.86` |
| tab-s10-lite | `100.72.176.9` |
