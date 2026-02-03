# FEセルフレビュー一式（pnpm + Playwright）共通方針

## 目的
変更が仕様どおりで回帰がなく、品質（a11y/性能/セキュリティ/保守性）が担保されていることを確認する。

## コンテキスト最小化
- 調査は原則 `git diff` の変更ファイルから開始し、必要がある場合のみ依存先へ広げる（理由を明記）。
- 変更されていない領域を広く読み込まない。

## 並列化
- 観点別のサブエージェントを同時に動かし、メインは結果を集約する。
- 高ボリュームの実行ログは verification-runner に隔離し、メインには要約のみ返す。

## 重大度
- Critical: マージ不可（セキュリティ/データ破壊/重大回帰）
- High: 早期に修正推奨（ユーザー影響大）
- Medium: 余裕があれば修正
- Low: 参考/改善提案

## 出力
- 各指摘は file:line を含める（可能なら）。
- 根拠（diff/実行結果/推測）を明確にする。
- 修正案は最小差分。

## エージェントファイル（必須7体）
以下の7体が本パックの中核：
spec-behavior-reviewer / ui-responsive-reviewer / a11y-reviewer / perf-reviewer / security-reviewer / maintainability-reviewer / verification-runner
