# Tailscale — VPN Mesh

Instalação em bare metal. Conecta o servidor principal, Orange Pi e dispositivos remotos numa rede privada segura.

---

## Instalação

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

Ou via repositório oficial:

```bash
curl -fsSL https://pkgs.tailscale.com/stable/debian/bookworm.noarmor.gpg \
  | tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null

curl -fsSL https://pkgs.tailscale.com/stable/debian/bookworm.tailscale-list \
  | tee /etc/apt/sources.list.d/tailscale.list

apt update && apt install tailscale -y
```

---

## Configuração inicial

### Autenticar e subir na rede

```bash
tailscale up --authkey=<SEU_AUTH_KEY>
```

> Gere auth keys em: https://login.tailscale.com/admin/settings/keys
> Prefira **reusable + pre-approved** keys para automação.

### Subir com opções recomendadas

```bash
tailscale up \
  --authkey=<SEU_AUTH_KEY> \
  --advertise-exit-node \
  --accept-dns=false \
  --hostname=servidor-principal
```

| Flag | Descrição |
|------|-----------|
| `--advertise-exit-node` | Permite usar este servidor como saída de tráfego |
| `--accept-dns=false` | Não sobrescrever DNS local (Pi-hole já gerencia) |
| `--hostname` | Nome do nó na rede Tailscale |

---

## Anunciar o DNS do Pi-hole para a rede Tailscale

No painel do Tailscale (https://login.tailscale.com/admin/dns):

1. **Global nameservers** → Add nameserver → Custom → IP Tailscale do servidor (`100.x.x.x`)
2. Marcar **Override local DNS** se quiser que todos os dispositivos Tailscale usem o Pi-hole

```bash
# Verificar IP Tailscale do servidor
tailscale ip -4
```

---

## Habilitar IP Forwarding (para exit node)

```bash
echo 'net.ipv4.ip_forward = 1' | tee -a /etc/sysctl.d/99-tailscale.conf
echo 'net.ipv6.conf.all.forwarding = 1' | tee -a /etc/sysctl.d/99-tailscale.conf
sysctl -p /etc/sysctl.d/99-tailscale.conf
```

### Aprovar exit node no painel

Acessar https://login.tailscale.com/admin/machines → máquina → Edit route settings → Exit Node → Approve.

---

## Habilitar Subnet Router (expor rede local)

Para acessar dispositivos da rede local (que não têm Tailscale instalado) via VPN:

```bash
tailscale up \
  --advertise-routes=192.168.1.0/24 \
  --accept-dns=false
```

Aprovar no painel: máquina → Edit route settings → Subnet routes → Approve.

---

## Comandos úteis

```bash
# Status da conexão
tailscale status

# IP na rede Tailscale
tailscale ip

# Ping em outro nó
tailscale ping <hostname>

# Ver logs
journalctl -u tailscaled -f

# Desconectar (mantém instalado)
tailscale down

# Reconectar
tailscale up
```

---

## Chave SSH via Tailscale (MagicSSH)

Habilitar no painel: Settings → SSH → Enable Tailscale SSH.

```bash
# Após habilitar, SSH direto pelo nome do nó
ssh root@servidor-principal
```

---

## Inicialização automática

```bash
systemctl enable tailscaled
systemctl start tailscaled
```

---

## Troubleshooting

### Não conecta

```bash
# Ver estado detalhado
tailscale status --json | jq

# Logs do daemon
journalctl -u tailscaled --since "5 minutes ago"
```

### Perda de conectividade após reboot

```bash
# Verificar se o serviço subiu
systemctl status tailscaled

# Re-autenticar se necessário
tailscale up --authkey=<SEU_AUTH_KEY>
```
