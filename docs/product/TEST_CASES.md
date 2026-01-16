# Test Cases

## 基本テスト

| コマンド | 期待結果 |
|---------|---------|
| `./philo 5 800 200 200` | 誰も死なない、永続実行 |
| `./philo 5 800 200 200 7` | 全員7回食べて終了 |
| `./philo 1 800 200 200` | 800ms後に死亡（フォーク1つしかない） |
| `./philo 4 410 200 200` | 誰も死なない |
| `./philo 4 310 200 100` | 誰かが死ぬ |

## 死亡検出テスト（10ms以内）

| コマンド | 確認ポイント |
|---------|-------------|
| `./philo 4 310 200 100` | time_to_die(310) から 10ms以内に死亡メッセージ |

## エッジケース

| コマンド | 期待結果 |
|---------|---------|
| `./philo 1 200 200 200` | 200ms後に死亡 |
| `./philo 2 800 200 200` | 誰も死なない |
| `./philo 200 800 200 200` | 誰も死なない（大人数） |

## エラーケース（不正な引数）

| コマンド | 期待結果 |
|---------|---------|
| `./philo` | エラーメッセージ |
| `./philo 5` | エラーメッセージ |
| `./philo 5 800 200` | エラーメッセージ |
| `./philo -5 800 200 200` | エラーメッセージ |
| `./philo 5 -800 200 200` | エラーメッセージ |
| `./philo 0 800 200 200` | エラーメッセージ |
| `./philo abc 800 200 200` | エラーメッセージ |

---

## valgrindとは

### 概要
valgrindは**動的解析ツール**。プログラムを実際に実行しながら、メモリやスレッドの問題を検出する。

コンパイル時にはわからない「実行時の問題」を見つけるために使う。

### なぜ使うのか

**1. 目に見えないバグを見つける**

マルチスレッドプログラムのバグは：
- 毎回再現しない（タイミング依存）
- 普通に動いているように見えても壊れている
- 本番環境でだけ発生する

valgrindはこれらを**実行中に監視して検出**する。

**2. subjectの要件**

> Your program must not have any data races.

データレースがないことを証明する手段として必須。

---

## データレースチェック（helgrind）

### データレースとは
複数スレッドが同時に同じメモリにアクセスし、少なくとも1つが書き込みで、適切な同期がない状態。

```c
// スレッド1          // スレッド2
someone_died = 1;    if (someone_died)  // 同時アクセス = データレース
```

### helgrindの役割
- mutexのlock/unlockを追跡
- 保護されていないメモリアクセスを検出
- 「このアクセスは危険」と警告

### 使い方
```bash
valgrind --tool=helgrind ./philo 5 800 200 200 3
```

### 出力の読み方
```
==1234== ERROR SUMMARY: 0 errors from 0 contexts
```
- **0 errors**: データレースなし（OK）
- **N errors**: N箇所で問題あり（要修正）

### エラーが出た場合の例
```
==1234== Possible data race during read of size 4 at 0x...
==1234==    at 0x...: check_death (monitor.c:20)
==1234==  This conflicts with a previous write of size 4 by thread #3
==1234==    at 0x...: set_death_flag (philo.c:45)
```
→ `monitor.c:20` と `philo.c:45` で同じ変数に同期なしでアクセスしている

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
valgrind --leak-check=full ./philo 5 800 200 200 3
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

### リークがある場合の例
```
==1234== LEAK SUMMARY:
==1234==    definitely lost: 128 bytes in 2 blocks
==1234==    indirectly lost: 0 bytes in 0 blocks
```
→ 128バイト（2箇所）がfreeされていない

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
./philo 5 800 200 200 7

# 4. 死亡テスト
./philo 4 310 200 100

# 5. データレースチェック
valgrind --tool=helgrind ./philo 5 800 200 200 3

# 6. メモリリークチェック
valgrind --leak-check=full ./philo 5 800 200 200 3
```
