# AI Development Harness

## PromptではなくHarness

このRepositoryでは、AI駆動開発を単発Promptの工夫だけに依存させません。

Repository内に、

- Skills
- subagents
- read-only agents
- review contracts
- verification roles
- post-fix scanning
- workspace permissions

を置き、AIの仕事の仕方そのものをEngineering対象にしています。

## Example Structure

```text
.claude/
├── agents/
│   ├── fix-mapper.md
│   ├── fix-verifier.md
│   └── post-scanner.md
└── skills/
    └── review/
        └── SKILL.md

.codex/
├── config.toml
└── agents/
    └── review-reader.toml

research/
└── .claude/
    ├── settings.json
    └── skills/
        └── ai-research/
            └── SKILL.md
```

## Separation of Responsibilities

```text
Implementation
    ↓
Independent Review
    ↓
Finding / Obligation
    ↓
Fix Mapping
    ↓
Intentional Edit
    ↓
Verification
    ↓
Post Scan
```

1つのAIに「調査して、設計して、実装して、レビューして、修正して」を連続で任せないことが重要です。

## Goal

AIの知能を完全に制御することではありません。

**AIが間違えることを前提に、間違いが次の工程で発見可能な構造にすること**が目的です。
