
### 文字列「yyyymmdd_hhmmss」を得る

``` sas
%local _timestamp;
data _null_;
    call symputx("_timestamp", put(today(), yymmddn8.) || '_' || compress(put(datetime(), tod8.), ':'));
run;
```

### ソースステートメント（実行しているSASコードの内容）をSASログに表示しないようにする

``` sas
option nosource;
```

### NOTEログを表示しないようにする
- エラーメッセージや警告メッセージは変わらず表示される。

``` sas
option nonotes;
```

### 特殊文字を文字リテラルとして扱う

|内容|コード上の記述|
|-|-|
|タブ文字|`'09`x`|
|改行文字LF|`'0A'x`|
|改行文字CR|`'0D`x`|

### dataステップ内でマクロ変数に値を代入する
- `call symputx("マクロ変数名", 格納したい値)`で代入する。

### SASデータセットの各行の値を(任意の区切り文字)で結合し、文字列としてマクロ変数に格納する
- `retain`や`end = eof`を活用する。

``` sas
data _null_;
    set work.input_table end = eof;
    attrib
        input_file_list length = $1000.
        output_file_list length = $1000.
    ;
    retain input_file_list output_file_list;

    input_file_list = catx(' ', input_file_list, cats('input_', category, '.xlsx'));
    output_file_list = catx(' ', output_file_list, cats('output_', category, '.csv'));

    /* 最後まで行ったらマクロ変数に出力 */
    if (eof) then do;
        call symputx("input_file_list", input_file_list, 'G');
        call symputx("output_file_list", output_file_list, 'G');
    end;
run;
```

- `end=一時変数名` 最後の行を読み込んだ時、一時変数に「1」が入る
- `_N_` データステップの反復回数（処理中の行番号）が格納される
- `cats(値1, 値2, ...)` 引数値の両端の半角スペースを除いて結合るす

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
    set sashelp.class
    put Name +(-1) ', ' Age ;
run;
```

### 外部ファイルにテキストを出力する
- `filename`および`file`ステートメントを使用する。
  - `filename` ... 「SASファイル参照名」と「外部ファイルまたは出力デバイス」の関連付け・関連付けの解除を行う。
  - `file` ... putステートメントの出力先ファイルを指定する。
 
```sas
data _null_;
    filename OUTFILE "file_path";
    file OUTFILE encoding="utf-8"; /* ファイル内容を空っぽにする */
run;

data _null_;
    set sashelp.class;
    file OUTFILE MOD; /* ファイルに追記する */
    put Name;
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

### ソート済のデータセット列について、その値をもつ「最初の行」や「最後の行」だけで処理をしたい時
- `first.column_name`や`last.column_name`を使用し、`if`文で分岐させる。
  - 使用したいデータセット列は、`proc sort`などであらかじめソートしておく必要がある。

``` sas
proc sort data=sashelp.cars out=work.cars;
    by Type;
run;

data _null_;
    set work.cars

run;
```

### 外部ファイルの存在を確認する
- dataステップ内で`fileexist()`を使用する。存在する場合は`1`、存在しない場合は`0`が返却される。


### 文字列を置換する
- `tranwrd()`を使用する。
  -`tranwrd(変換したい文字列, 検索文字列, 置換する文字列)`

### dataステップ内のset句で読み込んだデータセットに、行番号を付与する
- `set データセット名 CUROBS=一時変数名`で新しい列に行番号を挿入できる。

### proc sqlでのサブクエリの使用
- 通常のSQLのように記述できる。

### 列名を変更してデータセットを読み込む
- `set work.table1(rename_(new_column_name=old_column_name));`のように`rename`を使用する。

### 文字列の長さに合わせてブロックコメントを生成する

- `length()`および`klength()`を組み合わせ、マルチバイト文字を含めたいい感じの文字長を計算し、ブロックコメントを生成する。

``` sas
data _null_;
    comment_text = strip(&comment_contents.);
    comment_len = (length(comment_text) - klength(comment_text)) / 2 + klength(comment_text);
    comment_line = '/*-' || repeat('-', comment_len - 1) || '-*/';
    put comment_line;
    put '/* ' "&comment_contents." ' */';
    put comment_line;
run;
```

### 指定の文字列をN回繰り返した文字列を用意する
- dataステップ内で`repeat(繰り返したい文字, N - 1);`を使用する。

### Excelファイルを読み込み、データセット化する
- `proc import`を使用する。

``` sas
%macro ImportExcel(i_file_path=, i_sheet_name=, ods_output_name=);
    proc import datafile=&i_file_path;
        replace
        dbms = xlsx
        out &ods_output_name.;
        getnames = yes;
        sheet = &i_sheet_name.;
    run;
%mend ImportExcel;
```

### set句で読み込む行の条件を指定する
- `whereデータセットオプション`を使用する。

``` sas
data work.cars;
    set sashelp.cars(where=(weight>6000));
run;
```

### proc sql内で「欠損値かどうか」の条件を指定する
- `where`ステートメント内で`is missing演算子`を使用する

### proc sqlで取得したクエリ結果を、区切り文字で結合しマクロ変数に代入する
- `into: マクロ変数名 separated by '区切り文字'`を用いる。（複数列から出力する場合でもintoは1つだけ．マクロ変数名にコロンを前置する）

``` sas
proc sql noprint;
    select Name, Age
        into :name_list separated by ' ', age_list separated by ' '
        from sashelp.class
        where age > 12;
quit;
```

### リスト文字列などを指定の区切り文字で区切り、n番目の要素を取り出す
- `scan(文字列, n, 区切り文字(指定しない場合はあらゆる文字列で分割する))`で取り出す。

### リスト文字列の要素でループする
- `%do %while`などを組み合わせる。

``` sas
%macro LoopTest();
    %let name_list = AAA BBB CCC DDD;
    %let index = 1;
    %let name = %scan(&name_list., &index., ' ');

    %do %while(not %sysevalf(%superq(name) =, boolean));
        %put &name.;

        %let index = %eval(&index. + 1);
        %let name = %scan(&name_list., &index., ' ');
    %end;
%mend LoopTest;
```
