# Architecture

## 全体フロー

```
main()
  │
  ├─ parse_args()        引数をパース・検証
  │
  ├─ init_data()         構造体・mutex・スレッド初期化
  │
  ├─ start_simulation()  シミュレーション開始
  │     │
  │     ├─ 哲学者スレッド × N本 (philo_routine)
  │     │     └─ eat → sleep → think のループ
  │     │
  │     └─ メインスレッドで死亡監視 (monitor)
  │           └─ 全員の最終食事時刻をチェック
  │
  └─ cleanup_data()      リソース解放
```

## ファイル構成

| ファイル | 責務 |
|---------|------|
| main.c | エントリポイント、全体制御 |
| parse.c | 引数パース・バリデーション |
| init.c | 構造体・mutex・哲学者の初期化と解放 |
| time.c | 時間管理（現在時刻取得、経過時間、スリープ） |
| utils.c | ユーティリティ（ft_atoi_safe、print_status） |
| philo.c | 哲学者スレッドのメインルーチン |
| actions.c | 哲学者のアクション（eat、sleep、think） |
| monitor.c | シミュレーション開始・死亡監視 |

## 主要関数

### parse.c
| 関数 | 役割 |
|------|------|
| `parse_args` | argc/argvをパースしてt_dataに格納 |
| `validate_args` | 値の範囲チェック（正の整数か等） |

### time.c
| 関数 | 役割 |
|------|------|
| `get_time` | 現在時刻をミリ秒で取得 |
| `get_elapsed_time` | シミュレーション開始からの経過時間 |
| `ft_usleep` | 死亡チェックしながらスリープ（10ms要件対応） |

### utils.c
| 関数 | 役割 |
|------|------|
| `ft_atoi_safe` | 文字列→int変換（エラーチェック付き） |
| `print_status` | ログ出力（mutex保護、死亡後は出力しない） |

### init.c
| 関数 | 役割 |
|------|------|
| `init_data` | 全mutex・哲学者配列を初期化 |
| `cleanup_data` | 全リソースを解放 |

### philo.c
| 関数 | 役割 |
|------|------|
| `philo_routine` | 各哲学者スレッドのメインループ |

### actions.c
| 関数 | 役割 |
|------|------|
| `philo_eat` | フォーク取得→食事→フォーク解放 |
| `philo_sleep` | 睡眠 |
| `philo_think` | 思考 |

### monitor.c
| 関数 | 役割 |
|------|------|
| `start_simulation` | スレッド生成・死亡監視・スレッド回収 |

## データ構造

### t_data（シミュレーション全体の共有データ）
```c
typedef struct s_data
{
    int             num_philos;      // 哲学者の数
    int             time_to_die;     // 死亡までの時間(ms)
    int             time_to_eat;     // 食事時間(ms)
    int             time_to_sleep;   // 睡眠時間(ms)
    int             must_eat_count;  // 必要食事回数(-1なら無制限)
    int             someone_died;    // 死亡フラグ
    int             all_ate;         // 全員食べ終わりフラグ
    long long       start_time;      // シミュレーション開始時刻
    pthread_mutex_t *forks;          // フォーク用mutex配列
    pthread_mutex_t print_mutex;     // printf保護用
    pthread_mutex_t death_mutex;     // someone_died保護用
    pthread_mutex_t meal_mutex;      // 食事カウント保護用
    t_philo         *philos;         // 哲学者配列
}   t_data;
```

### t_philo（各哲学者のデータ）
```c
typedef struct s_philo
{
    int             id;              // 哲学者番号(1〜N)
    int             meals_eaten;     // 食事回数
    long long       last_meal_time;  // 最後に食事を開始した時刻
    pthread_t       thread;          // スレッド
    pthread_mutex_t *left_fork;      // 左フォークへのポインタ
    pthread_mutex_t *right_fork;     // 右フォークへのポインタ
    t_data          *data;           // 共有データへのポインタ
}   t_philo;
```

## mutex設計

| mutex | 守るもの | 理由 |
|-------|---------|------|
| `forks[i]` | 各フォークの使用権 | 同時に2人が同じフォークを取れないように |
| `print_mutex` | printf出力 | ログが混ざらないように |
| `death_mutex` | someone_died | 死亡判定のデータレース防止 |
| `meal_mutex` | meals_eaten, last_meal_time | 食事カウントのデータレース防止 |

## デッドロック回避戦略

哲学者が全員同時に左フォークを取ると、誰も右フォークを取れずデッドロック。

**対策：偶数番号と奇数番号でフォーク取得順を変える**
- 奇数番号: 左→右
- 偶数番号: 右→左

これにより循環待ちを防ぐ。
