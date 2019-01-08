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

#### 今回のゴール

ライブコーディングで掲示板の作成を行い、簡単なアプリケーションの作成の流れを理解する。  

+++

@snap[north-west]
今回のゴール
@snapend

#### 成果物

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

#### threads テーブル

| カラム名 | 型 | 主キー |
|---|---|---|
| id | int | ○ |
| subject | text | |

+++

@snap[north-west]
DB構造
@snapend

#### responses テーブル

| カラム名 | 型 | 主キー |
|---|---|---|
| id | int | ○ |
| thread_id | int | |
| response | text | |

---

### 製造

- マイグレーション
- モデルの作成
- コントローラーの作成

---

### マイグレーション

+++

@snap[north-west]
マイグレーション
@snapend

#### ファイルの生成

```
php artisan make:migration create_threads_table
```

@size[0.7em](`database/migrations/2018_12_09_053117_create_threads_table.php` のファイルが生成されます)

+++

@snap[north-west]
マイグレーション
@snapend

#### カラムの定義

```
public function up()
{
    Schema::create('threads', function (Blueprint $table) {
        $table->increments('id');
        $table->text('subject');
        $table->timestamps();
    });
}
```
@[5](subjectをtext型で定義します)

+++

@snap[north-west]
マイグレーション
@snapend

#### マイグレーションの実行

```
php artisan migrate
```

---

### モデルの作成

+++

@snap[north-west]
モデルの作成
@snapend

#### ファイルの生成

```
php artisan make:model Models/Thread
```

@size[0.7em](`app/Models/Thread.php` のファイルが生成されます)

+++

@snap[north-west]
モデルの作成
@snapend

#### モデルの実装

```
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Thread extends Model
{
    protected $fillable = [
        'subject',
    ];
}
```
@[9-11](fillableを定義します)


---

### コントローラーの作成

+++

@snap[north-west]
コントローラーの作成
@snapend

#### ファイルの生成

```
php artisan make:controller -r ThreadController
```

@size[0.7em](`app/Http/Controllers/ThreadController.php` のファイルが生成されます)

+++

@snap[north-west]
コントローラーの作成
@snapend

#### コントローラーの実装

---?code=---?code=ThreadController.php&lang=php
@[9-11](fillableを定義します)

