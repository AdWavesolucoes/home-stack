# Pi-hole

Servidor DNS primário da rede com bloqueio de anúncios e trackers.

---

## Instalação

```bash
curl -sSL https://install.pi-hole.net | bash
```

## Configuração

O Pi-hole é configurado via `/etc/pihole/pihole.toml`.

### Upstream DNS

O Pi-hole não usa servidores DNS externos. Todo o tráfego DNS é encaminhado para o **Unbound** local na porta 5335:

```toml
[dns]
  upstreams = ["127.0.0.1#5335"]
```

Isso garante resolução DNS privada e completa sem depender de terceiros (Google, Cloudflare, etc.).

### Opções relevantes habilitadas

| Opção | Valor | Descrição |
|-------|-------|-----------|
| `CNAMEdeepInspect` | `true` | Inspeciona CNAMEs para bloquear rastreadores que usam redirecionamentos |
| `blockESNI` | `true` | Bloqueia `_esni.` subdomínios para evitar bypass de bloqueio |
| `EDNS0ECS` | `true` | Obtém IP real do cliente mesmo atrás de NAT |
| `showDNSSEC` | `true` | Exibe queries DNSSEC no log |
| `interface` | `eth0` | Escuta apenas na interface de rede principal |

### Interface Web

Acessível em `http://dns.lan` ou `http://<IP>` (porta 80/443).

---

## Domínios Locais (.lan)

Os domínios internos são resolvidos via registros locais no Pi-hole (DNS Records), apontando para o Nginx Proxy Manager que distribui o tráfego para cada serviço.
