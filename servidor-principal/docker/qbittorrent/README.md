# qBittorrent

Cliente BitTorrent com interface web.

> **Status:** pausado temporariamente.

---

## docker run

```bash
docker run -d \
  --name qbittorrent \
  --restart unless-stopped \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=America/Sao_Paulo \
  -e WEBUI_PORT=8081 \
  -p 6881:6881 \
  -p 6881:6881/udp \
  -p 8081:8081 \
  -v /docker/qbittorrent/config:/config \
  -v /mnt/B0CEF741CEF6FE82/media/downloads:/downloads \
  lscr.io/linuxserver/qbittorrent:latest
```

---

## Acesso

Interface web: `http://<IP>:8081` ou `https://torrent.lan`

---

## Observações

- Os downloads vão para `/mnt/B0CEF741CEF6FE82/media/downloads`.
- O mesmo disco é mapeado no Plex, permitindo que arquivos baixados apareçam automaticamente na biblioteca de mídia.
