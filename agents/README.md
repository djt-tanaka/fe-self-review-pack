# FEセルフレビュー一式（pnpm + Playwright）

- group: fe-self-review-pack
- version: 1.0.0
- created: 2026-02-03

## 目的
フロントエンドの変更を、観点別のサブエージェントで並列にセルフレビューする。

## 構成（必須）
- spec-behavior-reviewer: 仕様/挙動/状態遷移
- ui-responsive-reviewer: UI/レスポンシブ
- a11y-reviewer: アクセシビリティ
- perf-reviewer: 性能
- security-reviewer: セキュリティ
- maintainability-reviewer: 保守性
- verification-runner: 実行（lint/typecheck/test/build/e2e）

## 追加（任意）
- api-contract-reviewer
- state-management-reviewer
- i18n-l10n-reviewer
- design-system-reviewer
- pr-writer

## 使い方
- 起点コマンド: `/fe-self-review-pack:review <対象PRや説明>`
- 共通方針: skills/fe-self-review/SKILL.md を参照
