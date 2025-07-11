# Proxy Docker Compose

Este repositório contém a configuração de um **nginx-proxy** com suporte a HTTPS automático via Let's Encrypt.
Ele é utilizado como proxy reverso para outros containers, como o projeto [Feiras Londrina](https://github.com/srcjp/feiraslondrina).

## Uso rápido
1. Instale Docker e Docker Compose na máquina.
2. Crie a rede externa chamada `proxy` caso ainda não exista:
   ```bash
   docker network create proxy
   ```
3. Suba o proxy:
   ```bash
   docker-compose up -d
   ```

O container `nginx-proxy` ficará ouvindo nas portas **80** e **443**. Qualquer container conectado a esta rede com as variáveis `VIRTUAL_HOST` e `LETSENCRYPT_HOST` terá o tráfego roteado automaticamente.

Exemplo de execução do [Feiras Londrina](https://github.com/srcjp/feiraslondrina):
```bash
# dentro do repositório do feiraslondrina
cp .env.example .env  # edite os domínios desejados
docker-compose up -d --build
```

Aguarde alguns minutos para que o Let's Encrypt emita os certificados. Caso algo não funcione, verifique os logs do proxy com:
```bash
docker-compose logs -f nginx-proxy
```

## Observações
- O label do container do proxy foi atualizado para `com.github.nginx-proxy.nginx-proxy.nginx`. Se estiver usando um projeto que depende deste proxy (como o feiraslondrina), verifique se o mesmo label é utilizado nos containers proxied.

