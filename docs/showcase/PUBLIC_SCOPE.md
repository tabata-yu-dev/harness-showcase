# Public / Internal Showcase Scope

## 方針

Showcaseの目的は、private projectの収益ロジックを公開することではない。

目的は、次のEngineering能力を実物で示すこと。

- Multi-Agent Research
- AI input-right boundary
- Claude Skills
- Codex independent review
- responsibility separation
- Evidence contract
- machine evaluation
- Research / Execution separation
- versioned milestone / gate management
- documentation architecture

## 原型を強く残したもの

- `.claude/agents/*`
- `.codex/*`
- `research/.claude/settings.json`

## Private情報だけ落として原型を残したもの

- `.claude/skills/*`
- `research/.claude/skills/ai-research/SKILL.md`
- `CLAUDE.md`
- `AGENTS.md`
- `docs/basic/基本ドキュメント_概要マップ.md`
- `docs/milestones.md`
- `docs/sections/共通設計_リポジトリ構成.md`
- `docs/verification_system.md`
- `docs/handoff/HANDOFF.md`

## 出さないもの

- `src/revenue_project/**` の本体source
- production config
- private strategy declarations
- Signal conditions
- raw / normalized / aggregate Dataset
- actual orders / fills / P&L
- account / key / secret
- concrete capital / risk values
- private research hypotheses
- full business roadmap

## 原則

**説明用に綺麗に作り直しすぎない。**

多少の内部用語、責務名、workflow名を残し、「実際に運用しているRepositoryの断片」であることを優先する。
