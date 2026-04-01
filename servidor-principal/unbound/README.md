# Unbound

Resolver DNS recursivo local. Recebe as queries do Pi-hole e resolve diretamente nos servidores raiz, sem passar por terceiros.

---

## Por que Unbound em vez de DNS externo?

Com Unbound, a resolução DNS acontece assim:

```
Pi-hole → Unbound → Servidores Raiz → Resposta
```

Sem Unbound (usando ex. 8.8.8.8):

```
Pi-hole → Google DNS → Resposta  (Google vê todas as suas queries)
```

---

## Instalação

```bash
apt install unbound -y
```

## Configuração

Arquivo: `/etc/unbound/unbound.conf.d/pi-hole.conf`

```conf
server:
  interface: 127.0.0.1
  port: 5335

  do-ip4: yes
  do-udp: yes
  do-tcp: yes

  # IPv6 desabilitado
  do-ip6: no

  # Aceita queries apenas do localhost (Pi-hole)
  access-control: 127.0.0.1 allow

  # Não revela identidade nem versão do servidor
  hide-identity: yes
  hide-version: yes

  # Pré-carrega registros para reduzir latência
  prefetch: yes
  num-threads: 1
```

O arquivo de configuração está disponível em [pi-hole.conf](pi-hole.conf).

---

## Segurança

- Escuta **apenas em `127.0.0.1:5335`** — inacessível direto pela rede
- `access-control: 127.0.0.1 allow` — somente o Pi-hole pode consultar
- `hide-identity` e `hide-version` — não expõe informações do servidor
- IPv6 desabilitado para evitar tentativas de bind em `::1`

---

## Verificação

```bash
# Testar resolução direta no Unbound
dig google.com @127.0.0.1 -p 5335

# Verificar status do serviço
systemctl status unbound
```
