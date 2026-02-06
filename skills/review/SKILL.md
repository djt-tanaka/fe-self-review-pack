---
name: FEセルフレビュー
description: FE変更のセルフレビューを観点別サブエージェントで並列実行する
version: 1.0.0
---

# FEセルフレビュー一式（pnpm + Playwright）

## 目的
変更が仕様どおりで回帰がなく、品質（a11y/性能/セキュリティ/保守性）が担保されていることを確認する。

## 実行手順

### 1) 差分スナップショット（最小）
- git status
- git diff --stat
- git diff（長い場合は変更ファイル一覧と主要差分のみ）
- package.json の scripts 一覧

### 2) 並列ファンアウト
サブエージェントを同時に起動。各エージェントには変更ファイル一覧と主要入口だけ渡し、差分中心で調査させる。

- spec-behavior-reviewer
- ui-responsive-reviewer
- a11y-reviewer
- perf-reviewer
- security-reviewer
- maintainability-reviewer
- verification-runner（pnpmで lint/typecheck/test/build/e2e=Playwright を実行。ログは要約のみ）

### 3) 集約
- 結果を統合し、重大度定義に従い優先度順に並べる
- 未実施の検証があるなら理由と代替案を明記
- PR説明文ドラフト（何を/なぜ/どう検証したか/残リスク）を作成

## 共通制約
- 調査は原則 `git diff` の変更ファイルから開始し、必要がある場合のみ依存先へ広げる（理由を明記）。
- 変更されていない領域を広く読み込まない。
- コード変更は行わない。必要なら「最小修正案」を提示するだけ。
- 高ボリュームの実行ログは verification-runner に隔離し、メインには要約のみ返す。

## 重大度
- Critical: マージ不可（セキュリティ/データ破壊/重大回帰）
- High: 早期に修正推奨（ユーザー影響大）
- Medium: 余裕があれば修正
- Low: 参考/改善提案

## 出力フォーマット（厳守）

```
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
```

## エージェント構成

### 必須（7体）
| エージェント | 観点 |
|---|---|
| spec-behavior-reviewer | 仕様/挙動/状態遷移 |
| ui-responsive-reviewer | UI/レスポンシブ |
| a11y-reviewer | アクセシビリティ |
| perf-reviewer | 性能 |
| security-reviewer | セキュリティ |
| maintainability-reviewer | 保守性 |
| verification-runner | 実行（lint/typecheck/test/build/e2e） |

### 任意（5体）
| エージェント | 観点 |
|---|---|
| api-contract-reviewer | API契約/データ境界 |
| state-management-reviewer | 状態管理 |
| i18n-l10n-reviewer | 国際化/地域化 |
| design-system-reviewer | デザインシステム準拠 |
| pr-writer | PR本文作成 |
