> 「知識」がポイントになる内容を記載。

### Base言語活用のポイント
- マクロの世界とdataステップの世界を分けて考える
- シングルクォートとダブルクォートの違いをしっかり押さえる
- ステートメント、ステートメントに紐づくオプション、データセットに紐づくオプションをしっかり区別する
- dataステップが実際に動くときのなんとなくのイメージを掴む
  - 記述するのは列名とかだけであるが、実際の処理では1行ずつループされる
- 他のプログラミング言語とは異なり、「マクロを呼び出した箇所にマクロの定義内容を出力する」性質を持っている。そのため、カンマ区切りの内容だけを持つマクロを定義し、proc sqlのselect句の指定部分に用いるなどの芸当が可能。

### データセットの内容はいじらないが、dataステップ内で使用できる関数を使用したいとき
- 出力先データセット名に`_null_`を指定する。

``` sas
data _null_;
  put 'ログ出力のテスト';
run;
```

### 特殊文字を文字リテラルとして扱う

|内容|コード上の記述|
|-|-|
|タブ文字|`'09'x`|
|改行文字LF|`'0A'x`|
|改行文字CR|`'0D'x`|

### dataステップ内でマクロ変数に値を代入する
- `call symputx("マクロ変数名", 格納したい値)`で代入する。

### リスト文字列などを指定の区切り文字で区切り、n番目の要素を取り出す
- dataステップ内で`scan(文字列, n, 区切り文字(指定しない場合はあらゆる文字列で分割する))`を使用し、値を取り出す。

### ログにテキストや変数の内容を出力する
- dataステップ内では`put`、マクロ内では`%put`を使用する。
  - データセット内の変数を出力する場合は、シングルクォートやダブルクォートで括る必要はなく、変数名をそのまま指定する。
  - 複数の内容を連結して出力する場合、`put`は半角スペースで並べて記載、`%put`は`||`や`cats()`で明示的に結合して記載する。
  - `put`でデータセットの列を並べて記載した際、不要な半角スペースが付与されてしまう場合は、列名の直後に`+(-1)`を記載しておく。（詳細はputステートメントのリファレンスに記載されている）

``` sas
%let val = test;
%put &val.;
```

``` sas
data _null_;
    set sashelp.class;
    put Name +(-1) ', ' Age ;
run;
```


### データセットの列を重複を削除して取得
- `proc sort`および`nodupkey`を使用する

``` sas
/* carsデータセットのMake列の内容を、重複削除して取得する */
proc sort data=sashelp.cars(keep=Make) out=work.cars nodupkey;
    by logic_type;
run;
```

### 外部ファイルの存在を確認する
- dataステップ内で`fileexist()`を使用する。存在する場合は`1`、存在しない場合は`0`が返却される。
  - [FILEEXIST関数 - SAS® Help Center](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lefunctionsref/n06xm8hwk0t0axn10gj16lfiri43.htm)

### 文字列を置換する
- `tranwrd()`を使用する。
  -`tranwrd(変換したい文字列, 検索文字列, 置換する文字列)`
  - [TRANWRD関数 - SAS® Help Center](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lefunctionsref/p0pgemqcslm9uen1tvr5gcrusgrw.htm)

### dataステップ内のset句で読み込んだデータセットに、行番号を付与する
- `set データセット名 CUROBS=一時変数名`で新しい列に行番号を挿入できる。

### proc sqlでのサブクエリの使用
- 通常のSQLのように記述できる。

### 列名を変更してデータセットを読み込む
- `set work.table1(rename=(new_column_name=old_column_name));`のように`rename`を使用する。


### 指定の文字列をN回繰り返した文字列を用意する
- dataステップ内で`repeat(繰り返したい文字, N - 1);`を使用する。

### proc sql内で「欠損値かどうか」の条件を指定する
- `where`ステートメント内で`is missing演算子`を使用する



### set句で読み込む行の条件を指定する
- `whereデータセットオプション`を使用する。

``` sas
data work.cars;
    set sashelp.cars(where=(weight>6000));
run;
```
