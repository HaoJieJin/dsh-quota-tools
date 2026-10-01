# DSH 额度工具

**🌐 [English](README.md) | [简体中文](README.zh-CN.md)**

两款配套 CLI 工具，服务于 **DeepSeek Harness (DSH)** agent 平台。

| 工具 | 用途 |
|---|---|
| `dsh-quota-check` | 子代理模型路由的起飞前检查：读取 DSH 额度插件快照以及本地「已耗尽记忆」文件，然后推荐一条路由，或报告某模型今天已耗尽 |
| `dsh-cost` | 计算某个 DSH 会话花了多少钱，使用真实 token 数 × 官方价格，并支持 **DeepSeek 峰谷定价**（工作日 9-12 点与 14-18 点为高峰 ×2；周末/节假日为谷时半价） |

## 环境要求

- Node.js 18+
- 为运行中的 DSH 实例设计，但两个工具都能优雅降级：
  - `dsh-quota-check` 读取 `http://127.0.0.1:${DSH_PORT||3080}/dsh-quota/snapshot`
  - `dsh-cost` 读取 `$DSH_HOME` 下的会话日志文件

## 用法

```bash
# dsh-quota-check
dsh-quota-check                          # 人类可读的汇总
dsh-quota-check --json                   # 原始快照
dsh-quota-check --pick default|long|short|vision   # 推荐路由
dsh-quota-check --check <provider>/<model>         # 检查单条路由
dsh-quota-check --dry <provider>/<model> [reason]  # 标记为今天已耗尽
dsh-quota-check --undry <provider>/<model>         # 取消已耗尽标记
dsh-quota-check --clear                  # 清除今天的记忆

# dsh-cost
dsh-cost                                 # 最近一个活动会话
dsh-cost --session <id>                  # 指定会话
dsh-cost --all                           # 所有会话
dsh-cost --today                         # 今天总计
dsh-cost --json                          # 机器可读
dsh-cost --balance                       # 仅账户余额
dsh-cost --no-net                        # 仅本地（不联网）
dsh-cost --peak                          # 当前是高峰还是谷时
```

退出码（`dsh-quota-check`）：`0` = 可用，`2` = 已耗尽，`3` = 未知。

## 环境变量

| 变量 | 说明 |
|---|---|
| `DSH_HOME` | DSH home 路径（默认相对脚本解析） |
| `DSH_QUOTA_MEM` | 额度记忆 JSON 文件路径 |
| `DSH_PORT` | DSH Web 服务端口（默认 `3080`） |
| `CURL` | curl 可执行文件路径（默认 `curl`） |

## License

MIT
