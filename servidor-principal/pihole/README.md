# Pi-hole — DNS Primário

Instalação em bare metal (sem Docker) para garantir disponibilidade máxima.

---

## Instalação

```bash
curl -sSL https://install.pi-hole.net | bash
```

Durante o instalador:
- Interface: `eth0`
- Upstream DNS: `127.0.0.1#5335` (Unbound — configurar depois)
- Blocklists: manter padrão (adicionar mais depois)
- Instalar web admin: **Sim**
- Instalar lighttpd: **Sim**
- Habilitar logging: **Sim**

> Instale o Unbound **antes** de apontar o Pi-hole para `127.0.0.1#5335`.
> Na instalação inicial use `1.1.1.1` como upstream e troque depois.

---

## Pós-instalação

### Definir senha do painel

```bash
pihole -a -p
```

### Apontar para Unbound

Após instalar o Unbound:

```bash
# Editar upstream DNS
pihole -a setdns 127.0.0.1#5335

# Ou via arquivo de config:
nano /etc/pihole/setupVars.conf
```

```ini
PIHOLE_DNS_1=127.0.0.1#5335
PIHOLE_DNS_2=
DNS_FQDN_REQUIRED=true
DNS_BOGUS_PRIV=true
DNSSEC=false
```

> DNSSEC fica desabilitado aqui porque o Unbound já faz a validação DNSSEC.

### Reiniciar serviços

```bash
pihole restartdns
```

---

## Configuração do DHCP

Se quiser usar o Pi-hole como servidor DHCP (recomendado para garantir que todos os dispositivos usem o Pi-hole como DNS):

1. **Desabilitar DHCP no roteador** primeiro
2. Acessar o painel web → Settings → DHCP → Enable

```ini
# Em setupVars.conf
DHCP_ACTIVE=true
DHCP_START=192.168.1.100
DHCP_END=192.168.1.200
DHCP_ROUTER=192.168.1.1
DHCP_LEASETIME=24
PIHOLE_DOMAIN=lan
DHCP_IPv6=false
```

---

## Blocklists recomendadas

Adicionar em Settings → Blocklists:

```
https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts
https://raw.githubusercontent.com/PolishFiltersTeam/KADhosts/master/KADhosts.txt
https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Spam/hosts
https://v.firebog.net/hosts/static/w3kbl.txt
https://adaway.org/hosts.txt
https://v.firebog.net/hosts/AdguardDNS.txt
https://v.firebog.net/hosts/Admiral.txt
https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt
https://v.firebog.net/hosts/Easylist.txt
https://pgl.yoyo.org/adservers/serverlist.php?hostformat=hosts&showintro=0&mimetype=plaintext
https://raw.githubusercontent.com/FadeMind/hosts.extras/master/UncheckyAds/hosts
https://raw.githubusercontent.com/bigdargon/hostsVN/master/hosts
```

Após adicionar, atualizar gravity:

```bash
pihole -g
```

---

## DNS Local (registros customizados)

Para acessar serviços pelo nome (ex: `grafana.home`):

```bash
nano /etc/pihole/custom.list
```

```
192.168.1.10  servidor.home
192.168.1.10  grafana.home
192.168.1.10  portainer.home
192.168.1.10  prometheus.home
192.168.1.10  pihole.home
192.168.1.10  homepage.home
192.168.1.10  navidrome.home
```

```bash
pihole restartdns
```

---

## Comandos úteis

```bash
# Status
pihole status

# Atualizar Pi-hole
pihole -up

# Atualizar listas de bloqueio
pihole -g

# Ver logs em tempo real
pihole -t

# Whitelist de domínio
pihole -w dominio.com

# Blacklist manual
pihole -b dominio.com

# Desabilitar por X segundos
pihole disable 300

# Reabilitar
pihole enable
```

---

## Backup

```bash
# Exportar configurações (teleporter)
pihole -a -t

# O arquivo .tar.gz gerado contém:
# - Listas de bloqueio/whitelist
# - Configurações DNS
# - Grupos e clientes
# - Regex filters
```

Para restaurar: Settings → Teleporter → Restore.

---

## Próximo passo

Instalar o [Unbound](../unbound/README.md) para resolver DNS recursivamente.
