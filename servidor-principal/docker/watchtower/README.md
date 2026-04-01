# Watchtower

Atualiza automaticamente as imagens Docker dos containers marcados com a label correspondente.

---

## docker run

```bash
docker run -d \
  --name watchtower \
  --restart always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower:1.7.1
```

---

## Como funciona

O Watchtower verifica periodicamente se há novas versões das imagens no registry. Quando encontra, para o container, baixa a nova imagem e reinicia com as mesmas configurações.

## Opt-in por label

Para que o Watchtower atualize um container, adicione a label:

```bash
--label com.centurylinklabs.watchtower.enable=true
```

Containers sem essa label são ignorados.
