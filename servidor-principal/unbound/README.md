# Unbound — Resolver DNS Recursivo

Instalação em bare metal. Resolve DNS diretamente nos servidores raiz sem depender de terceiros (Cloudflare, Google, etc.).

---

## Como funciona

```
Pi-hole :53
    │
    ▼ (encaminha para)
Unbound :5335
    │
    ▼ (resolve diretamente)
Servidores Raiz DNS (.)
```

---

## Instalação

```bash
apt install unbound -y
```

---

## Configuração

Criar o arquivo de configuração do Pi-hole:

```bash
nano /etc/unbound/unbound.conf.d/pi-hole.conf
```

Cole o conteúdo do arquivo [`pi-hole.conf`](./pi-hole.conf) deste repositório.

---

## Iniciar e habilitar

```bash
systemctl enable unbound
systemctl start unbound
```

### Testar se está funcionando

```bash
# Deve retornar resposta com SERVFAIL não ocorrendo
dig pi-hole.net @127.0.0.1 -p 5335

# Teste de validação DNSSEC (deve retornar SERVFAIL — sinal que DNSSEC funciona)
dig sigfail.verteiltesysteme.net @127.0.0.1 -p 5335

# Teste de domínio válido com DNSSEC (deve retornar NOERROR)
dig sigok.verteiltesysteme.net @127.0.0.1 -p 5335
```

---

## Evitar conflito com systemd-resolved

```bash
systemctl disable systemd-resolved
systemctl stop systemd-resolved
```

Se `/etc/resolv.conf` for um symlink para o resolved:

```bash
rm /etc/resolv.conf
echo "nameserver 127.0.0.1" > /etc/resolv.conf
```

---

## Apontar Pi-hole para Unbound

Após instalar e testar o Unbound:

```bash
pihole -a setdns 127.0.0.1#5335
pihole restartdns
```

---

## Comandos úteis

```bash
# Status
systemctl status unbound

# Verificar config (sem reiniciar)
unbound-checkconf

# Reiniciar
systemctl restart unbound

# Ver logs
journalctl -u unbound -f

# Estatísticas
unbound-control stats_noreset
```

---

## Troubleshooting

### Porta 53 já em uso

```bash
# Verificar quem está usando a porta 53
ss -tlnp | grep :53

# Se for o systemd-resolved:
systemctl disable systemd-resolved --now
```

### Unbound não resolve

```bash
# Testar diretamente
dig google.com @127.0.0.1 -p 5335

# Ver erros
journalctl -u unbound --since "10 minutes ago"
```

### Permissões do arquivo de config

```bash
chown root:root /etc/unbound/unbound.conf.d/pi-hole.conf
chmod 644 /etc/unbound/unbound.conf.d/pi-hole.conf
```
