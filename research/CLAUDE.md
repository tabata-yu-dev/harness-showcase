# Research ワークスペース（AI Research）

ここは**市場EvidenceをResearchするClaude Code**の起動場所です。開発・実装・レビューを行うClaudeはrepository rootで起動し、ここを使いません。

- 入口は skill `ai-research`（[SKILL.md](.claude/skills/ai-research/SKILL.md)）
- 3エージェントResearchを1サイクル回し、段ごとにResearch Resultへ記録する
- 開発用skill・subagentはこのworkspaceでは無効化する
- このsessionは実装コード・本番設定・正本を変更しない
- Researchの機械評価・実績Dataset・Dashboardは外部AIへ渡さないため読ませない
- 直すべき点が見つかった場合は、Research sessionで修正せずDevelopment側へ返す

構成・役割・境界は `docs/` と `research/.claude/settings.json` を参照する。
