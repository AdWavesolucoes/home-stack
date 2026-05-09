# Servidor Casa — Documentação

Documentação da infraestrutura do servidor doméstico rodando **DietPi** (Debian 13 Trixie) em um **Intel Core i7-4770** com 16 GB DDR3.

---

## Arquivos deste diretório

| Arquivo | Descrição |
|---|---|
| [`INFRA.md`](INFRA.md) | Hardware, rede, armazenamento, diagrama de arquitetura |
| [`SERVICOS.md`](SERVICOS.md) | Todos os serviços instalados, containers Docker, stacks |
| [`docker/`](docker/) | Configurações dos serviços Docker individuais |
| [`pihole/`](pihole/) | Configuração Pi-hole |
| [`unbound/`](unbound/) | Configuração Unbound DNS recursivo |
| [`tailscale/`](tailscale/) | Configuração Tailscale VPN |
| [`ftp/`](ftp/) | Configuração vsftpd |

---

## Resumo Rápido

| Item | Valor |
|---|---|
| OS | DietPi / Debian 13 Trixie |
| CPU | Intel Core i7-4770 (4C/8T @ 3.4 GHz) |
| RAM | 16 GB DDR3 Kingston Dual Channel |
| IP Local | 192.168.0.100 |
| IP Tailscale | 100.69.59.102 |
| DNS local | Pi-hole + Unbound |
| Proxy reverso | Nginx Proxy Manager |
| Containers ativos | 10 |

---

## Acesso Rápido

| Serviço | URL |
|---|---|
| Open WebUI (IA) | http://aichat.lan |
| Nextcloud | https://nextcloud.lan |
| Home Assistant | http://192.168.0.100:8123 |
| Portainer | https://portainer.lan |
| Pi-hole | http://dns.lan/admin |
| NPM Admin | http://proxy.lan |
| Homepage | http://home.lan |
| Plex | http://plex.lan:32400/web |

---

## Segurança — Arquivos Sensíveis

> ⚠️ Os arquivos abaixo **NÃO devem ser commitados**. Estão listados no `.gitignore`.

| Arquivo | Conteúdo sensível |
|---|---|
| `/opt/paperclip/.env` | OpenAI API Key |
| `/opt/stacks/watchtower/docker-compose.yml` | Token do bot Telegram hardcoded |
| `/opt/nextcloud/docker-compose.yml` | Senhas do banco hardcoded |
| `/opt/stacks/homeassistant/config/` | Credenciais RTSP da câmera |
| `/root/pc-monitor-webapp/docker-compose.yml` | Senha MQTT hardcoded |

---

## Hardware Futuro Planejado

- **RTX 3060** (prevista para ago/2026) — habilitar `deepseek-coder-v2:16b` no Ollama
