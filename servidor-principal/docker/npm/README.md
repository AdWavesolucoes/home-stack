# Nginx Proxy Manager (NPM)

Proxy reverso com interface web para gerenciar hosts, certificados TLS e redirecionamentos.

---

## docker run

```bash
docker run -d \
  --name npm \
  --restart unless-stopped \
  -p 80:80 \
  -p 443:443 \
  -p 81:81 \
  -v /opt/npm/data:/data \
  -v /opt/npm/letsencrypt:/etc/letsencrypt \
  jc21/nginx-proxy-manager:latest
```

---

## Acesso

- Interface de administração: `http://<IP>:81`
- Proxy reverso: portas 80 e 443

---

## Uso

O NPM recebe todo o tráfego HTTP/HTTPS da rede interna e distribui para os serviços corretos com base no domínio `.lan`.

Exemplo de fluxo:

```
https://grafana.lan  →  NPM  →  http://grafana:3000
https://portainer.lan →  NPM  →  http://portainer:9000
```

### TLS nos domínios .lan

Os certificados para os domínios `.lan` são gerados com **mkcert** e uma CA raiz própria instalada nos dispositivos da rede. Isso permite HTTPS válido nos serviços internos sem depender de Let's Encrypt.

```bash
# Instalar mkcert
apt install mkcert -y
mkcert -install

# Gerar certificado
mkcert grafana.lan
```
