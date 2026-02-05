# FEセルフレビュー一式（pnpm + Playwright）共通方針

## 目的
変更が仕様どおりで回帰がなく、品質（a11y/性能/セキュリティ/保守性）が担保されていることを確認する。

## 共通制約
- 調査は原則 `git diff` の変更ファイルから開始し、必要がある場合のみ依存先へ広げる（理由を明記）。
- 変更されていない領域を広く読み込まない。
- コード変更は行わない。必要なら「最小修正案」を提示するだけ。
- 高ボリュームの実行ログは verification-runner に隔離し、メインには要約のみ返す。

## 並列化
- 観点別のサブエージェントを同時に動かし、メインは結果を集約する。

## 重大度
- Critical: マージ不可（セキュリティ/データ破壊/重大回帰）
- High: 早期に修正推奨（ユーザー影響大）
- Medium: 余裕があれば修正
- Low: 参考/改善提案

## 出力ルール
- 各指摘は file:line を含める（可能なら）。
- 根拠（diff/実行結果/推測）を明確にする。
- 修正案は最小差分。

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
