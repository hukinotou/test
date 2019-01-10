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
- ルーティング
- リレーション

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

### ルーティング

+++

@snap[north-west]
ルーティング
@snapend

#### ルーティングとは

モデル、コントローラー、ビューを用意しただけではlaravelは動きません  
URLとコントローラーのメソッドを紐づける必要があります

+++

@snap[north-west]
ルーティング
@snapend

### routes/web.php

``` php
Route::get('/', 'ThreadController@index');
Route::resource('threads', 'ThreadController');
```
@[1]('/'にアクセスされたときの設定をします。ここではスレッド一覧を表示します。)
@[2](`Route::resource`を宣言することで基本的なCRUDのルーティングが追加されます)

+++

@snap[north-west]
ルーティング
@snapend

#### laravelでの標準的なルーティング

| 処理 | method | URI | アクション |
|---|---|---|---|
| 一覧表示 | GET | /threads | index |
| 作成画面 | GET | /threads/create | create |
| 登録処理 | POST | /threads | store |
| 詳細表示 | GET | /threads/{thread} | show |
| 編集画面 | GET | /threads/{thread}/edit | edit |
| 更新処理 | PUT/PATCH | /threads/{thread} | update |
| 削除処理 | DELETE | /threads/{thread} | destroy |

+++

@snap[north-west]
ルーティング
@snapend

### PUT? PATCH? DELETE?

HTMLではGETもしくはPOSTのみしか利用できません  
laravelでは擬似的にmethodを変更する仕組みがあります

```
<form action="/foo/bar" method="POST">
    @method('PUT')
</form>
```
@[1](`PUT`で送信されたと認識します)

---

### モデル結合ルート

+++

@snap[north-west]
モデル結合ルート
@snapend

#### 暗黙の結合

```
Route::get('/threads/{thread}', 'ThreadController@show');
```

```
public function show(Thread $thread)
{
    return view('threads.show', ['thread' => $thread]);
}
```

@size[0.7em](`/threads/34`というURLにアクセスした場合、`threads`テーブルから`id`が34のレコードを取得し、`threadController`の`show`アクションへ渡されます)

---

### 動かしてみよう！

スレッドのCRUDができたので、動かしてみましょう

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

+++

@snap[north-west]
response系のファイル生成
@snapend

#### ルーティング

```
Route::post('threads/{thread}', 'ResponseController@store')->name('response.store');
Route::resource('response', 'ResponseController', ['only' => ['update', 'edit', 'destroy']]);
```
@[1](スレッドに紐づくレスポンスを表示します)
@[2](レスポンスのCRUDを用意しますが、`only`オプションで利用するアクションを制限します)

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
<td>{{ count($thread->responses) }}</td>
```
threadsテーブルにはresponseカラムはありません。  
`Thread`モデルで定義した`hasMany`定義によってレスポンスの情報が取得されます

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

+++

@snap[north-west]
リレーション
@snapend

### N+1問題解決

#### index.blade.php

```
{{ $thread->responses_count }}
```
`$thread->responses_count`でレスポンス数が取得できます

+++

@snap[north-west]
リレーション
@snapend

### 実行されるSQL

```
select * from threads
```

```
select
    threads.*,
    (
        select count(*) 
        from responses 
        where threads.id = responses.thread_id
    ) as responses_count
from threads
```
---

### バリデーション

+++

@snap[north-west]
バリデーション
@snapend

#### ファイルの生成

```
php artisan make:make:request -r StoreThread
```

@size[0.7em](`app/Http/Requests/StoreThread.php` のファイルが生成されます)

+++

@snap[north-west]
バリデーション
@snapend

#### StoreThread.php実装

```
public function authorize()
{
    return true;
}
```
@size[0.7em](更新対象のデータに対して更新権限があるかのチェックなどを行う関数です。今回は制限を設けないため`true`を返します)

+++

@snap[north-west]
バリデーション
@snapend

#### StoreThread.php実装

```
public function rules()
{
    return [
        'subject' => 'required|max:20',
    ];
}
```
@size[0.7em](`subject`に対して必須`required`と最大長`max`を設定します)

+++

@snap[north-west]
バリデーション
@snapend

#### ThreadController.php実装

```
public function store(StoreThread $request)
{
    $thread = new Thread;
    $thread->fill($request->all());
    $thread->save();

    return redirect('threads');
}
```
@[1](`Request`の代わりに、`StoreThread`を利用することで自動的にバリデーションが実行されます)

+++

@snap[north-west]
バリデーション
@snapend

#### ビュー実装

```
<input type="txt" name="subject" value="{{ old('subject', $thread->subject) }}">
@if ($errors->has('subject'))
    {{ $errors->first('subject') }}
@endif
```
@[1](`old`関数を利用することでバリデーションエラー時に入力値の復元がされます。第二引数には編集画面で利用する初期値を設定できます)
@[2-4](エラーの有無を判定し、エラーメッセージを表示します)

+++

@snap[north-west]
バリデーション
@snapend

#### ビュー実装

```
class="@if ($errors->has('subject')) is-invalid @endif"
```
@size[0.7em](上記のようにすることでエラー時に`is-invalid`classを追加することもできます)
