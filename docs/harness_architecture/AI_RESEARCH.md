# AI Research Architecture

## Overview

Aurea PhyllotaxyのResearch層では、AIを1つの万能エージェントとして扱わず、異なる役割へ分離します。

```text
                         Public Information
                                ↑
                                │
                         ┌──────┴──────┐
                         │    Codex    │
                         │  Research   │
                         └──────┬──────┘
                                │
┌────────────┐          ┌───────▼────────┐          ┌────────────┐
│  Evidence  ├─────────►│     Claude     │◄─────────┤   Gemini   │
└────────────┘          │ Analysis /     │          │ Refutation │
                        │ Synthesis      │          └────────────┘
                        └───────┬────────┘
                                │
                                ▼
                        ┌───────────────┐
                        │Research Result│
                        └───────┬───────┘
                                │
                                ▼
                         Candidate Layer
```

## Non-majority Design

複数モデルの一致を正しさとして扱いません。

CodexとGeminiは、Claudeの判断を補強するための「票」ではなく、異なるfailure modeを持つ独立した観点です。

最終統合は元Evidenceへ戻ります。

## Failure is explicit

外部Researchが失敗した場合や、入力権限が足りない場合は、Research Cycleを完全と偽装しません。

`unresolved` / `incomplete` は通常の結果として残します。
