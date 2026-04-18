# 量化基本面研究团队的 CLAUDE.md、Skills 和 Hooks 实战配置指南

> 来源：微信公众号 Zen Traders
> 原文链接：https://mp.weixin.qq.com/s?__biz=MzkxMTIxNzgyMw%3D%3D&mid=2247489627&idx=1&sn=e9fdcfeea61876c2bdb10d55000d152b
> 发布日期：2026年4月4日 02:15（美国）
> 作者：Zen Traders

---

## 为什么要做这件事

我们是一个五人的量化基本面研究团队，日常围绕美股 portfolio 策略展开工作——从因子研究、SEC Filing 解析到信号回测，大量时间花在重复性的数据处理和代码编写上。

拿到 Claude Team 版本后，我做的第一件事不是直接开干，而是**先做配置**。就像量化系统里你不会裸写策略一样，Claude Code 也需要一套"策略框架"来确保团队五个人的使用体验一致、效率最大化。

这篇文章完整分享我们的配置方案，包括三大核心组件：**CLAUDE.md**、**Skills** 和 **Hooks**。

---

## 先回答最重要的问题：数据安全

做量化的人把策略代码和持仓数据喂给 AI，第一反应一定是：**安全吗？**

答案取决于你用的是哪个版本。

Claude 的产品线分两大类：消费级（Free / Pro / Max）和商业级（Team / Enterprise / API）。在 2025 年 9 月的隐私政策更新后，消费级版本引入了训练数据的 opt-in 机制——如果用户选择同意，对话数据可能被保留最长 5 年用于模型训练。

**核心区别在于**：Anthropic 在商业版中充当数据处理者（data processor），你的组织是数据控制者（data controller）。用户对话数据不会进入模型训练管道，无论你是否做了任何设置。

具体来说，Team 版本的数据保护要点：

- 对话数据不用于模型训练
- 数据保留期最短（可配置）
- 支持 SSO 和审计日志

所以对于做美股量化的团队，**Team 版本是最低摩擦的安全选择**。当然好的操作习惯仍然重要：不要在对话中直接粘贴完整持仓明细或 API 密钥——这是使用任何 SaaS 产品的基本原则。CLAUDE.md 里写的是代码规范和工作流，不是实际持仓数据，这一点分清楚就行。

---

## 一、CLAUDE.md：给 Claude 植入团队 DNA

CLAUDE.md 是 Claude Code 的持久记忆文件。每次会话启动时自动读取作为上下文。可以理解为：**团队的编码准则和业务规则的永久存储**。

### 文件放在哪？

Claude Code 支持多层级 CLAUDE.md，作用域从大到小：

1. **全局**（用户主目录）—— 通用偏好
2. **项目级**（仓库根目录）—— 团队共享规范
3. **目录级**（子目录）—— 按模块定制

核心配置放在项目级 CLAUDE.md 里，通过 Git 共享给所有人。

### 我们的 CLAUDE.md 长什么样

```markdown
# Quant Fundamental Research — US Equities

## 技术栈
- Python 3.11+ / Pandas / NumPy / Polars
- 数据源：Bloomberg API, Compustat via WRDS, SEC EDGAR
- 回测框架：自研 engine（src/backtest/）
- 因子库：src/factors/ | 策略库：src/strategies/

## 代码规范
- type hints + docstring
- DataFrame 列名 snake_case
- 因子命名：{category}_{name}_{window} 如 value_ep_ttm
- 日期格式 YYYY-MM-DD
- 回测指标必含：Ann Return, Max DD, Sharpe, Turnover

## 数据约定
- Ticker 格式：AAPL（纯 ticker）或 AAPL US Equity（BBG）
- GICS 行业分类为默认行业体系
- Portfolio 权重之和 = 1.0
- 缺失值：行业中位数填充优先
- 收益率：对数收益率
- 市值单位：百万美元
```

### 三条核心写作原则

1. **具体不抽象**：给出真实字段名、函数签名、文件路径
2. **结果导向**：先说输出什么，再说怎么调用
3. **可验证**：每个规范都能被 linter 或测试捕获

### 进阶：rules 目录拆分

CLAUDE.md 过长时用 `rules/` 拆分，并支持路径匹配——只有编辑因子代码时才加载因子规范：

```yaml
---
paths:
  - "src/factors/**/*.py"
---
# 因子开发规范
- 因子函数接受 DataFrame 输入返回 Series
- 必须处理 NaN
- 必须包含 IC 测试
```

---

## 二、Skills：量化工具箱

Skills 是技能扩展系统。写一个 SKILL.md，Claude 就学会一项新能力。可通过 `/skil` 手动调用，也可让 Claude 自动加载。

### 核心 Skills 一览

**因子回测 Skill** — 一键跑完 Rank IC 时序、ICIR、分组回测、多头组合绩效，输出结论。

**财务提取 Skill** — 自动提取 Revenue、Net Income、Gross Margin、ROE、FCF，计算 YoY 增速，对比 GICS 行业中位数，标注异常值。

**组合分析 Skill** — 加载配置 YAML，跑回测引擎，生成月度热力图、vs SPY 超额曲线、分年度绩效表，输出 PDF。设置了 `disable-model-invocation: true`。

### Skill 设计要点

（原文此处有详细展开，原文快照未完整捕获）

---

## 三、Hooks：确定性自动化

（原文此处有详细展开，原文快照未完整捕获）

### 团队配置的四个关键 Hook

（原文此处有详细展开，原文快照未完整捕获）

---

## 四、落地清单

（原文此处有详细展开，原文快照未完整捕获）

---

## 最后

（原文此处有详细展开，原文快照未完整捕获）

---

> Zen Traders — 我们在硅谷构建 Agentic Strategy Pipeline。公众号专注分享开发过程中的技术方法论与踩坑记录，欢迎先私信交流；也可访问 zentraders.xyz 体验产品。
