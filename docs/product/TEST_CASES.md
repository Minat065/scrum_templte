# Test Cases

## 基本テスト

| コマンド | 期待結果 |
|---------|---------|
| `./program` | |

## エッジケース

| コマンド | 期待結果 |
|---------|---------|
| | |

## エラーケース（不正な引数）

| コマンド | 期待結果 |
|---------|---------|
| `./program` | エラーメッセージ |

---

## valgrindとは

### 概要

valgrindは**動的解析ツール**。プログラムを実際に実行しながら、メモリやスレッドの問題を検出する。

コンパイル時にはわからない「実行時の問題」を見つけるために使う。

### なぜ使うのか

**1. 目に見えないバグを見つける**

実行時のバグは：

- 毎回再現しない（タイミング依存）
- 普通に動いているように見えても壊れている
- 本番環境でだけ発生する

valgrindはこれらを**実行中に監視して検出**する。

**2. 42課題の要件**

メモリリークやデータレースがないことを証明する手段として必須。

---

## データレースチェック（helgrind）

### データレースとは

複数スレッドが同時に同じメモリにアクセスし、少なくとも1つが書き込みで、適切な同期がない状態。

### helgrindの役割

- mutexのlock/unlockを追跡
- 保護されていないメモリアクセスを検出
- 「このアクセスは危険」と警告

### 使い方

```bash
valgrind --tool=helgrind ./program args
```

### 出力の読み方

```
==1234== ERROR SUMMARY: 0 errors from 0 contexts
```

- **0 errors**: データレースなし（OK）
- **N errors**: N箇所で問題あり（要修正）

### レビュー時のチェックポイント

1. `ERROR SUMMARY: 0 errors` か確認
2. エラーがあれば、どの変数でどの関数間か確認
3. その変数にmutex保護があるか確認

---

## メモリリークチェック

### メモリリークとは

mallocしたメモリをfreeし忘れること。

### 使い方

```bash
valgrind --leak-check=full ./program args
```

### 出力の読み方

```
==1234== HEAP SUMMARY:
==1234==     in use at exit: 0 bytes in 0 blocks
==1234==   total heap usage: 10 allocs, 10 frees, 1,024 bytes allocated
==1234== All heap blocks were freed -- no leaks are possible
```

- **in use at exit: 0 bytes**: リークなし（OK）
- **All heap blocks were freed**: 完璧

### レビュー時のチェックポイント

1. `definitely lost: 0 bytes` か確認
2. リークがあれば `--show-leak-kinds=all` で詳細確認
3. どのmallocがfreeされていないか特定

---

## レビュー時のコマンド一覧

```bash
# 1. ビルド確認
make re

# 2. norminette
norminette *.c *.h

# 3. 基本動作
./program args

# 4. データレースチェック（スレッド使用時）
valgrind --tool=helgrind ./program args

# 5. メモリリークチェック
valgrind --leak-check=full ./program args
```
