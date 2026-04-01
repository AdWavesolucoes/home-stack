# FTP — vsftpd

Servidor FTP local para transferência de arquivos na rede interna.

---

## Por que bare metal?

O FTP roda diretamente no host para acesso direto ao sistema de arquivos sem camadas adicionais de mapeamento de volumes.

---

## Instalação (DietPi)

O vsftpd é instalado via DietPi-Software:

```bash
dietpi-software install 83
```

Ou manualmente:

```bash
apt install vsftpd -y
```

## Configuração

Arquivo: `/etc/vsftpd.conf`

| Opção | Valor | Descrição |
|-------|-------|-----------|
| `anonymous_enable` | `NO` | Acesso anônimo desabilitado |
| `local_enable` | `YES` | Usuários locais do sistema podem fazer login |
| `write_enable` | `YES` | Permite upload de arquivos |
| `local_root` | `/mnt/dietpi_userdata` | Diretório raiz dos usuários |
| `idle_session_timeout` | `60` | Desconecta sessões inativas em 60s |
| `data_connection_timeout` | `30` | Timeout de conexão de dados em 30s |
| `listen_ipv6` | `NO` | Escuta apenas IPv4 |
| `rsa_cert_file` | `/etc/ssl/private/vsftpd.pem` | Certificado TLS |

---

## Aviso de Segurança

FTP transmite credenciais em texto claro por padrão. Use apenas na rede local (LAN) e nunca exponha a porta 21 para a internet. Para acesso remoto, prefira SFTP via SSH (porta 22, já disponível via Tailscale).
