---
name: codex-guided-implement
description: Codexに実装前のコードベース調査と実装後の独立評価を任せ、Claude Codeが正本と要求に対して実装・検証・補正する。
---

# Codex調査付き実装

> Showcase extract: private project固有の市場・資本・口座情報だけを除外し、実運用の責務分離と手順は原型を残している。

このスキルでは役割を混ぜない。

- **Codex**: 実装前のコードベース横断調査、要求と現行実装の差分・根本原因の特定、実装後の独立評価
- **Claude Code**: Codex調査結果を起点に必要箇所だけ確認し、正本と要求に対して実装・検証・補正する
- **ユーザー**: 正本・コード・実測からは決まらない仕様、外部操作、費用、本番操作の判断

Codexへ実装を任せない。  
Claude Codeだけで実装前の広範囲調査を完結させない。  
Codexの評価中にCodex自身へコードを修正させない。

実装上の基準は「既存コードをどれだけ残すか」ではなく、

- ユーザー依頼
- 現在有効な正本
- 実際に達成すべき動作
- 維持すると正本で決まっている性質

に対して自然で一貫した実装になっているかとする。

## 1. 開始条件

引数または直前のユーザー指示を今回の実装依頼として扱う。

許可しないもの:

- 依頼と無関係な変更
- commit / push
- 本番操作、課金を伴う操作
- 共有環境やリポジトリ外の変更
- 正本にない仕様を仮定で決めること
- 外部実測でしか分からない事項を未確認のまま仕様化すること

**1回の起動で扱う実装単位は1つだけ。**

`USER_DECISION_REQUIRED` は質問して止まる。判断待ちの単位を「実装済み」と報告しない。

Codex調査前にはClaude Code自身が関連実装を広範囲に読まない。起点pathを特定するための軽い検索だけを許す。

## 2. Codex MCPを準備する

repositoryの詳細調査より先に、Codexの新規threadと継続threadのtool schemaを取得する。

どちらかを利用できない場合は作業を開始しない。ClaudeのsubagentやClaude単独の横断調査で代替しない。

Codexへは長い文書本文を転記しない。渡すのは:

- ユーザー依頼
- 依頼ファイルのpath
- 正本 / handoffの起点path
- repository内の必要な起点path

秘密情報や認証情報は含めない。

## 3. Codexに実装前調査を依頼する

新しいCodex threadを1本開始し、読み取り専用で調査させる。

禁止:

- file create / edit / delete
- commit / push
- 新しい設計文書の作成

Codex自身に現在有効な正本、関連コード、呼出経路、副作用、永続化、監査、検証方法を調査させる。

返却契約:

```text
STATUS: READY | USER_DECISION_REQUIRED | BLOCKED

REQUIREMENTS
- R1 | 根拠 | 満たすべき動作 | 完了証拠

TARGET_RESPONSIBILITIES
CURRENT_PATH
INVARIANTS
ROOT_CAUSES
EXISTING_FIT
CHANGE
REPLACE_OR_REMOVE
ADD
RISKS
IMPLEMENTATION_ORDER
VERIFY
UNRESOLVED
```

`REUSE` を独立した目的として探させない。

既存構造が今回の要求にそのまま適合する場合だけ `FIT` とする。

## 4. Codex調査後にClaude Codeが読む

Codexが `READY` を返してからClaude Code自身のコード読取を開始する。

repository全体を読み直さず、Codexが返した要求・責務・現行経路・根本原因・変更候補・検証対象を中心に読む。

Codexの返却を無条件に信じない。正本・コード・依頼と照合する。

## 5. 実装する

- 症状ではなく根本原因を直す
- 変更量の小ささを評価基準にしない
- 現在の要求に不要な将来機能を先回りしない
- 仕様が決まらない場合は止まって質問する
- 既存interfaceや既存共通部品を残すこと自体を目的にしない

## 6. 実行して確かめる

修正後はrepositoryが定める実際の入口と検証レーンを通す。

未実行を成功として報告しない。

外部環境でしか確認できない項目は、ローカル検証と分離して報告する。

## 7. Codexに独立評価させる

実装後、別の評価としてCodexに確認させる。

Codex自身には修正させない。

評価観点:

- REQUIREMENTSを満たしたか
- ROOT_CAUSEを解消したか
- 旧経路や二重経路が残っていないか
- INVARIANTSを壊していないか
- 不要な先回り実装が入っていないか
- 実行した検証は完了証拠として十分か

## 8. 補正と終了

妥当な指摘だけをClaude Codeが補正し、再実行する。

同じ問題を根拠なく反復しない。

最終報告:

```text
IMPLEMENTED
VERIFIED
CODEX_EVALUATION
UNRESOLVED
NOT_EXECUTED
```

commit / pushはしない。
