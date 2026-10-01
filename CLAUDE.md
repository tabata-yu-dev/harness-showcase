# CLAUDE.md

このrepositoryは、AI Research / Strategy Exploration / Execution Safety / Evaluationを分離して扱うprivate R&D systemのShowcase extractです。

## 目的

開発そのものを目的にせず、**実データで仮説を検証できるsystemを作ること**を目的とする。

標準は短いcycle:

```text
修正
→ 実行・確認
→ 結果確認
→ 必要なら即修正
```

## 越えてはならない線

- `Strategy → SignalCandidate → Risk → OrderIntent → Execution` の順を守る
- Research / CLI / Reportから直接Executionへ到達しない
- 未承認・未検証の設定で外部side effectを始めない
- Raw / Normalized / Decision / Order / Fill / Auditを上書きしない
- 未実行の検査を成功として扱わない
- 秘密情報をGit・通常log・AI inputへ入れない
- 仕様判断を仮定で埋めて完了と呼ばない
- 質問・説明要求への回答だけでfileを変更しない

## 作業の入口

| 入口 | 使うとき |
| --- | --- |
| ユーザー指示による直接修正 | 標準 |
| 一問ずつの確認 | 仕様が本当に分岐するとき |
| `/handoff-implement` | 次の新規実装単位へ進む |
| `/review` | 独立AIレビューを名指しで実行 |
| `/review-fix` | 指定review findingの包括補正 |

## 規則の在処

| 知りたいこと | 正 |
| --- | --- |
| 現在の段階とGate | `docs/milestones.md` |
| 正本の責務分担 | `docs/basic/基本ドキュメント_概要マップ.md` |
| package / Composition Root | `docs/sections/共通設計_リポジトリ構成.md` |
| 検証lane | `docs/verification_system.md` |
| 現在地 | `docs/handoff/HANDOFF.md` |
