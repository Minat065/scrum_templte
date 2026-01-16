---
description: イテレーション完了時の振り返り
allowed-tools: Read, Write, Edit, Bash
---

# レトロスペクティブ

イテレーション完了時に実行。

## 手順

### 1. 情報収集
- docs/iterations/current/GOAL.md を確認
- 関連するセッションログを確認
- 完了したタスクを確認

### 2. OUTCOME.md 作成
`docs/iterations/archive/XXX_[機能名]/OUTCOME.md` を作成

### 3. RETRO.md 草案作成
`docs/iterations/archive/XXX_[機能名]/RETRO.md` を作成
```markdown
# Retrospective: [機能名]

## Keep（続けること）
- [うまくいったこと]

## Problem（問題だったこと）
- [困ったこと、非効率だったこと]

## Try（次に試すこと）
- [改善案]

## 学び
[このイテレーションで得た教訓]

## プロセス改善
[AiDDワークフロー自体への改善点]
```

### 4. REVIEW.md テンプレート作成
Minatoが記入するための空テンプレートを作成

### 5. アーカイブ
- current/ の内容を archive/XXX_[機能名]/ に移動
- current/GOAL.md を空テンプレートに戻す

### 6. 振り返りレポート

Minatoに以下を報告：
```
## イテレーション完了: [機能名]

### 成果サマリー
- 

### 振り返りポイント
- Keep: 
- Problem: 
- Try: 

### 確認事項
- REVIEW.md の記入をお願いします
- 次のイテレーションのゴールを教えてください
```

## 注意
- REVIEW.md はMinatoが記入。草案を書かない
- 次のイテレーション開始はMinatoの指示を待つ