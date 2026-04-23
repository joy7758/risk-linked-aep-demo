# Risk-linked Agent Evidence Profile Demo

## Project Status / 项目状态

**v0.1-review frozen artifact / v0.1-review 冻结制品**  
Tag / 标签: `v0.1-review`  
Commit / 提交: `841f7d6`  
Demo / 演示: PASS / WARN / FAIL reproducible / 可复现  
Acceptance / 验收: PASS  

This repository contains a minimal research prototype for a **Risk-linked Agent Evidence Profile (AEP)**. It links pre-execution risk findings with runtime execution evidence and validates executions as PASS, WARN, or FAIL.  
本仓库包含一个最小化的 **Risk-linked Agent Evidence Profile (AEP)** 研究原型。它把执行前风险发现与运行时执行证据连接起来，并将执行结果判定为 PASS、WARN 或 FAIL。

Not a production system. Not a compliance tool. Not a real scanner, SARIF, MCP, FDO registry, or data-space implementation.  
这不是生产系统，不是合规工具，也不是真实 scanner、SARIF、MCP、FDO registry 或 data-space 实现。

Recommended entry points / 推荐入口:

- `REVIEW_HANDOFF.md`
- `release/v0.1-review/README.md`
- `paper/aep-profile-v0.1.md`
- `paper/draft/full-paper-draft.md`

## Problem / 问题

Security scanners can identify potential risks, but they do not prove what actually happened during an agent execution.  
安全扫描器可以识别潜在风险，但它们并不能证明一次 agent 执行过程中实际发生了什么。

## Contribution / 贡献

This demo links pre-execution risk findings with runtime execution evidence and validator conclusions. A scanner report says what may go wrong, the AEP event log records what actually happened, and the validator decides whether the execution is acceptable.  
这个 demo 将执行前风险发现、运行时执行证据和 validator 判定连接在一起。scanner report 说明“可能出什么问题”，AEP event log 记录“实际发生了什么”，validator 决定“这次执行是否可接受”。

## Architecture / 架构

```text
Scanner risk report
      ↓
Demo agent runtime
      ↓
AEP collector
      ↓
AEP bundle + event log
      ↓
AEP validator
      ↓
PASS / WARN / FAIL
```

## Run / 运行

```bash
python3 demo/run_all.py
```

You can also use the Makefile targets:  
你也可以使用 Makefile 目标：

```bash
make demo
make acceptance
```

If the local system maps `python` to Python 3, `python demo/run_all.py` may also work.  
如果本地系统把 `python` 映射到 Python 3，那么 `python demo/run_all.py` 也可能可以运行。

## Expected Results / 预期结果

PASS:
- no high-risk scanner finding
- only allowed tools used

PASS（通过）：
- 没有高风险 scanner finding
- 只使用被允许的工具

WARN:
- high-risk `write_file` finding exists
- `write_file` was not attempted

WARN（警告）：
- 存在高风险 `write_file` finding
- 但没有尝试 `write_file`

FAIL:
- high-risk `write_file` finding exists
- `write_file` was attempted
- collector blocked execution
- attempt still counts as violation

FAIL（失败）：
- 存在高风险 `write_file` finding
- 尝试了 `write_file`
- collector 阻断了执行
- 该尝试仍被计为违规

## Why This Matters / 为什么这件事重要

Scanner tells what may go wrong. AEP records what actually happened. Validator determines whether execution is acceptable. FDO/data-space environments can preserve and reference the resulting evidence object as an evidence profile for accountable agent execution.  
Scanner 说明可能出什么问题。AEP 记录实际发生了什么。Validator 判断这次执行是否可接受。FDO/data-space 环境可以保存并引用由此产生的证据对象，把它作为可问责 agent 执行的 evidence profile。

## Paper-facing Materials / 论文相关材料

The `paper/` directory contains research-facing notes for contribution framing, profile description, validator rules, evaluation, and scope control.  
`paper/` 目录包含面向研究写作的材料，包括贡献定位、profile 描述、validator 规则、评估说明和范围控制。

The `paper/draft/` directory contains a controlled paper skeleton for later manuscript development.  
`paper/draft/` 目录包含受控的论文草稿骨架，用于后续手稿发展。

The `paper/related-work/` directory contains terminology and related-work positioning notes.  
`paper/related-work/` 目录包含术语说明和 related work 定位笔记。

The `paper/references/` directory contains source inventory and claim-to-source mapping notes.  
`paper/references/` 目录包含来源清单和 claim-to-source 映射笔记。

- `paper/contribution-statement.md`
- `paper/aep-profile-v0.1.md`
- `paper/validator-rules.md`
- `paper/evaluation-section.md`
- `paper/research-scope.md`
- `paper/abstract-draft.md`

## v0.1 Review Release / v0.1 评审发布包

Review-release materials are available in `release/v0.1-review/`.  
评审发布材料位于 `release/v0.1-review/`。

Freeze guidance is available in `FREEZE.md` and `release/v0.1-review/freeze-checklist.md`.  
冻结指导见 `FREEZE.md` 和 `release/v0.1-review/freeze-checklist.md`。

Manual review handoff is available in `REVIEW_HANDOFF.md`.  
人工评审交接材料见 `REVIEW_HANDOFF.md`。

- `release/v0.1-review/README.md`
- `release/v0.1-review/artifact-manifest.md`
- `release/v0.1-review/reproducibility-instructions.md`
- `release/v0.1-review/review-checklist.md`
- `release/v0.1-review/paper-draft-index.md`
- `release/v0.1-review/release-notes-v0.1.md`
- `release/v0.1-review/scope-and-nonclaims.md`

## Scope Limits / 范围边界

- no real scanner integration / 不包含真实 scanner 集成
- no real FDO registry / 不包含真实 FDO registry
- no production cryptographic signing / 不包含生产级密码签名
- no LLM runtime / 不包含 LLM runtime
- no MCP integration / 不包含 MCP 集成

The signature is a demo HMAC-SHA256 value using a fixed secret in source code. It is not production key management and must not be treated as a real trust mechanism.  
签名是一个使用源码中固定 secret 生成的 demo HMAC-SHA256 值。它不是生产级密钥管理，也不能被视为真实信任机制。
