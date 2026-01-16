# Constraints

## norm規約

### 関数
- 1関数25行以内
- 1関数4引数以内
- 1行80文字以内

### 変数
- 宣言は関数先頭
- 1行1宣言
- 初期化は宣言と別行

### ファイル
- 1ファイル5関数以内
- ヘッダにはプロトタイプ、マクロ、typedef、structのみ

### 禁止事項
- for文禁止（whileを使う）
- switch文禁止
- 三項演算子の入れ子禁止
- グローバル変数禁止

## 許可関数

| 関数 | ヘッダ | 用途 |
|------|--------|------|
| memset | string.h | メモリ初期化 |
| printf | stdio.h | 出力 |
| malloc | stdlib.h | メモリ確保 |
| free | stdlib.h | メモリ解放 |
| write | unistd.h | 低レベル出力 |
| usleep | unistd.h | マイクロ秒スリープ |
| gettimeofday | sys/time.h | 現在時刻取得 |
| pthread_create | pthread.h | スレッド作成 |
| pthread_detach | pthread.h | スレッド切り離し |
| pthread_join | pthread.h | スレッド終了待ち |
| pthread_mutex_init | pthread.h | mutex初期化 |
| pthread_mutex_destroy | pthread.h | mutex破棄 |
| pthread_mutex_lock | pthread.h | mutexロック |
| pthread_mutex_unlock | pthread.h | mutexアンロック |

## メモリ管理

- malloc したら必ず free
- 二重 free 禁止
- 解放後のポインタは NULL 代入
- valgrind で検証必須

## プログラム仕様

### 引数
```
./philo number_of_philosophers time_to_die time_to_eat time_to_sleep [number_of_times_each_philosopher_must_eat]
```

- number_of_philosophers: 哲学者の数（= フォークの数）
- time_to_die: 最後の食事開始から死亡までの時間（ms）
- time_to_eat: 食事にかかる時間（ms）
- time_to_sleep: 睡眠時間（ms）
- number_of_times_each_philosopher_must_eat: オプション、全員がこの回数食べたら終了

### ログ形式
```
timestamp_in_ms X has taken a fork
timestamp_in_ms X is eating
timestamp_in_ms X is sleeping
timestamp_in_ms X is thinking
timestamp_in_ms X died
```

### 重要な制約
- **データレース禁止**: プログラムにデータレースがあってはならない
- **死亡表示は10ms以内**: 哲学者の死亡から10ms以内に表示
- **ログの重複禁止**: メッセージが重ならないこと

## コンパイル

- フラグ: -Wall -Wextra -Werror
- リンク: -pthread
- 警告は全てエラー扱い

## 提出

- ディレクトリ: philo/
- 必須ファイル: Makefile, *.h, *.c
- Makefile ルール: NAME, all, clean, fclean, re
