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

+++?code=ThreadController.php&lang=php&title=ThreadController.php
@[16-20](一覧取得処理 取得したスレッド一覧`$threads`をviewへ渡します)
@[27-30](作成画面の表示)
@[38-45](新規登録処理 fillではモデルのfillableで指定したデータのみ取り込まれます)
@[53-56](スレッド表示)
@[64-67](スレッド編集画面)
@[76-82](スレッド更新処理)
@[90-95](スレッド削除処理)

---

### ビューの作成

+++

@snap[north-west]
ビューの作成
@snapend

#### ファイルの生成

`resources/views/threads`へビューファイルを作成します。  
コマンドはないため、手動で作成します。

[Bladeテンプレート](https://readouble.com/laravel/5.7/ja/blade.html)を利用します。  
`{{ $thread->subject }}`のように波カッコで囲うとエスケープされて出力されます。  
そのほか、`@if`、`@for`、`@foreach`などが用意されています。

---

### response系のファイル生成

+++

@snap[north-west]
response系のファイル生成
@snapend

#### ファイル生成

```
php artisan make:model -c -r -m Response
```

上記のコマンドで下記のファイルが生成されます

- `database/migrations/2018_12_09_132732_create_responses_table.php`
- `app/Models/Response.php`
- `app/Http/Controllers/ResponseController.php`

+++

@snap[north-west]
response系のファイル生成
@snapend

#### ファイル実装

時間がないため、事前に用意していたファイルをコピーします  
ビューファイルもコピーします

---

### リレーション

+++

@snap[north-west]
リレーション
@snapend

#### 関係性

`threads`テーブルと`responses`テーブルは1対多の関係です  
モデルに定義をすることで自動的に関連するデータを取得することができます

+++

@snap[north-west]
リレーション
@snapend

#### Thread.php実装

```
class Thread extends Model
{
    protected $fillable = [
        'subject',
    ];

    public function responses()
    {
        return $this->hasMany('App\Models\Response');
    }
}
```
@[7-10](スレッドから見てレスポンスは1対多の関係なので、`hasMany`を定義します)

+++

@snap[north-west]
リレーション
@snapend

#### Response.php実装

```
class Response extends Model
{
    protected $fillable = [
        'response',
    ];

    public function thread()
    {
        return $this->belongsTo('App\Models\Thread');
    }
}
```
@[7-10](レスポンスから見てスレッドは多対1の関係なので、`belongsTo`を定義します)

+++

@snap[north-west]
リレーション
@snapend

### リレーションの利用方法

```
@forelse ($threads as $thread)
    <tr>
        <th scope="row">{{ $thread->id }}</th>
        <td><a href="{{ url('threads/' . $thread->id) }}">{{ $thread->subject }}</a></td>
        <td>{{ count($thread->responses) }}</td>
        <td>{{ $thread->created_at }}</td>
        <td>{{ $thread->updated_at }}</td>
```
@[5](threadsテーブルにはresponseカラムはありません。`Thread`モデルで定義した`hasMany`定義によってレスポンスの情報が取得されます)

+++

@snap[north-west]
リレーション
@snapend

### N+1問題

スレッドの件数分だけレスポンスを`count`するSQLが発行されてしまいます。

- スレッドの一覧情報を取得するSQL＠1本
- スレッドごとのレスポンス数を取得するSQL＠N本

+++

@snap[north-west]
リレーション
@snapend

### N+1問題解決

#### ThreadController.php

```
public function index()
{
    $threads = Thread::withCount('responses')->get();
    return view('threads.index', ['threads' => $threads]);
}
```
@[3](`withCount`を利用することで`responses_count`としてレスポンス数を取得できます)

#### index.blade.php

```
@forelse ($threads as $thread)
    <tr>
        <th scope="row">
            {{ $thread->id }}
        </th>
        <td>
            <a href="{{ url('threads/' . $thread->id) }}">{{ $thread->subject }}</a>
        </td>
        <td>
            {{ $thread->responses_count }}
        </td>
        <td>
            {{ $thread->created_at }}
        </td>
        <td>
            {{ $thread->updated_at }}
        </td>
```
@[5](`$thread->responses_count`でレスポンス数が取得できます)

+++

@snap[north-west]
リレーション
@snapend

### 実行されるSQL

```
select * from threads
```

```
select threads.*, (select count(*) from responses where threads.id = responses.thread_id) as responses_count from threads
```
