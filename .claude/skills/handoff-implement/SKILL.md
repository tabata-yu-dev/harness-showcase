---
name: handoff-implement
description: HANDOFFの次の新規実装単位を、守る性質・責務地図・母集団の宣言・検証・引き継ぎ更新まで一体で実装する。
---

# 実装

> Showcase extract: Revenue固有の利益値・口座条件は除外。実装単位、責務地図、母集団、検証、handoff更新という本来の構造は残している。

編集前に、

- `AGENTS.md`
- `docs/handoff/HANDOFF.md`
- `docs/registers/`
- 対象phaseのsection仕様
- `docs/basic/` の正本
- review workflow

を確認する。

**このSkillは先に進む単位だけに使う。**
レビューで見つかった指摘の修正は `/review-fix` が持つ。

## 1. 実装単位を特定する

HANDOFFの「次に行うこと」から、新規実装単位を1つだけ特定する。

独立した複数単位を含む、または仕様判断が必要な場合は、勝手に狭めずユーザーへ返す。

## 2. 実装前に契約を確定する

編集前に次を明示する。

- 守る性質（invariants）
- 責務地図
- 意味的な兄弟責務
- 除外範囲
- 必要なレビュー水準

責務地図:

```text
entry
→ state / decision
→ canonical record / side effect
→ projection / reader
→ recovery / reverse path
→ parallel / failure boundary
```

### 母集団を実装の宣言として持つ

状態、許可、監査、永続化、復元、外部副作用、秘密情報に触れる場合:

1. 成功条件を肯定形で定義
2. 性質を破り得る軸を名指し
3. 各軸の値を列挙可能にする
4. 軸の積（cells）とnot-applicableを宣言
5. 実装のdocstring等へ所在を残す

「この関数を直した」ことを網羅性の根拠にしない。

## 3. 実装と検証

- 必要なコードとproject内設定だけを変更
- 宣言した母集団を意識して不変条件を維持
- 実際のentrypointを通して確認
- 保存・停止・復元は永続化後も確認
- 未実行・失敗・実行不能を成功扱いしない

## 4. Review Request / Handoff

ゲートでreviewを行う場合、レビュー依頼書には最低限:

- 守る性質と出所
- 責務地図
- 兄弟責務
- 母集団の宣言の所在
- 軸と全数
- 厚い / 薄い
- 再利用の対象
- 除外範囲
- verification commands
- result destination

を入れる。

HANDOFFは「次のセッションが実装事実を復元できる」量だけを残し、古くなった記述を足しっぱなしにしない。

## 禁止

- reviewそのものの実施
- review findingをこのSkillへ移す
- 無関係な欠陥の修正
- 共有環境変更
- repo外変更
- 仕様を弱めて実装に合わせる
- 未決の値を仮置きして完了扱いする
