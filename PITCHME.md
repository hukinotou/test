### 1時間で作るlaravelアプリケーション

---

### 目次

- 今回のゴール
- 命名規則
- DB構造
- コーディング

---

### 今回のゴール

+++

@snap[north-west]
今回のゴール
@snapend

ライブコーディングで掲示板の作成を行い、簡単なアプリケーションの作成の流れを理解する。  

+++

@snap[north-west]
今回のゴール
@snapend

作成する掲示板  
[http://192.168.1.124/laravel-bbs/public/](http://192.168.1.124/laravel-bbs/public/)

ソースコード  
[https://github.com/hukinotou/laravel-bbs](https://github.com/hukinotou/laravel-bbs)

+++

@snap[north-west]
今回のゴール
@snapend

### 前提条件

- laravelのインストールが終わっていること
- 基盤に関連するようなところは話しません

---

### 命名規則

+++

@snap[north-west]
命名規則
@snapend

### コントローラー

| 項目 | 値 |
|---|---|
| namespace |  `app/Http/Controllers` |
| 書き方 | パスカルケース + Controller |
| 複数/単数 | 単数形 |
| 例 | UserController |

+++

@snap[north-west]
命名規則
@snapend

### モデル

| 項目 | 値 |
|---|---|
| namespace |  `app` ※ |
| 書き方 | パスカルケース |
| 複数/単数 | 単数形 |
| 例 | User |

@size[0.7em](※ 標準では`app`ですが、今回は`app/Models`としました)

+++

@snap[north-west]
命名規則
@snapend

### DBテーブル

| 項目 | 値 |
|---|---|
| 書き方 | スネークケース |
| 複数/単数 | 複数形 |
| 例 | users |

+++

@snap[north-west]
命名規則
@snapend

### URL

| 項目 | 値 |
|---|---|
| 書き方 | ハイフネーション |
| 複数/単数 | 複数形 |
| 例 | http://example.com/users/3 |

---

### DB構造

@size[0.7em](今回はSQLiteを使用します)

+++

@snap[north-west]
DB構造
@snapend

@snap[west]
threads テーブル

| カラム名 | 型 | 主キー |
|---|---|---|
| id | int | ○ |
| subject | text | |

@snapend

@snap[east]
responses テーブル

| カラム名 | 型 | 主キー |
|---|---|---|
| id | int | ○ |
| thread_id | int | |
| response | text | |

@snapend
