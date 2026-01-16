# CLAUDE.md

## プロジェクト概要

- 課題名:
- 目的:
- 現在のフェーズ: Phase 0 - プロジェクト立ち上げ

## 技術スタック

- 言語: C
-

## 制約

- 42 Tokyo norm規約準拠
-

## コマンド

- `/init-project` - プロジェクト初期化
- `/start` - セッション開始
- `/session-end` - セッション終了
- `/retro` - イテレーション振り返り
- `/finish` - 仕上げモード
- `/learn` - 学習の気づきを記録

## ワークフロー

1. セッション開始時は `/start` で状況把握
2. 実装はイテレーション単位で計画
3. コードは小さな単位で実装・テスト
4. 私の承認なしにコミットしない
5. セッション終了時は `/session-end` でログ保存

## ドキュメント規約

- docs/ 配下は常に最新を維持
- 意思決定は必ず DECISIONS.md に記録
- 学びは LEARNINGS.md に蓄積
- 他人が読む前提で書く

## 参照ドキュメント

- docs/product/VISION.md - ゴールと背景
- docs/product/BACKLOG.md - タスク優先順位
- docs/product/TEST_CASES.md - テストケース・valgrind使い方
- docs/architecture/CONSTRAINTS.md - 制約詳細
- docs/architecture/ARCHITECTURE.md - 全体設計
- docs/iterations/current/GOAL.md - 今のイテレーション目標
- docs/learning/INTENT.md - 課題の意図

## 現在の状況

- 最終更新:
- 進行中タスク: プロジェクト初期化待ち
- ブロッカー: なし
- 次のアクション: subject.pdf配置後、`/init-project`実行 
