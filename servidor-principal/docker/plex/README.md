# Plex Media Server

Servidor de mídia para filmes e séries com suporte a transcodificação via hardware (Intel Quick Sync).

> **Status:** pausado temporariamente.

---

## docker run

```bash
docker run -d \
  --name plex \
  --restart unless-stopped \
  --network host \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=America/Sao_Paulo \
  -e VERSION=docker \
  -v /opt/plex/config:/config \
  -v /mnt/B8B615FAB615BA38/fILMES:/movies \
  -v /mnt/B0CEF741CEF6FE82:/torrents \
  --device /dev/dri:/dev/dri \
  lscr.io/linuxserver/plex:latest
```

---

## Observações

- `--network host` — necessário para o Plex descobrir clientes na rede local via multicast.
- `/dev/dri` — acesso ao hardware de vídeo para transcodificação acelerada (Intel Quick Sync).
- Os filmes ficam no disco montado em `/mnt/B8B615FAB615BA38/fILMES`.
- Os downloads do qBittorrent ficam em `/mnt/B0CEF741CEF6FE82`, mapeado como `/torrents` para o Plex também ter acesso.
