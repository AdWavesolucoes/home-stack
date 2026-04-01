# Servidor Principal

Servidor rodando **DietPi (Debian 13 / Trixie)** em hardware x86_64.

---

## Sistema Operacional

| Item | Valor |
|------|-------|
| **SO** | DietPi (Debian GNU/Linux 13 Trixie) |
| **Kernel** | 6.12.x |
| **Arquitetura** | x86_64 |
| **Hostname** | DietPi |

---

## Serviços em Bare Metal

Esses serviços rodam diretamente no host, fora do Docker, por serem críticos para o funcionamento da rede. Se o Docker travar ou reiniciar, DNS, VPN e acesso FTP continuam disponíveis.

| Serviço | Documentação |
|---------|-------------|
| Pi-hole | [pihole/README.md](pihole/README.md) |
| Unbound | [unbound/README.md](unbound/README.md) |
| Tailscale | [tailscale/README.md](tailscale/README.md) |
| vsftpd (FTP) | [ftp/README.md](ftp/README.md) |

---

## Serviços Docker

Os demais serviços rodam em containers Docker.

Documentação: [docker/README.md](docker/README.md)
