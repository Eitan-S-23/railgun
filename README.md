# railgun.info 自动签到

针对 https://railgun.info/ 的自动签到脚本，通过 GitHub Actions 定时运行（默认每 8 小时一次），并可推送结果到企业微信/钉钉等 Webhook。

## 部署方法

1. Fork 或将本仓库推送到你的 GitHub。
2. 在仓库 `Settings` → `Secrets and variables` → `Actions` → `New repository secret` 添加以下两个 Secret：

### 必需的环境变量（Secrets）

**ACCOUNTS_JSON**（账号列表，JSON 数组）：
```json
[
  { "email": "your_email@example.com", "cookie": "koa:sess=xxxxx; koa:sess.sig=xxxxx" },
  { "email": "another@example.com",   "cookie": "koa:sess=xxxxx; koa:sess.sig=xxxxx" }
]
```

Cookie 获取方法：登录 https://railgun.info/ 后，打开浏览器开发者工具 → Application/Storage → Cookies，复制 `koa:sess` 和 `koa:sess.sig` 两项。

**PUSH_WEBHOOK_URL**：推送 Webhook 地址（企业微信机器人等，可选；不配置则仅在日志中输出）。

3. 进入仓库 `Actions` 标签页，启用 workflow。可在 `Actions` → `railgun-checkin` → `Run workflow` 手动触发测试。

## 本地运行

```bash
pip install requests curl_cffi
export ACCOUNTS_JSON='[{"email":"...","cookie":"..."}]'
export PUSH_WEBHOOK_URL='https://...'
python main.py
```

## 定时策略

默认 `.github/workflows/checkin.yml` 中 cron 为 `0 */8 * * *`（UTC，每 8 小时执行一次），可自行修改。
