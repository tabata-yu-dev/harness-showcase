# HANDOFF（Showcase Extract）

## 現在地

- Production Execution、Research/Data/AI、Development/Controlを物理的・責務的に分離
- Demo executionで自動Entry → Hold → Exit → Reconciliation → Flat → Next Signalの実端末経路を確認済み
- Research Dataset / Aggregate / Read-only Dashboardの経路を実装
- 3-Agent AI Research基盤を実機で1 cycle完了
  - Claude primary analysis
  - Codex external research
  - Gemini independent refutation
  - Claude synthesis
  - `flow_complete=True`
- Candidate ledger / Shadow / Machine Evaluation / AI Assessment / Rankingを実装
- Short-term declarative Strategy Engineを追加
- Strategy Cartridgeを追加し、Candidate declarationをRevenue本体のcodeから分離
- 開発機上のFake Terminalで探索loopをentrypointからDashboardまで通過確認

## 現行構造

```text
AI Research
   ↓
Candidate Ledger
   ↓
Strategy Cartridge
   ↓
Shadow
   ↓
Machine Evaluation
   ↓
Demo Trial
   ↓
Machine Evaluation + AI Assessment
   ↓
Human Decision
   ↓
Formal Configuration
```

## 確かめたこと

- Candidate validationがinvalid declarationを拒否する
- 実装が必要なCandidateをTrialへ誤投入しない
- AI assessmentを無制限に上書きしない
- same input → same evaluation fingerprint
- Dashboardはmissingを0として表示しない
- Research AIはProduction実績Dataset / Machine Evaluationを直接読まない
- Research ResultだけではStrategy adoption / capital / Production transitionを決めない

## 次の作業

Private projectでは、実機への反映、Candidate generation、Shadow / Trialの反復、Formal evaluationへ進むためのEvidence蓄積を継続する。

Showcase repositoryでは実データ・Candidate条件・P&L・private strategyは公開しない。
