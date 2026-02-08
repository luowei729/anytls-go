# AnyTLS

一个试图缓解 嵌套的TLS握手指纹(TLS in TLS) 问题的代理协议。`anytls-go` 是该协议的参考实现。

- 灵活的分包和填充策略
- 连接复用，降低代理延迟
- 简洁的配置

[用户常见问题](./docs/faq.md)

[协议文档](./docs/protocol.md)

[URI 格式](./docs/uri_scheme.md)

## 快速食用方法

为了方便，示例服务器和客户端默认采用不安全的配置，该配置假设您不会遭遇 TLS 中间人攻击（这种情况偶尔发生在网络接入层，在骨干网络上几乎不可能实现）；否则，您的通信内容可能会被中间人截获。

### 示例服务器

```
./anytls-server -l 0.0.0.0:8443 -p 密码
```

`0.0.0.0:8443` 为服务器监听的地址和端口。

### 示例客户端

```
./anytls-client -l 127.0.0.1:1080 -s 服务器ip:端口 -p 密码
```

`127.0.0.1:1080` 为本机 Socks5 代理监听地址，理论上支持 TCP 和 UDP(通过 udp over tcp 传输)。

#### SOCKS5 用户名密码认证 (可选)

从 v0.0.13 版本起，示例客户端支持 SOCKS5 用户名密码认证 (RFC 1929):

```
./anytls-client -l 127.0.0.1:1080 -s 服务器ip:端口 -p 密码 -socks-user 用户名 -socks-pass 密码
```

如果提供了 `-socks-user` 或 `-socks-pass` 参数，则必须同时提供两者。如果不提供这些参数，SOCKS5 代理将不使用认证。

v0.0.12 版本起，示例客户端可直接使用 URI 格式:

```
./anytls-client -l 127.0.0.1:1080 -s "anytls://password@host:port"
```

### sing-box

https://github.com/SagerNet/sing-box

它包含了 anytls 协议的服务器和客户端。

### mihomo

https://github.com/MetaCubeX/mihomo

它包含了 anytls 协议的服务器和客户端。

### Shadowrocket

Shadowrocket 2.2.65+ 实现了 anytls 协议的客户端。

## 开发与发布

### 自动构建与发布

项目使用 GitHub Actions 进行持续集成和自动发布：

1. **测试构建**: 每次推送到 `main` 分支或创建 Pull Request 时，会自动运行测试和构建
2. **发布版本**: 创建以 `v` 开头的 tag（如 `v1.0.0`）时，会自动运行 GoReleaser 编译多平台二进制文件并创建 GitHub Release

发布流程会为以下平台生成二进制文件：
- Linux (amd64, arm64)
- Windows (amd64, arm64)
- macOS (amd64, arm64)

所有发布文件都包含 SHA256 校验和。

### 本地开发

```bash
# 构建客户端
go build ./cmd/client

# 构建服务器
go build ./cmd/server
```
