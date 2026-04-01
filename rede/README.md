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

Os domínios `.lan` são resolvidos pelo Pi-hole via registros DNS locais, apontando para o Nginx Proxy Manager que distribui para cada serviço:

| Domínio | Serviço |
|---------|---------|
| `dns.lan` | Pi-hole |
| `grafana.lan` | Grafana |
| `metrics.lan` | Prometheus |
| `portainer.lan` | Portainer |
| `proxy.lan` | Nginx Proxy Manager |
| `torrent.lan` | qBittorrent |

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
