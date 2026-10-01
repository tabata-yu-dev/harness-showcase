# Codexの返却物の契約

> Showcase extract: 実運用の圧縮契約を公開可能な範囲で残している。

この契約を読むのは `/review` だけ。

## Thread structure

- Round 1: 責務地図
- Round 2: 正本から母集団を引き直す独立走査
- Reproducer-Integrator: 両Roundの候補を再現・統合

Round 2へRound 1の候補・返却・判断・threadIdを渡さない。

## Returned data

Round完了時は圧縮した `ROUND_SUMMARY` だけを受け取る。

走査過程、ファイル全文、テスト出力全文を返させない。

### 必須

```text
STATUS
ROUND
UNITS
DECLARED
EVIDENCE
DIVERGENCE
THICKNESS
PROBES
TESTS
CANDIDATES
OBLIGATIONS
SCANNED
UNSCANNED
CAP
```

### EVIDENCE

論理単位ごとに、その周が実際に実行した反例を1行返す。

候補0件でも必須。

### DIVERGENCE

Round 2だけ:

- 正本にあるが宣言に無い軸
- 宣言にあるが正本に出所を持たない軸

差がなければ `none` と、突き合わせた正本sectionを返す。

### CAP

上限に達した場合は `CAP: reached` と明示する。

切り捨てを黙って成功扱いしない。

## Write boundary

Round 1 / Round 2はfileを書かない。

Review本体はコード修正を行わない。

Review中にコード領域へ意図しない差分が出た場合、そのRoundの結果を無条件に採用しない。
