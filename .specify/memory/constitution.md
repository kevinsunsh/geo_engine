<!--
Sync Impact Report:
- Version change: 1.0.0 → 1.0.0 (no content changes, verification only)
- Principles verified: 7 core principles (all present and aligned)
  1. 正确性优先（Correctness First）
  2. 可观测性即契约（Observability as Contract）
  3. 防御性工程（Defensive by Default）
  4. 测试驱动可信度（Test for Trust, Not Coverage）
  5. 性能与成本意识（Efficiency & Cost Awareness）
  6. 开发者体验（Developer Experience, DX）
  7. 演进而非重构（Evolve, Don't Rewrite）
- Modified sections: None
- Added sections: None
- Removed sections: None
- Templates requiring updates:
  ✅ .specify/templates/plan-template.md (Constitution Check section aligned with all 7 principles)
  ✅ .specify/templates/tasks-template.md (task categorization aligned with principles, includes constitution references)
  ✅ .specify/templates/spec-template.md (verified - no direct constitution references needed, aligns with requirements structure)
  ✅ .specify/templates/checklist-template.md (verified - template structure compatible)
  ✅ .cursor/commands/speckit.constitution.md (verified - command file references are generic)
- Follow-up TODOs: None
-->

# Geo Engine Constitution

## Core Principles

### 1. 正确性优先（Correctness First）

系统输出（如召回率、内容排名）必须可验证、可复现。

关键逻辑（如排名解析、域名匹配、奖励计算）需有确定性行为，避免随机性导致调试困难。

任何数据采集或分析模块必须包含完整性校验机制（如结果条数校验、字段非空断言）。

### 2. 可观测性即契约（Observability as Contract）

每个模块必须输出结构化日志（JSON格式），包含：trace_id, module, action, status, duration_ms。

所有外部调用（如 Playwright 操作、API 请求）必须记录请求/响应快照（脱敏）用于回溯。

关键指标（如采集成功率、召回率变化）必须通过 Prometheus Metrics 暴露，支持告警。

### 3. 防御性工程（Defensive by Default）

所有外部输入（搜索结果、API 响应）视为不可信，必须校验、清洗、超时控制。

网络调用必须包含重试+熔断（使用 tenacity 或类似），避免雪崩。

爬虫类操作必须遵守 robots.txt（若适用）并实现自动降级（如频繁失败则暂停该平台）。

### 4. 测试驱动可信度（Test for Trust, Not Coverage）

单元测试聚焦边界条件与错误路径（如解析失败、空结果、反爬响应）。

集成测试必须覆盖真实平台快照回放（录制 Perplexity/Qwen 响应，用于 CI 回归）。

禁止"为了覆盖率而测试"——业务逻辑正确性 > 行覆盖数字。

### 5. 性能与成本意识（Efficiency & Cost Awareness）

资源消耗（CPU、内存、网络）必须可度量、可限制。单次采集任务不得超过 500MB 内存。

Playwright 等重型依赖必须按需启动、及时释放，禁止常驻浏览器实例。

所有定时任务必须支持动态扩缩容判断（如无关键词时自动休眠）。

### 6. 开发者体验（Developer Experience, DX）

本地开发必须支持 make dev 一键启动监控沙盒（含 mock 平台或录制回放）。

配置必须集中、版本化、环境隔离（使用 Pydantic Settings + .env）。

新模块接入必须提供清晰的 Adapter 接口契约，降低扩展成本。

### 7. 演进而非重构（Evolve, Don't Rewrite）

架构允许渐进式替换（如从 Playwright 迁移到官方 API），但需保持数据模型兼容。

旧模块标记 @deprecated 而非直接删除，确保平滑过渡。

技术债必须显式记录（如 # TECHDEBT: ... 注释 + GitHub Issue 关联）。

## Governance

本宪法是 Geo Engine 项目的最高治理文档，所有开发实践、代码审查和技术决策必须遵循这些原则。

### 修订程序

- 原则的添加、修改或删除需要：
  1. 在 GitHub Issue 中记录变更提案及理由
  2. 团队评审和讨论
  3. 更新本文件并同步更新相关模板
  4. 更新版本号（遵循语义化版本规则）

### 版本管理

- **MAJOR**: 向后不兼容的原则移除或重新定义
- **MINOR**: 新增原则或实质性扩展指导
- **PATCH**: 澄清、措辞修正、非语义性改进

### 合规审查

所有 PR 和代码审查必须验证是否符合宪法原则。违反原则的代码需要：
- 在 PR 中明确说明违反的原则
- 提供正当理由或迁移计划
- 标记为技术债（# TECHDEBT）并关联 Issue

**Version**: 1.0.0 | **Ratified**: 2025-11-23 | **Last Amended**: 2025-11-23
