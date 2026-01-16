---
description: Phase 0 - プロジェクト立ち上げ。VISION, BACKLOG, CONSTRAINTS等を作成
allowed-tools: Read, Write, Edit, Bash
---

# プロジェクト初期化

新しい42 Tokyo課題のAiDD環境を立ち上げます。

## 手順

### 1. 課題情報の収集

以下を確認させてください：

- 課題名は何ですか？
- docs/product/subject.pdf は配置済みですか？
- 締切はいつですか？

### 2. ドキュメント構造の作成

以下のディレクトリとファイルを作成します：
```
docs/
├── product/
│   ├── subject.pdf（事前配置）
│   ├── VISION.md
│   ├── BACKLOG.md
│   └── DEFINITION_OF_DONE.md
├── architecture/
│   ├── ARCHITECTURE.md
│   ├── DECISIONS.md
│   └── CONSTRAINTS.md
├── iterations/
│   └── current/
│       └── GOAL.md
├── learning/
│   ├── LEARNINGS.md
│   ├── INTENT.md
│   ├── EXAM_PREP.md
│   └── SCRUM_NOTES.md
├── sessions/
└── WORKFLOW.md
```

### 3. 各ドキュメントの草案作成

docs/product/subject.pdf を分析し、以下を作成します：

**VISION.md**
- この課題で何を学ぶか
- 完了時の自分の状態

**BACKLOG.md**
- 必要なタスクを洗い出し
- 優先度を付けて整理

**CONSTRAINTS.md**
- norm規約
- 許可関数リスト
- その他制約

**INTENT.md**
- 課題の意図（初期仮説）
- 各要件の隠れた意図

### 4. CLAUDE.md の初期化
プロジェクト概要と現在の状況を記載

### 5. 確認
作成したドキュメントを提示し、レビューを依頼します。

## 完了条件
- [ ] docs/product/subject.pdf が配置されている
- [ ] 全ドキュメントが作成されている
- [ ] Minatoのレビューを受けている
- [ ] 修正指示があれば反映済み

## 注意
- 草案作成後、必ずMinatoの承認を得る
- 不明点は推測せず確認する
- 課題の意図は仮説として記載（後で更新）