# Research / Execution Boundary

## Concept

Research AIとExecutionを同一責務にしません。

```text
開発機
Development / Control
       │
       ▼
専用機
Research / Data / AI / Dashboard
       │
       │ read-only operational records
       ▼
Windows VPS
Execution / Reconciliation / Audit
```

## Boundary Rules

Research AI:

- 仮説を生成できる
- 公開情報を調査できる
- 仮説を反証できる
- 次に必要なEvidenceを提案できる

Research AIから直接行わないこと:

- order submission
- production approval
- risk-limit modification
- capital allocation
- production transition

## Why

AIの能力を増やすことと、AIの権限を増やすことは別問題です。

Researchを高速化しながら、Executionの責任境界を維持することを目的とします。
