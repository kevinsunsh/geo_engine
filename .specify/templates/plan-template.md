# Implementation Plan: [FEATURE]

**Branch**: `[###-feature-name]` | **Date**: [DATE] | **Spec**: [link]
**Input**: Feature specification from `/specs/[###-feature-name]/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

[Extract from feature spec: primary requirement + technical approach from research]

## Technical Context

<!--
  ACTION REQUIRED: Replace the content in this section with the technical details
  for the project. The structure here is presented in advisory capacity to guide
  the iteration process.
-->

**Language/Version**: [e.g., Python 3.11, Swift 5.9, Rust 1.75 or NEEDS CLARIFICATION]  
**Primary Dependencies**: [e.g., FastAPI, UIKit, LLVM or NEEDS CLARIFICATION]  
**Storage**: [if applicable, e.g., PostgreSQL, CoreData, files or N/A]  
**Testing**: [e.g., pytest, XCTest, cargo test or NEEDS CLARIFICATION]  
**Target Platform**: [e.g., Linux server, iOS 15+, WASM or NEEDS CLARIFICATION]
**Project Type**: [single/web/mobile - determines source structure]  
**Performance Goals**: [domain-specific, e.g., 1000 req/s, 10k lines/sec, 60 fps or NEEDS CLARIFICATION]  
**Constraints**: [domain-specific, e.g., <200ms p95, <100MB memory, offline-capable or NEEDS CLARIFICATION]  
**Scale/Scope**: [domain-specific, e.g., 10k users, 1M LOC, 50 screens or NEEDS CLARIFICATION]

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

### 1. 正确性优先（Correctness First）
- [ ] 系统输出是否可验证、可复现？
- [ ] 关键逻辑是否有确定性行为（无随机性）？
- [ ] 数据采集/分析模块是否包含完整性校验机制？

### 2. 可观测性即契约（Observability as Contract）
- [ ] 是否规划结构化日志（JSON格式，包含 trace_id, module, action, status, duration_ms）？
- [ ] 外部调用是否记录请求/响应快照（脱敏）？
- [ ] 关键指标是否通过 Prometheus Metrics 暴露？

### 3. 防御性工程（Defensive by Default）
- [ ] 外部输入是否有校验、清洗、超时控制？
- [ ] 网络调用是否包含重试+熔断机制？
- [ ] 爬虫操作是否遵守 robots.txt 并实现自动降级？

### 4. 测试驱动可信度（Test for Trust, Not Coverage）
- [ ] 是否规划边界条件和错误路径测试？
- [ ] 集成测试是否覆盖真实平台快照回放？
- [ ] 测试策略是否聚焦业务逻辑正确性而非覆盖率数字？

### 5. 性能与成本意识（Efficiency & Cost Awareness）
- [ ] 资源消耗是否可度量、可限制（单次任务 <500MB 内存）？
- [ ] 重型依赖（如 Playwright）是否按需启动、及时释放？
- [ ] 定时任务是否支持动态扩缩容判断？

### 6. 开发者体验（Developer Experience, DX）
- [ ] 本地开发是否支持 `make dev` 一键启动监控沙盒？
- [ ] 配置是否集中、版本化、环境隔离（Pydantic Settings + .env）？
- [ ] 新模块是否提供清晰的 Adapter 接口契约？

### 7. 演进而非重构（Evolve, Don't Rewrite）
- [ ] 架构是否允许渐进式替换，保持数据模型兼容？
- [ ] 旧模块是否标记 @deprecated 而非直接删除？
- [ ] 技术债是否显式记录（# TECHDEBT 注释 + GitHub Issue）？

## Project Structure

### Documentation (this feature)

```text
specs/[###-feature]/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)
<!--
  ACTION REQUIRED: Replace the placeholder tree below with the concrete layout
  for this feature. Delete unused options and expand the chosen structure with
  real paths (e.g., apps/admin, packages/something). The delivered plan must
  not include Option labels.
-->

```text
# [REMOVE IF UNUSED] Option 1: Single project (DEFAULT)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [REMOVE IF UNUSED] Option 2: Web application (when "frontend" + "backend" detected)
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [REMOVE IF UNUSED] Option 3: Mobile + API (when "iOS/Android" detected)
api/
└── [same as backend above]

ios/ or android/
└── [platform-specific structure: feature modules, UI flows, platform tests]
```

**Structure Decision**: [Document the selected structure and reference the real
directories captured above]

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |
