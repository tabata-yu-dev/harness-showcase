---
name: review-fix
description: 指定レビュー記録のfindingと未閉鎖の完了義務を、一つの母集団として包括修正する。仕様が決めていないことはASKとして止まる。
---

# レビュー指摘の修正

> Showcase extract: private finding IDやGate名は省略。fix-mapper → main → verifier → post-scanner の実責務分離は原型を残している。

本線は最初にreview記録の所在と正本の入口だけを確認する。

広い読込みは `fix-mapper` へ委任し、本線へ全文を戻さない。

## 実行分離

| 実行者 | 役割 | 書込み |
|---|---|---:|
| `fix-mapper` | 広い読込み、ASK、責務地図、母集団、編集候補、検証範囲の圧縮 | 禁止 |
| 本線 | 仕様判断、修正方針、必要箇所の精読、意図的な編集 | 唯一許可 |
| `fix-verifier` | 修正後再現、verification lane、結果の圧縮 | 意図的変更は禁止 |
| `post-scanner` | 兄弟・逆方向・restart・failure・retry・parallel再走査 | 禁止 |

同じ役割のagentを場当たり的に複製しない。

## 1. 対象を一つの補正操作として確定

母集団は「指摘件数」ではなく「未閉鎖の完了義務」。

- 修正対象finding
- 同じfamilyの未閉鎖obligation
- review contract不足
- prior fixの再発
- tradeoff

をまとめて扱う。

`TRADEOFF_DEADLOCK` や未解決ASKがある場合は編集を始めない。

## 2. fix-mapper

`FIX_MAP`には最低限:

- FAMILY_ID
- OBLIGATION_ID
- invariant
- specification source
- population
- responsibility map
- edit candidates
- preserve paths
- excluded paths
- axis change
- reproduction scope
- tradeoff contract
- ASK / blockers

を含める。

対応先が決まらないobligationを要約で消さない。

## 3. 本線だけが編集

本線は必要な正本節と編集候補だけを精読する。

受入済みの仕様が決めているのに実装の軸が足りない場合は、軸を足して母集団を埋める。

仕様が決めていないことは設計して埋めず、ASKで止まる。

## 4. fix-verifier

全OBLIGATIONについて:

- 修正前の再現条件が発火しない
- 非退行条件が保たれる
- tradeoff両側が許容範囲

を確認する。

1件でも未実行なら `pass` にしない。

## 5. post-scanner

変更点を起点に:

- sibling
- reverse
- restart
- failure
- retry
- parallel

を再走査する。

再発は新規findingとして水増しせず、

- FIX_INCOMPLETE
- FIX_SCOPE_GAP
- TRADEOFF_DEADLOCK
- NEW_INDEPENDENT_FINDING

へ分類する。

## 6. 終了

全obligationが閉鎖されたときだけ補正を完了とする。

進展の無い反復は止める。
