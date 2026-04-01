# Orange Pi Zero 2W — DNS Secundário

Atua como servidor DNS secundário da rede para garantir disponibilidade caso o servidor principal fique offline.

---

## Hardware

| Item | Valor |
|------|-------|
| **Placa** | Orange Pi Zero 2W |
| **SO** | DietPi |
| **Função** | DNS secundário |

---

## Status

Configuração base instalada. Pi-hole e Unbound ainda não configurados.

---

## Configuração planejada

A configuração seguirá o mesmo padrão do servidor principal:

- **Pi-hole** como DNS na porta 53
- **Unbound** como resolver recursivo na porta 5335
- **Tailscale** para acesso remoto

### Sincronização com o servidor principal

Após configurado, o **Gravity Sync** manterá o Pi-hole secundário em sincronia com o primário:

```bash
# Instalar Gravity Sync no secundário
curl -sSL https://raw.githubusercontent.com/vmstan/gravity-sync/master/install.sh | bash
```

O que é sincronizado automaticamente:

| Item | Sincronizado |
|------|-------------|
| Listas de bloqueio (Gravity) | Sim |
| Whitelist / Blacklist | Sim |
| Grupos e clientes | Sim |
| Regex filters | Sim |
| Configuração do Unbound | Não (local em cada instância) |
| Senha do painel | Não (independente) |

---

## Configuração dos clientes

Após o DNS secundário estar ativo, configurar nos dispositivos (ou no DHCP do roteador):

- DNS Primário: `192.168.0.100` (servidor principal)
- DNS Secundário: IP do Orange Pi
