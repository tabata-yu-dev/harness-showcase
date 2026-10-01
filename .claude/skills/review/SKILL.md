---
name: review
description: 完了した実装単位またはGateを、守る性質・責務地図・兄弟責務・障害経路にわたりCodexへ配布して独立レビューする。実装も設計も変更しない。
---

# レビュー

> Showcase extract: Revenue固有のGate IDや過去finding番号は省略。二周独立走査、母集団、Evidence、Reproducer-Integratorの構造は原型を残している。

渡されたレビュー依頼書を契約として扱う。

**検査はCodexが行う。**
Claude本線はレビュー契約、母集団、圧縮結果、最終記録だけを扱い、対象コードや長い走査ログを抱え込まない。

実装コード、設定、依存関係、本番データを変更しない。新しい設計も提案しない。

## 1. レビュー水準と対象

状態、許可、監査、永続化、復元、外部副作用、秘密情報の境界に触れる変更は性質横断レビューを要する。

依頼書には:

- 守る性質と出所
- 責務地図
- 兄弟責務
- 母集団の宣言
- 各行の厚み
- 必須反証観点とその持ち主
- 再利用の対象
- 除外範囲
- 受入基準
- 実行する検証

を要求する。

契約が不足していれば黙って範囲を狭めない。

## 2. Codexの独立した2周

### Round 1 — Responsibility Map

新しいCodex threadで、

```text
entry
→ state transition
→ canonical record
→ projection/read side
→ recovery/reverse
→ sibling/parallel/failure
```

を通して走査する。

### Round 2 — Specification Reconstruction

Round 1とは**別thread**で開始する。

Round 1の候補、判断、threadIdを渡さない。

依頼書の宣言を出発点にせず、参照する正本・受入基準から母集団を独立に引き直し、宣言との差を返す。

返す差:

- 正本にあるのに宣言に無い軸
- 宣言にあるが正本に出所を持たない軸

### Thickness

状態・権限・監査・永続化・復元・外部副作用・秘密情報に触れる論理単位は「厚い」とし、両Roundで走査する。

薄い単位はRound 1のみ。

費用の都合で厚い単位を薄くしない。

## 3. Evidence contract

各論理単位は、候補0件でも「実際に何を反証したか」を1行返す。

```text
EVIDENCE:
- command/injection | observed result
```

これが無いものを「走査済み0件」にしない。

**見て0件** と **見ずに0件** を区別する。

## 4. Reproducer-Integrator

Round 1 / Round 2の後、第三の独立threadで候補を再現・統合する。

役割:

- candidate reproduction
- duplicate family detection
- existing obligationとの照合
- false positive除外
- stable FAMILY / OBLIGATIONへの統合

Round 1とRound 2の「票」で決めない。

## 5. Result

結果には:

- scanned / unscanned
- findings
- stable family
- obligations
- tradeoffs
- reproduction
- remaining uncertainty
- review gate decision

を残す。

実装変更はしない。

Codex返却の詳細契約は `codex_contract.md` に分離する。
