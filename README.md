# Content Archive

何的内容存档库，按类别分目录存储。

## 目录结构

```
content-archive/
├── ai-digest/          🤖 AI Builders Digest 每日播报
├── economic-policy/    📋 每日经济政策资讯
├── skill-updates/      🛠️ 每日热门 Skill 更新
└── social/            🐦 社交媒体采集存档（扁平 md，不进 Git）
```

## social/ 存档规则（2026-04-17 起）

```
social/
├── 2026-04-17-youtube.md    扁平化：日期-平台.md
├── 2026-04-17-weibo.md
├── 2026-04-17-tech.md
└── 2026-04-17-custom.md
```

- **格式**：可读 Markdown，带平台分组和链接，直接浏览无需解析
- **存储位置**：`/root/content-archive/social/`（本地，不进 Git）
- **触发方式**：Console → 🐦 社交采集 tab → 手动触发 / Hook 自动

## 内容来源

| 目录 | cron 任务 | 触发时间 |
|------|-----------|---------|
| `ai-digest/` | AI Builders Digest 每日播报 | 每天 06:00 |
| `economic-policy/` | 每日经济政策资讯 | 每天 08:20 |
| `skill-updates/` | 每日热门 Skill 更新 | 每天 08:00 |
| `social/` | 社交媒体采集 | 手动触发（Console） |

## 同步规则

- **ai-digest / economic-policy / skill-updates**：通过 `content-sync.sh` 自动推送 PR 合并至 master
- **social/**：本地存档，不进 Git 仓库

## 维护

```bash
# 手动同步一篇内容
cat content.md | bash /root/.openclaw/workspace/scripts/content-sync.sh "标题" 子目录
```
