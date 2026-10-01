# 検証システム

> Showcase extract — 実際のlane構造と「未実行を成功にしない」契約を残す。

## 1. 目的

実装の安全性・再現性・文書整合性を確認する検査の実行定義を一箇所へ集約する。

概念:

```text
verification manifest
        ↓
verification runner
        ↓
quick / review / ci / push / network
```

同じ検査commandを複数文書へ複写しない。

## 2. 検査lane

| lane | 実行時点 | 主な性質 |
| --- | --- | --- |
| quick / review / ci | 編集区切り・review前・integration前 | compile / durability / docs / secrets / skills |
| push | push前 | safety logic |
| network | 外部到達性が必要なとき | explicit network checks |

quick / review / ciは同じ検査集合を使う。

### 2-1. lane結果は3値

成功 / 失敗の二値にしない。

| 結果 | 意味 |
| --- | --- |
| pass | 実行して合格 |
| fail | 実行して不合格 |
| not executed | 必須だが実行できなかった |

**未実行をpassへ丸めない。**

Network-dependent checkは明示的に有効化されない限り`not executed`。

## 3. 守る性質

代表例:

- secretsをtracked file / normal log / artifactへ出さない
- UTC / DSTをhost timezoneへ依存させない
- append-only recordを安全にrestoreできる
- unresolved stop reasonがある間は新規side effectを許可しない
- public authority decisionを一つにする
- cache / journal / memory / projectionが単独でpermissionを作らない
- Strategy / CLIからExecution adapterへ直接到達しない
- Intent → Order → Fill → Position reconciliationを追跡できる
- sibling / reverse / restart / double failure / parallel pathで同じ性質を守る

## 4. 設計規則

通常系だけでなく:

- missing input
- duplicate
- reorder
- write failure
- disconnect
- restart
- DST boundary
- parallel path

を対象にする。

状態・権限・監査・永続化・復元を変更した場合、修正箇所だけを確認しない。

責務地図から兄弟・逆方向・failure pathまで確認する。

## 5. AI Skillも検査対象

`.claude/skills/*/SKILL.md` 自体もrepository contractの一部。

Skill name / frontmatter / workflow boundaryが崩れていないかをverification対象にする。
