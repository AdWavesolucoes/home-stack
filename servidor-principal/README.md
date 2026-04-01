# Servidor Principal

Servidor central da homelab rodando DietPi. Responsável por DNS primário, proxy reverso e todos os serviços Docker.

---

## Especificações

| Item | Detalhe |
|------|---------|
| **SO** | DietPi (Debian-based) |
| **DNS Primário** | Pi-hole + Unbound (bare metal) |
| **VPN** | Tailscale (bare metal) |
| **Containers** | Docker (15+ serviços) |

---

## Ordem de Instalação

Siga essa sequência para replicar do zero:

1. [Instalação do DietPi](#1-instalação-do-dietpi)
2. [Configuração inicial](#2-configuração-inicial)
3. [Pi-hole](./pihole/README.md)
4. [Unbound](./unbound/README.md)
5. [Tailscale](./tailscale/README.md)
6. [Docker e serviços](./docker/README.md)

---

## 1. Instalação do DietPi

### Download

Acesse [dietpi.com/downloads](https://dietpi.com/#download) e baixe a imagem para seu hardware (x86_64, RPi, Orange Pi, etc.).

### Flash

```bash
# Com Balena Etcher (GUI) ou:
sudo dd if=DietPi_*.img of=/dev/sdX bs=4M status=progress conv=fsync
```

### Primeiro boot

No primeiro boot o DietPi roda o assistente de configuração automático. Você pode pré-configurar tudo via `dietpi.txt` antes do boot:

```bash
# Montar a partição boot do cartão/SSD e editar:
nano /mnt/boot/dietpi.txt
```

Configurações recomendadas no `dietpi.txt`:

```ini
# Locale e timezone
AUTO_SETUP_LOCALE=pt_BR.UTF-8
AUTO_SETUP_TIMEZONE=America/Sao_Paulo
AUTO_SETUP_KEYBOARD_LAYOUT=br

# Rede (se não usar DHCP)
AUTO_SETUP_NET_ETHERNET_ENABLED=1
AUTO_SETUP_NET_WIFI_ENABLED=0
AUTO_SETUP_NET_USESTATIC=1
AUTO_SETUP_NET_STATIC_IP=192.168.1.10
AUTO_SETUP_NET_STATIC_MASK=255.255.255.0
AUTO_SETUP_NET_STATIC_GATEWAY=192.168.1.1
AUTO_SETUP_NET_STATIC_DNS=1.1.1.1

# SSH habilitado desde o início
AUTO_SETUP_SSH_SERVER_INDEX=0

# Senha root (troque após instalação)
AUTO_SETUP_GLOBAL_PASSWORD=troqueesta
```

---

## 2. Configuração Inicial

### Acesso SSH

```bash
ssh root@192.168.1.10
# Senha padrão: dietpi (ou a definida no dietpi.txt)
```

### Atualização do sistema

```bash
apt update && apt upgrade -y
dietpi-update
```

### Configurar IP estático (se não fez via dietpi.txt)

```bash
dietpi-config
# Network Options → Ethernet → Static
```

Ou editar diretamente:

```bash
nano /etc/network/interfaces
```

```
auto eth0
iface eth0 inet static
  address 192.168.1.10
  netmask 255.255.255.0
  gateway 192.168.1.1
```

### Desabilitar resolv.conf automático (importante para Pi-hole)

```bash
# Impede que o systemd-resolved conflite com o Pi-hole na porta 53
systemctl disable systemd-resolved
systemctl stop systemd-resolved

# Definir DNS temporário até o Pi-hole estar rodando
echo "nameserver 1.1.1.1" > /etc/resolv.conf
```

### Criar usuário não-root (opcional mas recomendado)

```bash
adduser homelab
usermod -aG sudo homelab
# Copiar chaves SSH se necessário
rsync --archive --chown=homelab:homelab ~/.ssh /home/homelab
```

### Hardening SSH

```bash
nano /etc/ssh/sshd_config
```

```
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

```bash
systemctl restart ssh
```

---

## Próximo passo

Continuar com a instalação do [Pi-hole](./pihole/README.md).
