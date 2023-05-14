# webapp.io-deploy

设置 secret 环境变量部署到 webapp.io
<https://webapp.io/{你的GitHub用户名}/secrets>

部署项目
<https://webapp.io/{你的GitHub用户名}/deployments>

点 `Deploy a new site` 按钮 下载你 `fork`的本项目部署

## 环境变量说明

Nezha 的端口设置为 443 就会自动加 --tls

如果不使用的环境变量 可以删除 SECRET ENV 一行
例如不使用 v2board

```dockerfile
# Layerfile
# ...
RUN docker run -d -p 80:3000 --name container   \
    -e ARGO_AUTH=$ARGO_AsUTH \
    -e ARGO_DOMAIN=$ARGO_DOMAIN  \
    -e NEZHA_KEY=$NEZHA_KEY     \
    -e NEZHA_PORT=$NEZHA_PORT      \
    -e NEZHA_SERVER=$NEZHA_SERVER       \
    -e PORT=$PORT          \
    ghcr.io/3kmfi6hp/argo-airport-paas:main
    # ...
```

原来的

```dockerfile
# Layerfile
# ...
RUN docker run -d -p 80:3000 --name container   \
    -e ARGO_AUTH=$ARGO_AUTH \
    -e ARGO_DOMAIN=$ARGO_DOMAIN  \
    -e NEZHA_KEY=$NEZHA_KEY     \
    -e NEZHA_PORT=$NEZHA_PORT      \
    -e NEZHA_SERVER=$NEZHA_SERVER       \
    -e PORT=$PORT          \
    -e TARGET_HOSTNAME_URL=$TARGET_HOSTNAME_URL   \
    -e API_HOST=$API_HOST         \
    -e API_KEY=$API_KEY       \
    -e NODE_ID=$NODE_ID         \
    ghcr.io/3kmfi6hp/argo-airport-paas:main
    # ...
```

```env
# 以下为 cloudflared 相关配置
SECRET ENV ARGO_AUTH      # Argo 项目的认证信息TOKEN
SECRET ENV ARGO_DOMAIN    # Argo 访问项目的域名
# 以下为 Nezha 相关配置
SECRET ENV NEZHA_KEY      # Nezha 的 key
SECRET ENV NEZHA_PORT     # Nezha 的服务端口
SECRET ENV NEZHA_SERVER   # Nezha 的服务地址
SECRET ENV PORT           # 容器内服务的端口 一般设置 3000
SECRET ENV TARGET_HOSTNAME_URL # 代理目标主机的网址 不使用 v2board 设置 http://127.0.0.1:8081
SECRET ENV MAX_MEMORY_RESTART # PM2重启时的内存阈值
# 以下为 v2board 相关配置
SECRET ENV API_HOST       # v2board API 服务的域名URL
SECRET ENV API_KEY        # v2board API 的 access key
SECRET ENV CERT_DOMAIN    # v2board 证书的域名
SECRET ENV NODE_ID        # v2board 节点 ID
```

### 选择添加

```env
SECRET ENV SSH_PUB_KEY # 设置Public Key 用于ssh连接 一般不需要设置 除非你需要ssh连接 ssh-rsa AAAAB3NzaC1yc2EAAA...
SECRET ENV TUNNEL_TRANSPORT_PROTOCOL # 设置cloudflared 传输协议 默认为 quic 可选 http2
```
