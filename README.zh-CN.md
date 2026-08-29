# Helldivers 2 Skill for OpenClaw

[English](README.md) | [中文](README.zh-CN.md)

一个 [OpenClaw](https://github.com/openclaw/openclaw) 技能，通过社区 API [api.helldivers2.dev](https://api.helldivers2.dev) 查询 [Helldivers 2](https://www.helldivers2.com/) 的实时银河战争数据。

## 功能

直接向你的 OpenClaw 代理询问银河战争的最新态势：

- **战况总览** — 全服在线人数、任务统计、各阵营击杀数
- **主要指令（Major Order）** — 当前目标、完成进度、剩余时间、奖励
- **星球状态** — 全部 273 个星球：归属方、解放进度、在线玩家、生态环境、危害
- **活跃战役** — 当前所有前线战役及玩家分布
- **新闻快讯** — 最新的游戏内超级地球广播

## 使用示例

安装后，直接用自然语言提问：

- "Helldivers 2 现在战况怎么样？"
- "Major Order 进度到哪了？"
- "Malevelon Creek 上有多少玩家？"
- "超级地球有什么新消息？"

## 安装

将 `helldivers2/` 目录复制到你的 OpenClaw skills 文件夹：

```bash
cp -r helldivers2/ ~/.openclaw/skills/helldivers2/
```

或使用 OpenClaw 的插件/技能管理命令。

## API 说明

- **数据来源**: [api.helldivers2.dev](https://api.helldivers2.dev)（社区维护）
- **认证**: 无需认证；通过自定义请求头标识客户端
- **限流**: 每 10 秒滑动窗口约 5 次请求（429 时返回 `Retry-After: 10`）
- **安全速率**: 每 2 秒 1 次请求

### 可用端点

| 端点 | 说明 |
|------|------|
| `GET /api/v1/war` | 全服战况统计 |
| `GET /api/v1/assignments` | 当前主要指令 |
| `GET /api/v1/planets` | 全部 273 个星球 |
| `GET /api/v1/planets/{index}` | 单个星球详情 |
| `GET /api/v1/campaigns` | 活跃战役前线 |
| `GET /api/v1/dispatches` | 游戏内新闻推送 |

## 仓库结构

```
helldivers2/
└── SKILL.md    # 技能定义，包含 API 参考和使用模式
```

## 致谢

- 技能由 AI 助手 [Lumi](https://github.com/ProperSAMA) 为 ProperSAMA 创建
- 数据由 [Helldivers 2 社区 API](https://api.helldivers2.dev) 提供
- 游戏开发商：[Arrowhead Game Studios](https://www.arrowheadgamestudios.com/)

---

*为了超级地球！🌍✨*
