# Orange Pi Zero 2W — DNS Secundário

Servidor de baixo consumo rodando DietPi. Atua como DNS secundário e assume automaticamente se o servidor principal ficar indisponível.

---

## Hardware

| Componente | Detalhe |
|------------|---------|
| **Placa** | Orange Pi Zero 2W |
| **RAM** | 1GB (ou 4GB dependendo do modelo) |
| **SO** | DietPi |
| **Armazenamento** | Cartão microSD (16GB+) ou eMMC |

---

## Serviços instalados

| Serviço | Tipo | Descrição |
|---------|------|-----------|
| Pi-hole | Bare metal | DNS secundário + bloqueio de anúncios |
| Unbound | Bare metal | Resolver DNS recursivo (porta 5335) |
| Tailscale | Bare metal | VPN mesh |
| Gravity Sync | Bare metal | Sincronização com Pi-hole principal |

---

## Ordem de instalação

1. [Instalação do DietPi](#1-instalação-do-dietpi)
2. [Configuração inicial](#2-configuração-inicial)
3. [Pi-hole](./pihole/README.md)
4. [Unbound](./unbound/README.md)
5. [Tailscale](./tailscale/README.md)
6. [Gravity Sync](./gravity-sync/README.md)

---

## 1. Instalação do DietPi

### Download da imagem

Acesse [dietpi.com](https://dietpi.com/#download) e baixe a imagem para **Orange Pi Zero 2W**.

### Flash no cartão SD

```bash
sudo dd if=DietPi_OrangePiZero2W-*.img of=/dev/sdX bs=4M status=progress conv=fsync
```

### Pré-configurar antes do boot

Montar a partição boot do cartão e editar `dietpi.txt`:

```ini
AUTO_SETUP_LOCALE=pt_BR.UTF-8
AUTO_SETUP_TIMEZONE=America/Sao_Paulo
AUTO_SETUP_KEYBOARD_LAYOUT=br

# IP estático — use um IP diferente do servidor principal!
AUTO_SETUP_NET_ETHERNET_ENABLED=1
AUTO_SETUP_NET_USESTATIC=1
AUTO_SETUP_NET_STATIC_IP=192.168.1.11
AUTO_SETUP_NET_STATIC_MASK=255.255.255.0
AUTO_SETUP_NET_STATIC_GATEWAY=192.168.1.1
AUTO_SETUP_NET_STATIC_DNS=192.168.1.10  # Servidor principal como DNS inicial

AUTO_SETUP_SSH_SERVER_INDEX=0
AUTO_SETUP_GLOBAL_PASSWORD=troqueesta
```

---

## 2. Configuração inicial

### Acesso SSH

```bash
ssh root@192.168.1.11
```

### Atualizar sistema

```bash
apt update && apt upgrade -y
dietpi-update
```

### Desabilitar systemd-resolved

```bash
systemctl disable systemd-resolved
systemctl stop systemd-resolved
echo "nameserver 192.168.1.10" > /etc/resolv.conf
```

> Após o Pi-hole estar funcionando localmente, trocar para `127.0.0.1`.

---

## Diferenças em relação ao Servidor Principal

- **Sem Docker** — apenas serviços bare metal
- **IP diferente:** `192.168.1.11` (servidor principal: `192.168.1.10`)
- **Gravity Sync** em vez de ser a fonte — recebe sincronização do principal
- **Sem DHCP** — o DHCP fica apenas no servidor principal (ou no roteador)

---

## Configuração no roteador

Para que os dispositivos da rede usem ambos os DNS:

```
DNS Primário:    192.168.1.10  (Servidor principal)
DNS Secundário:  192.168.1.11  (Orange Pi)
```

Os dispositivos tentam o primário primeiro e caem no secundário automaticamente se o primário não responder.
