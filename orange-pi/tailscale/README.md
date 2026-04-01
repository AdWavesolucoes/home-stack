# Tailscale — Orange Pi

Instalação idêntica ao servidor principal. Conecta o Orange Pi à rede Tailscale para acesso e gerenciamento remoto.

---

## Instalação

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

---

## Configuração

```bash
tailscale up \
  --authkey=<SEU_AUTH_KEY> \
  --accept-dns=false \
  --hostname=orange-pi-dns
```

> `--accept-dns=false` é importante para não sobrescrever o DNS local gerenciado pelo Pi-hole.

---

## Inicialização automática

```bash
systemctl enable tailscaled
systemctl start tailscaled
```

---

## Verificar conectividade

```bash
# Ver IPs da rede Tailscale
tailscale status

# Ping no servidor principal via Tailscale
tailscale ping servidor-principal
```

---

## Uso com Gravity Sync

O Gravity Sync usa SSH para sincronizar entre os nós. Você pode usar o IP Tailscale como endereço do servidor principal na configuração do Gravity Sync — isso garante que a sincronização funcione mesmo fora da rede local.

Ver: [Gravity Sync](../gravity-sync/README.md)
