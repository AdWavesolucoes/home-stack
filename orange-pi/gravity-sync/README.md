# Gravity Sync

Sincroniza automaticamente a configuração do Pi-hole do servidor principal (primary) para o Orange Pi (secondary).

---

## O que é sincronizado

| Item | Sincronizado |
|------|:---:|
| Listas de bloqueio (Gravity) | ✅ |
| Whitelist / Blacklist | ✅ |
| Grupos e clientes | ✅ |
| Regex filters | ✅ |
| DNS local (custom.list) | ✅ |
| Configuração do Unbound | ❌ |
| Senha do painel Pi-hole | ❌ |
| Configuração de DHCP | ❌ |

---

## Pré-requisitos

### No Orange Pi (secondary)

1. Pi-hole instalado e funcionando
2. SSH habilitado
3. Acesso SSH do Orange Pi ao servidor principal (por chave)

### Configurar SSH sem senha

No Orange Pi:

```bash
# Gerar chave SSH (se ainda não tiver)
ssh-keygen -t ed25519 -C "gravity-sync@orange-pi" -f ~/.ssh/gravity_sync -N ""

# Copiar chave pública para o servidor principal
ssh-copy-id -i ~/.ssh/gravity_sync.pub root@192.168.1.10

# Testar
ssh -i ~/.ssh/gravity_sync root@192.168.1.10 "echo ok"
```

---

## Instalação do Gravity Sync

No **Orange Pi** (secondary):

```bash
curl -sSL https://raw.githubusercontent.com/vmstan/gravity-sync/master/install.sh | bash
```

---

## Configuração

```bash
gravity-sync config
```

O assistente vai perguntar:

| Pergunta | Valor |
|----------|-------|
| Endereço do servidor primary | `192.168.1.10` (ou IP Tailscale) |
| Usuário SSH | `root` |
| Chave SSH | `/root/.ssh/gravity_sync` |
| Diretório Pi-hole local | `/etc/pihole` |

### Verificar configuração gerada

```bash
cat /etc/gravity-sync/gravity-sync.conf
```

---

## Primeiro sync

```bash
# Puxar configs do servidor principal para o Orange Pi
gravity-sync pull
```

---

## Automação (cron)

```bash
gravity-sync automate
```

Escolha o intervalo — recomendado: **a cada hora** ou **a cada 15 minutos**.

Isso cria um cron job em `/etc/cron.d/gravity-sync`:

```
*/15 * * * * root /usr/local/bin/gravity-sync pull > /dev/null 2>&1
```

---

## Comandos úteis

```bash
# Sincronizar agora (pull do primary)
gravity-sync pull

# Ver status da última sincronização
gravity-sync logs

# Ver diferenças antes de sincronizar
gravity-sync compare

# Atualizar Gravity Sync
gravity-sync update

# Ver versão
gravity-sync version
```

---

## Troubleshooting

### Falha de conexão SSH

```bash
# Testar SSH manualmente
ssh -i /root/.ssh/gravity_sync root@192.168.1.10 "echo ok"

# Verificar permissões da chave
chmod 600 /root/.ssh/gravity_sync
chmod 644 /root/.ssh/gravity_sync.pub
```

### Pi-hole não atualiza após sync

```bash
# Forçar reload do Pi-hole
pihole restartdns
pihole -g
```

### Logs do Gravity Sync

```bash
cat /var/log/gravity-sync/gravity-sync.log
```

---

## Diagrama do fluxo de sincronização

```
Servidor Principal         Orange Pi
   (primary)               (secondary)
       │                       │
       │    gravity-sync pull  │
       │◄──────────────────────│
       │                       │
       │  /etc/pihole/         │
       │  gravity.db ─────────►│
       │  custom.list ────────►│
       │  adlists.list ───────►│
       │                       │
                         pihole restartdns
```
