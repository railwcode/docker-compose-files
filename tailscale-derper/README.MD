# tailscale-derper

## 介绍

Tailscale 使用自定义的 DERP Servers。

[Tailscale 基础教程：部署私有 DERP 中继服务器](https://icloudnative.io/posts/custom-derp-servers/)

## docker-compose.yaml

```yaml
version: '3.7'

services: 
  tailscale-derper:
    image: fredliang/derper
    container_name: tailscale-derper
    volumes:
      - ./certs:/app/certs
    ports:
      - 8443:8443
#      - 443:443
      - 3478:3478/udp
    expose:
      - 80
      - 443
    command:
      - "/app/derper"
      - "--hostname=$DERP_DOMAIN"
      - "--certmode=$DERP_CERT_MODE"
      - "--certdir=$DERP_CERT_DIR"
      - "--a=$DERP_ADDR"
      - "--stun=$DERP_STUN"
      - "--verify-clients=$DERP_VERIFY_CLIENTS"
      - "--stun-port=$DERP_STUN_PORT"

    networks:
      - default
networks:
  default:
```

## .env

```env
DERP_CERT_MODE=manual
DERP_CERT_DIR=/app/certs
DERP_DOMAIN=xxx.example.org
DERP_ADDR=:443
DERP_STUN=true
DERP_STUN_PORT=3478
DERP_VERIFY_CLIENTS=false
```