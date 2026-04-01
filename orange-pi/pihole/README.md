# Pi-hole — DNS Secundário (Orange Pi)

Instalação idêntica ao servidor principal. As listas e configurações são sincronizadas automaticamente via Gravity Sync.

---

## Instalação

```bash
curl -sSL https://install.pi-hole.net | bash
```

Durante o instalador:
- Interface: `eth0`
- Upstream DNS: `127.0.0.1#5335` (Unbound local — instalar antes)
- Blocklists: manter padrão (virão do Gravity Sync)
- Instalar web admin: **Sim**
- Instalar lighttpd: **Sim**
- Habilitar logging: **Sim**

---

## Pós-instalação

### Definir senha do painel

```bash
pihole -a -p
# Pode ser diferente da senha do servidor principal
```

### Apontar para Unbound local

```bash
pihole -a setdns 127.0.0.1#5335
pihole restartdns
```

---

## Configuração do DNS secundário

O Orange Pi **não** deve ter DHCP habilitado. Ele apenas responde consultas DNS como fallback.

```bash
# Verificar que DHCP está desabilitado
grep DHCP_ACTIVE /etc/pihole/setupVars.conf
# Deve retornar: DHCP_ACTIVE=false
```

---

## Diferenças do servidor principal

| Configuração | Servidor Principal | Orange Pi |
|---|---|---|
| **IP** | 192.168.1.10 | 192.168.1.11 |
| **DHCP** | Habilitado (opcional) | Desabilitado |
| **Gravity Sync** | Fonte (primary) | Destino (secondary) |
| **Listas** | Gerenciadas aqui | Recebidas via sync |

---

## Após configurar o Gravity Sync

As listas serão sincronizadas automaticamente do servidor principal. Não é necessário adicionar listas manualmente aqui.

Ver: [Gravity Sync](../gravity-sync/README.md)

---

## Comandos úteis

```bash
pihole status
pihole -up
pihole restartdns

# Ver logs
pihole -t
```
