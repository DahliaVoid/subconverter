# subconverter 最终版源码

这是当前运行中的 `subconverter:local` 镜像（容器 d30a9ec30e5e）对应的完整源码。

源码状态：
- 基于 metacubex/subconverter，HEAD 提交 `6b4d9fa`
- 已包含两轮本地修复（`patches/subconverter-fix.patch`、`patches/subconverter-fix2.patch`），主要修复 REALITY short-id 在 Clash 输出中被 YAML 1.1 误读、vless 参数解析（fp/spx/alpn）、Sing-box tls 输出等问题
- `base/` 为当前生产镜像实际使用的配置（含规则与缓存），不是上游默认模板

## 构建

```bash
docker build -t subconverter:local .
```

可选构建参数：

```bash
docker build --build-arg THREADS=2 --build-arg SHA=your-commit .
```

构建产物与当前使用的镜像保持一致：端口 `25500/tcp`、工作目录 `/base`、启动命令 `subconverter`、标签 `maintainer=local-build`。
