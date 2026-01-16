# 42 Tokyo AiDD Scrum Template

42 Tokyo課題をClaude Codeと共に開発するためのテンプレートです。
AiDD（AI-Driven Development）とスクラム的な進め方で、効率的かつ学習効果の高い開発を実現します。

## このテンプレートの特徴

- **ドキュメント駆動**: 意思決定・学び・進捗を全て記録
- **イテレーション型開発**: 小さく作って動かして学ぶサイクル
- **Claude Code統合**: スラッシュコマンドで開発フローを自動化
- **振り返り重視**: 実装だけでなく「なぜ」「何を学んだか」を蓄積

## 使い方

### 1. 新しい課題を開始

```bash
# 1. このテンプレートをコピー
cp -r scrum_template <課題名>
cd <課題名>

# 2. subject.pdfを配置
cp /path/to/subject.pdf docs/product/

# 3. Claude Codeでプロジェクト初期化
# Claude Codeを起動して以下のコマンドを実行
/init-project
```

### 2. 開発セッション開始

毎回のコーディングセッション開始時：

```
/start
```

Claudeが以下を自動で行います：
- 前回のセッションログを読み込み
- 現在のイテレーション目標を確認
- 次にやるべきことを提案

### 3. 開発サイクル

```
1. イテレーション目標を設定（docs/iterations/current/GOAL.md）
2. 小さく実装・テスト
3. 動いたら振り返り
4. 学びを記録（/learn コマンド）
```

### 4. セッション終了

```
/session-end
```

Claudeが以下を自動で行います：
- セッションログを docs/sessions/ に保存
- CLAUDE.md の「現在の状況」を更新
- 次回のアクションを明記

### 5. イテレーション完了時

```
/retro
```

振り返りを実施し、次のイテレーションに向けて学びを整理します。

### 6. 提出前の仕上げ

```
/finish
```

レビュー準備モードに入り、以下をチェック：
- norminette
- valgrind（メモリリーク・データレース）
- テストケース網羅性
- ドキュメント最終更新

## ディレクトリ構造

```
.
├── CLAUDE.md                    # Claudeへの指示・プロジェクト状況
├── README.md                    # このファイル
├── .claude/
│   └── commands/               # スラッシュコマンド定義
└── docs/
    ├── WORKFLOW.md             # 開発フロー詳細
    ├── product/                # プロダクト要求・仕様
    │   ├── subject.pdf         # 課題原文（配置必要）
    │   ├── VISION.md           # この課題で何を学ぶか
    │   ├── BACKLOG.md          # タスク一覧
    │   ├── TEST_CASES.md       # テストケース・valgrind使い方
    │   └── DEFINITION_OF_DONE.md
    ├── architecture/           # 技術設計
    │   ├── ARCHITECTURE.md     # 全体設計
    │   ├── CONSTRAINTS.md      # norm規約・許可関数
    │   └── DECISIONS.md        # アーキテクチャ決定記録
    ├── iterations/             # イテレーション管理
    │   ├── current/
    │   │   └── GOAL.md         # 今のイテレーション目標
    │   └── archive/            # 過去イテレーションの記録
    ├── learning/               # 学習記録
    │   ├── INTENT.md           # 課題の意図分析
    │   ├── LEARNINGS.md        # 学んだこと
    │   ├── EXAM_PREP.md        # 試験対策メモ
    │   └── SCRUM_NOTE.md       # スクラム運用の気づき
    └── sessions/               # セッションログ（自動生成）
```

## 主要ドキュメント

### プロジェクト管理

- **CLAUDE.md**: Claudeが常に参照。プロジェクト概要・現在の状況
- **docs/product/VISION.md**: このプロジェクトで何を達成するか
- **docs/product/BACKLOG.md**: やるべきタスクの優先順位付きリスト

### 設計・制約

