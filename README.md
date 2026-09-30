# wefewe/muse2api

`muse2api` 的自建构建镜像仓库（用于本集群内部部署，非上游官方仓库）。

## 这是什么

- **上游**：`github.com/czg86389-hub/muse2api`（MIT，Python + FastAPI；把 Meta **Muse** 图像/视频/对话网页端逆向封装为 OpenAI 兼容 API）。
- 上游**不发布任何镜像**，故本仓库自建并保持同步。仓库结构：
  - `upstream/` —— 上游源码快照（自动同步自 `czg86389-hub/muse2api` 默认分支 `main` 最新提交）；
  - `.github/workflows/build.yml` —— 多架构（amd64/arm64）构建并推送 `ghcr.io/wefewe/muse2api`；
  - `.github/workflows/sync-upstream.yml` —— 每日同步上游源码（有变化即 dispatch 重新构建）；
  - `.github/workflows/keepalive.yml` —— 月度保活提交（防 60 天定时停用）。

## 许可

上游为 MIT 许可，版权归原作者，详见 `upstream/LICENSE`。

## 部署（本集群）

- 镜像：`ghcr.io/wefewe/muse2api:latest`
- 端口：`18610`（容器内）；数据：`/app/data`（账号池、任务、媒体）
- 需 Chromium（镜像内已装，启动参数含 `--disable-dev-shm-usage`，无需额外 shm 配置）
- 编排见 `/opt/swarm/stacks/us/muse2api.yml`

> ⚠️ 上游内置 `/admin/update/upgrade`「一键在线升级」会自行拉取 GitHub 覆盖运行代码，与受控镜像更新模型冲突，部署时应于反代层阻断该路径。
