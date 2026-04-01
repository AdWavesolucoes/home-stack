# Tailscale

VPN mesh que conecta todos os dispositivos da rede de forma segura, permitindo acesso remoto sem expor portas para a internet.

---

## Por que bare metal?

O Tailscale roda diretamente no host para garantir que o acesso remoto esteja disponível independentemente do estado do Docker. Se um container travar ou o Docker reiniciar, a VPN continua funcionando.

---

## Instalação

```bash
curl -fsSL https://tailscale.com/install.sh | sh
tailscale up
```

## Dispositivos na Rede

| Hostname | OS | Função |
|----------|----|--------|
| `servidorcasa` | Linux | Servidor principal |
| `desktop-lucas` | Windows | Desktop pessoal |
| `poco-x7` | Android | Smartphone |
| `tab-s10-lite` | Android | Tablet |

---

## Uso

Com Tailscale ativo, todos os serviços acessíveis pelo IP interno (`192.168.0.x`) também ficam disponíveis remotamente via IP Tailscale (`100.x.x.x`), sem precisar abrir portas no roteador.

### Acesso ao Pi-hole remotamente

Configure o DNS do Tailscale para apontar para o IP Tailscale do servidor (`100.69.59.102`) e o bloqueio de anúncios funciona em qualquer rede.

---

## Observação

O Tailscale não gerencia o `/etc/resolv.conf` nesta instalação (operação não permitida pelo sistema). O DNS da VPN é configurado manualmente nos clientes quando necessário.
