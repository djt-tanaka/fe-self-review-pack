---
description: FE変更のセルフレビューを観点別サブエージェントで並列実行する
---

次の手順でセルフレビューを開始して。

1) 入口：差分スナップショット（最小）
- git status
- git diff --stat
- git diff（長い場合は変更ファイル一覧と主要差分のみ）
- package.json の scripts 一覧

2) 並列ファンアウト（サブエージェントを同時に起動。各エージェントには変更ファイル一覧と主要入口だけ渡し、差分中心で調査させる）
- spec-behavior-reviewer
- ui-responsive-reviewer
- a11y-reviewer
- perf-reviewer
- security-reviewer
- maintainability-reviewer
- verification-runner（pnpmで lint/typecheck/test/build/e2e=Playwright を実行。ログは要約のみ）

3) 集約
- 結果を統合し、優先度順に並べる：Critical（マージ不可） / High / Medium / Low
- 未実施の検証があるなら理由と代替案を明記
- PR説明文ドラフト（何を/なぜ/どう検証したか/残リスク）を作成

出力フォーマット（厳守）
## Summary
- 変更の要約（3行以内）
- 総合判定: PASS / PASS(with risks) / FAIL

## Findings (priority order)
- [Critical|High|Medium|Low] 指摘タイトル — file:line
  - 根拠（diffの該当やテスト結果の要約）
  - 推奨修正（最小差分）

## Verification results
- 実行したコマンド一覧と結果（成功/失敗の要約のみ）

## PR description draft
- 箇条書きで短く

対象PR/変更内容:
$ARGUMENTS
