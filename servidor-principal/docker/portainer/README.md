# Portainer

Interface web para gerenciar containers, imagens, volumes, redes e stacks Docker.

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `https://portainer.lan` | Acesso via proxy reverso (NPM) |
| `http://IP:9000` | HTTP direto |
| `https://IP:9443` | HTTPS direto |

---

## Quick Start

```bash
docker compose up -d
```

Primeiro acesso: crie o usuário admin em `http://IP:9000`.

---

## Observações

- O volume `portainer_data` persiste configurações, usuários e ambientes entre atualizações
- O socket Docker (`/var/run/docker.sock`) é montado para controle total do daemon

---

## docker run

```bash
docker run -d \
  --name portainer \
  --restart always \
  -p 9000:9000 \
  -p 9443:9443 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```

---

## Acesso

- HTTP: `http://<IP>:9000`
- HTTPS: `https://portainer.lan` ou `https://<IP>:9443`
