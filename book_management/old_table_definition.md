### テーブル構造

## 書籍テーブル（books）

### 概要
googleAPIなどを用いて取得してきた書籍情報を記録・管理する。
（何度もAPIをコールするのを避けるため、永続キャッシュのような形で保持する）

### テーブル構造

|カラム論理名|カラム物理名|データ型|備考|
|-|-|-|-|
|書籍ID|book_id|varchar(10)|`B-`および数値8桁|
|書籍名|book_name|varchar(200)|-|
|ISBN|isbn|varchar(13)|先頭の固定値`978`を含む|
|2段目バーコード|barcode|varchar(13)|-|
|出版社ID|publisher_id|varchar(10)|-|
|書籍サムネイル|image_base64|-|base64で画像を管理（未対応）|


## 所持書籍テーブル（registered_books）

### 概要
各ユーザの所持情報を管理する。

|カラム論理名|カラム物理名|データ型|備考|
|-|-|-|-|
|書籍ID|book_id|varchar(10)|`B-`および数値8桁（booksテーブルを参照）|
|シリーズ名|series_name|varchar(100)|-|
|シリーズ番号|series_number|varchar(20)|-|
|本の状態|status_read|varchar(10)|-|
|入手状態|status_get|varchar(10)|-|
|本の貸し出し|is_lending|boolean|-|
|本の借り受け|is_borrowing|boolean|-|
|紙の書籍か？|is_paper|boolean|-|
|購入日|purchase_date|date|-|
|最終更新日|last_update|timestamp(6) without time zone|-|
|ユーザID|user_id|varchar(100)|-|

### 書籍登録用一時テーブル

``` sql
CREATE TABLE books_register_temporary(
    isbn varchar(13),
    barcode varchar(13),
    book_name varchar(200),
    image_base64 varchar,
    user_id varchar(100)
);
```