- **docs/architecture/ARCHITECTURE.md**: ファイル構成・データ構造・設計判断
- **docs/architecture/CONSTRAINTS.md**: norm規約・許可関数・プログラム仕様
- **docs/architecture/DECISIONS.md**: なぜその設計にしたか

### 学習

- **docs/learning/INTENT.md**: 42がこの課題で何を学ばせたいか（推測）
- **docs/learning/LEARNINGS.md**: 実装中に学んだこと
- **docs/learning/EXAM_PREP.md**: 試験で聞かれそうなこと

## スラッシュコマンド一覧

| コマンド | タイミング | 目的 |
|---------|-----------|------|
| `/init-project` | プロジェクト開始時 | subject.pdf分析・ドキュメント草案作成 |
| `/start` | セッション開始時 | 前回の続きから再開 |
| `/session-end` | セッション終了時 | ログ保存・CLAUDE.md更新 |
| `/learn` | 気づきがあった時 | 学びをLEARNINGS.mdに記録 |
| `/retro` | イテレーション完了時 | 振り返り・次の計画 |
| `/finish` | 提出前 | レビュー準備・試験対策 |

## ワークフロー例

### 初回（プロジェクト立ち上げ）

1. subject.pdfを `docs/product/` に配置
2. `/init-project` でドキュメント生成
3. 生成されたVISION.md, BACKLOG.mdをレビュー
4. 最初のイテレーション目標を設定

### 通常の開発サイクル

1. `/start` でセッション開始
2. `docs/iterations/current/GOAL.md` を確認
3. 小さく実装（1機能ずつ）
4. テスト・動作確認
5. 学びがあれば `/learn`
6. `/session-end` で終了

### イテレーション完了時

1. `/retro` で振り返り
2. 次のイテレーション目標を設定
3. アーカイブに記録

## Tips

### Claudeとの協働のコツ

- **事前相談**: 実装前に設計を相談（特に難しい箇所）
- **小さく進める**: 一度に全部作らない。1関数ずつ動かす
- **学びを言語化**: 「わかった気」で終わらせず、言葉で説明する

### ドキュメントの使い方

- **DECISIONS.md**: 迷った設計判断は必ず記録（後で役立つ）
- **LEARNINGS.md**: 試験で聞かれそうなことを意識して書く
- **INTENT.md**: 課題の隠れた意図を考える（深い学びに繋がる）

### 振り返りの重要性

- コードが動いただけで終わらない
- 「なぜそうしたか」「何を学んだか」を明文化
- 次の課題・試験に活きる財産になる

## 承認フロー

このテンプレートでは、以下の操作は**あなたの承認が必要**です：

- ✅ gitコミット・プッシュ
- ✅ ドキュメントの大幅な変更
- ✅ アーキテクチャの重要な決定

Claudeは提案まで行い、最終判断はあなたが行います。

## カスタマイズ

### スラッシュコマンドの編集

`.claude/commands/` 配下のマークダウンファイルを編集してください。

例: `/start` の動作を変更したい場合
→ `.claude/commands/start.md` を編集

### ドキュメント構造の変更

`docs/` 配下の構造は自由に変更できます。
ただし、CLAUDE.mdの「参照ドキュメント」セクションも併せて更新してください。

## トラブルシューティング

### スラッシュコマンドが動かない

- `.claude/commands/` にファイルが存在するか確認
- Claude Codeを再起動

### Claudeが古い情報を参照している

- CLAUDE.mdの「現在の状況」を手動更新
- `/start` で最新状況を読み込み直し

### ドキュメントが多すぎて管理できない

- 最低限必要なのは: VISION, BACKLOG, CONSTRAINTS, ARCHITECTURE
- 他は必要に応じて使用（全部埋める必要はない）

## ライセンス

このテンプレートは自由に使用・改変してください。

## 参考

- [42 Tokyo](https://42tokyo.jp/)
- [Claude Code](https://claude.com/claude-code)

---

**良い学びと良いコードを。**
