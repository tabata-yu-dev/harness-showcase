# 開発と運用のマイルストーン

> Showcase extract — 実際のRevenue正本の段階名・責務・Gate構造を残し、privateな閾値・金額・個別Signal条件だけを除外している。

本書は、開発単位、運用・評価の段階、Gate、確認位置の正本である。

**期間が経過したから次へ進むのではなく、事前に定義したEvidenceと判断条件で進む。**

作業の標準cycle:

```text
修正 → 実行・確認 → 結果確認 → 必要なら即修正
```

## 1. 開発単位（D0〜D8）

開発単位はSystem Foundation。Demo以降の運用・評価段階とは分ける。

| 開発単位 | 内容 | 状態 |
| --- | --- | --- |
| D0〜D6 | 共通骨格、validation、market data、execution safety、capital/economics | Foundation |
| D7 | 共通識別・商品条件・AI boundary・cost allocation・共通移行 | Foundation |
| D8 | always-on運用の強化、remote recovery、notification、operations dashboard | 必要性確認後 |

`Strategy → SignalCandidate → Risk → OrderIntent → Execution` の機能開発は一度DEV-COMPLETEを通し、その後はDemo / Live Evidenceから短いcycleで直す。

## 2. Demo以降の運用・評価

実際の流れ:

```text
EXPLORATION
    ↓
DEMO-SETUP
    ↓
EVIDENCE
    ↓
EARLY-ECON
    ↓
SEED-APPROVAL
    ↓
LIVE-PRE
    ↓
LIVE-FIRST
    ↓
LIVE-VERIFY
    ↓
SCALE-DECIDE
    ↓
REINVEST
    ↓
EXPAND
    └──────────→ REINVEST / SCALE-DECIDEへ循環
```

### 2-1. EXPLORATION

**問い:** Formal evaluationへ進めるMarket / Signal familyはどれか。

Evidence:

- product terms
- quote / spread
- execution
- slippage / error
- signal density
- skipped opportunity
- holding / exit
- cost
- capital efficiency
- operational reliability

探索中の結果をFormal GateのEvidenceには数えない。

AI Researchを含むCandidate生成から、Shadow・short Trial・Machine Evaluation・AI Assessmentで候補を絞る。

### 2-2. DEMO-SETUP

**問い:** 何を、どの条件で、何のEvidenceとして集めるか。

ここで初めてFormal評価条件を事前宣言する。

- Market / Signal family
- versioned conditions
- scenario family
- evidence window
- cost allocation
- record contract
- approval / execution boundary
- Production autonomy
- non-blocking research replication

宣言前の観測は `post-hoc` として区別する。

### 2-3. EVIDENCE

**問い:** 判断に必要なEvidenceが揃ったか。

- Demo executed
- Historical replay
- Shadow
- operational records

結果を見て条件を後から寄せない。

### 2-4. EARLY-ECON

**問い:** Cost控除後に経済性が成立するか。

Machine Evaluationは事前に宣言した指標から計算する。

AIは利益額を作らない。

成立しない場合:

- Evidenceを延長
- 新versionとして再宣言
- Market / Signal familyを変更
- EXPLORATIONへ戻る
- Projectを停止

### 2-5. SEED-APPROVAL

**問い:** Formal Demo / Economicsを基に、実資金へ進むか。

ここは**Human Decision Gate**。

Systemは金額・優先順位を作らず、承認されたrecordとの一致だけを確認する。

### 2-6. LIVE-PRE

**問い:** Production前提条件が揃っているか。

- product / permission
- market data
- quantity
- cost
- funding
- stop/recovery path
- execution environment

不足があればLiveへ進まない。

### 2-7. LIVE-FIRST

**問い:** 最小のLive executionが事故なく回るか。

- duplicate order
- unknown position
- reconciliation
- execution cost
- approved risk limit

問題があれば停止し、人の再開承認へ戻る。

### 2-8. LIVE-VERIFY

**問い:** Demo / Scenario推定と実測の差は許容できるか。

- execution cost
- slippage
- latency
- capture
- operational burden

差を過去artifactへ上書きせず、新versionとして反映する。

### 2-9. SCALE-DECIDE

**問い:** Scale / Fix / Stopのどれか。

Scaleは単なる残高比例ではなく、実測したmarginal economicsとrisk/capacityから判断する。

### 2-10. REINVEST

**問い:** 実現したProject profitをどこまで次の配分へ回すか。

Estimateやunrealized valueではなく、実現したresultを基準にする。

### 2-11. EXPAND

**問い:** 次のMarket / Strategy / Operational Capabilityへ拡張するか。

次の対象も同じGateを通る。

## 3. 確認の位置

| 位置 | 対象 |
| --- | --- |
| EVIDENCE中 | Determinism / post-hoc拒否 / ResearchからExecutionへの非到達 |
| DEMO-SETUP前 | Production autonomy / recovery / local audit / non-blocking replication |
| LIVE-PRE | 外部境界 / permission / production prerequisites |
| EXPAND | always-on強化 / remote operations |

Human Decision GateとCode Verification Gateを同じものにしない。
