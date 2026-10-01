---
name: ai-research
description: 許可されたEvidenceを使い、Claudeの主分析・Codexの外部調査・Geminiの独立反証・Claudeの最終統合を1サイクル回してResearch Resultへ記録する。
---

# ai-research（3エージェントResearchの1サイクル）

> Showcase extract: 個別Signal、収益目標値、実データpathは除外。AIの役割・Evidence境界・入力権限・記録契約は原型を残している。

**多数決にしない。最後は必ず元のEvidenceへ戻って統合する。**

## 0. 前提

- Evidenceにできるのは、外部AIへの入力が `permitted` の情報源だけ
- 未許可のsourceは読ませない
- missingを0で埋めない
- Research AIへProduction実績を無条件で渡さない

## 1. 問いとEvidenceを選ぶ

問いは「次の判断に効く1つ」に絞る。

Evidenceを削りすぎず、sourceのcontext / guideも含める。

Cycle開始時に:

- question
- selected records
- evidence fingerprints
- source rights

を固定する。

## 2. Claude — 主分析

構造:

```json
{
  "summary": "...",
  "claims": [],
  "hypotheses": [],
  "codex_questions": [],
  "refutation_focus": []
}
```

claimには元Evidenceの裏付けを要求する。

Codex向け問いには内部Evidenceの逐語や内部数値を混ぜない。

## 3. Codex — 外部調査

Codexは公開情報だけで答えられる問いを扱う。

例:

- 公開仕様
- 取引制度
- 市場構造
- 一次情報
- 類似事例
- 学術的裏付け

Evidenceそのものは渡さない。

sourceの日付と一次情報性を見る。

## 4. Gemini — 独立反証

Geminiへは:

- question
- claims
- hypotheses
- refutation focus
- permitted Evidence

を送る。

入力権限に問題があれば止める。

費用を伴う実行はbudget boundaryを通す。

## 5. Claude — 最終統合

再び元Evidenceを読み直す。

```json
{
  "conclusion": "...",
  "claim_assessments": [],
  "gemini_dispositions": [],
  "codex_dispositions": [],
  "unresolved": [],
  "next_candidates": []
}
```

verdict:

- supported
- weakened
- refuted
- unresolved

Codex/Geminiの文章だけをEvidenceにしない。

## 6. Research Result

1 Cycle = 1 directory。

```text
cycle/
├── start
├── evidence
├── claude-analysis
├── codex-1
├── gemini-1
├── synthesis
└── result
```

append-onlyを基本とし、やり直しは新しいattemptまたは新しいCycleで残す。

成立しなかった担い手があれば `flow_complete=false` と理由を残す。

## 7. Explorationとの接続

Research Resultは候補生成・AI assessmentへ使える。

ただしAIは:

- 実績値を作らない
- Machine Evaluationの値を推測しない
- Production移行を決めない
- 資本配分を決めない

Candidateは後段のShadow / Trial / Machine Evaluationへ送る。
