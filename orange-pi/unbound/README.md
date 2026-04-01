# Unbound — Orange Pi

Configuração idêntica ao servidor principal. Cada instância resolve DNS de forma independente.

---

## Instalação

```bash
apt install unbound -y
```

---

## Configuração

```bash
nano /etc/unbound/unbound.conf.d/pi-hole.conf
```

Cole o conteúdo do arquivo [`pi-hole.conf`](./pi-hole.conf) — é **idêntico** ao do servidor principal.

---

## Iniciar

```bash
systemctl enable unbound
systemctl start unbound
```

### Testar

```bash
dig pi-hole.net @127.0.0.1 -p 5335
```

---

## Nota

O Unbound do Orange Pi **não** é sincronizado pelo Gravity Sync — cada instância mantém sua própria configuração local. Isso é intencional: se o servidor principal cair, o Orange Pi ainda resolve DNS independentemente.
