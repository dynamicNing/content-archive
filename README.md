# Content Archive

何的内容存档库，按类别分目录存储。

## 目录结构

```
content-archive/
├── ai-digest/          🤖 AI Builders Digest 每日播报
├── economic-policy/    📋 每日经济政策资讯
├── skill-updates/      🛠️ 每日热门 Skill 更新
└── social/            🐦 社交媒体原始采集（本地存档，不进 Git）
    ├── youtube/
    ├── weibo/
    ├── tech/
    └── custom/
```

## 内容来源

| 目录 | cron 任务 | 触发时间 |
|------|-----------|---------|
| `ai-digest/` | AI Builders Digest 每日播报 | 每天 06:00 |
| `economic-policy/` | 每日经济政策资讯 | 每天 08:20 |
| `skill-updates/` | 每日热门 Skill 更新 | 每天 08:00 |
| `social/` | 社交媒体采集 | 手动触发（Console） |

## 同步规则

- **md 文件**：通过 `content-sync.sh` 自动推送 PR 合并至 master
- **social/ JSONL**：本地采集存档，`.gitignore` 忽略，不进 Git 仓库

## 维护

```bash
# 手动同步一篇内容
cat content.md | bash /root/.openclaw/workspace/scripts/content-sync.sh "标题" 子目录
```
