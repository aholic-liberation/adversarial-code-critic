# adversarial-code-critic

> **English abstract**: A Claude Code skill that turns the model into an adversarial reviewer rather than a sycophantic assistant. Triggered by the words “核查” / “核验”, it demands evidence-backed findings, grades issues P0–P3, loops through “critique → rework → re-critique” until the user says “pass”, and ships generic checklists for embedded C, mathematical modeling (CUMCM B/C), and PCB design.

## 这是什么

一个 **Claude Code 用户级 skill**：把模型从「助手」切换成「反方辩论家 / 批评家」。

默认立场是「这份成果有问题」，直到证据证明它没有。目标不是让用户听着舒服，而是让成果在真实世界里站得住。

核心约束：

- **证据优先**：每条批评必须落到 `file:line`、具体片段/寄存器、文档段落或数据字段；找不到证据就不说。
- **三个维度**：硬伤/错误、名实相符（是否真的做到了它声称的事）、现实适用性/隐患。
- **严重度分级**：P0 致命、P1 重要、P2 待验证、P3 可选；**无 P0/P1 即达标**，不为了显得尖锐硬挑 P3。
- **循环协议**：定基调 → 评审 → 用户修改/反驳 → 诚实复核 → 用户说「pass」才退出。
- **清单优先**：先对照 `references/` 里的领域清单；清单没覆盖的，联网检索补充，并把可复用的坑点沉淀回清单。

## 触发方式

在 Claude Code 里说 **「核查」** 或 **「核验」** 即可启用；说 **「pass」** 验收退出。

## 目录结构

```
adversarial-code-critic/
├── SKILL.md
└── references/
    ├── embedded-c-checklist.md       # 通用嵌入式 C：ISR、定时器、PID、通信、内存等
    ├── mathmodel-bc-checklist.md     # 数学建模 B/C 题：优化/运筹、数据/统计
    ├── cumcm-guide-checklist.md      # 数学建模国赛规范与国奖标准
    └── pcb-design-checklist.md       # PCB：原理图、布局布线、制造文件、设计流程
```

## 安装

```bash
git clone https://github.com/aholic-liberation/adversarial-code-critic.git \
  ~/.claude/skills/adversarial-code-critic
```

也可以只把目录复制到 `~/.claude/skills/` 下。Claude Code 下次启动时会识别 `SKILL.md`。

## 使用

```
核查一下这个驱动程序的超时处理
核验这篇论文的模型检验部分
```

首轮它会先问清「这个东西原本要做什么」，再按 P0–P3 输出评审报告。你修改或反驳后，它会复核；确认无 P0/P1 后说「pass」结束。

## 清单是活文档

`references/` 里的清单是**提醒清单，不是豁免边界**：不在清单里的真问题照样要批。目标对象属于新领域时，先联网检索该领域的缺陷模式与标准，再把可复用的条目补进清单。

平台/项目专属的清单往往包含内部环境、地址和现场记录，**建议留在私有 skill 中，不要提交到公开仓库**。本仓库只保留可公开复用的通用清单。

## License

[MIT](LICENSE)
