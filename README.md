# Econ Skills

用于经济学实证研究的可复用 Agent skills。

仓库：https://github.com/lawvinx/econ-skils

## Challenge：实证结论压力测试

本仓库保存了 Pedro H. C. Sant’Anna 的 [claude-code-my-workflow](https://github.com/pedrohcgs/claude-code-my-workflow) 中 `challenge` skill 的 Codex 适配版。上游采用 MIT 许可；版权及许可文本均予保留。原文保存在 `skills/challenge/upstream/SKILL.original.md`。

它要求先确定目标因果量、基准估计和合理的分析分支，再运行固定预算的规格网格，报告全部结果及失败，检验敏感性和安慰剂，并记录分析尝试。

### 安装

把 `skills/challenge` 文件夹复制到 `~/.codex/skills/challenge`。已有同名目录时先备份，不要直接覆盖。下一轮对话可显式调用。

### 使用

```text
$challenge 实际分析脚本路径 --forks 64 --dry-run
$challenge 实际分析脚本路径 --forks 64
```

`--forks 64` 指分析规格预算，而不是并行 Agent 数量或 GitHub fork 数量。参数由 Agent 按 skill 指引解释；这不是独立的命令行回归程序。需要实际存在、能够运行的脚本及数据。

`agents/openai.yaml` 保留仅显式调用的设置。Codex 入口已移除 Claude Code 专用工具配置并补齐本地引用。不同目标因果量分别报告；规格曲线本身不能替代因果识别或正式的联合推断。

### 上游与 fork

- 上游：https://github.com/pedrohcgs/claude-code-my-workflow

本仓库保存提取及适配后的 skill。它不是上游整个项目的 GitHub fork；GitHub fork 是账号下另一个独立仓库。

## 许可

见根目录及各 skill 中的 `LICENSE`。本仓库包含 skill 文档和使用说明。
