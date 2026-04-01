# Portainer

Interface web para gerenciar containers, imagens, volumes e redes Docker.

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
