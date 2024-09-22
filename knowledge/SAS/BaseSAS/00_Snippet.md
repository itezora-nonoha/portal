> 「記述しているSnippet」の内容がポイントになる内容を記載。

### よく使うシステムオプション
- [カテゴリ別のシステムオプション - SAS® Help Center](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lesysoptsref/p0r3gu6wvyq0l0n1gkadgl7iukfm.htm)

#### ログを簡素化する

``` sas
option nosource nonotes;
```

- `nosource` ... ソースステートメント（実行しているSASコードの内容）をSASログに表示しないようにする
- `nonotes` ... NOTEログを表示しないようにする（エラーメッセージや警告メッセージは変わらず表示される）

#### ログを詳細化する

``` sas
option symbolgen mprint mlogic;
```

- `symbolgen` ... マクロ変数の置換結果を表示する
- `mprint` ... マクロ実行時に行われるSASステートメントをログに出力する
- `mlogic` ... マクロ変数の評価式やマクロの実行開始、終了をログに出力する

### 文字列「yyyymmdd_hhmmss」を得る

``` sas
%local _timestamp;
data _null_;
    call symputx("_timestamp", put(today(), yymmddn8.) || '_' || compress(put(datetime(), tod8.), ':'));
run;
```

### SASデータセットの各行の値を(任意の区切り文字)で結合し、文字列としてマクロ変数に格納する
- `retain`や`end = eof`を活用する。
  - [RETAINステートメント - SAS® Help Center](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lestmtsref/p0t2ac0tfzcgbjn112mu96hkgg9o.htm) 

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
- `cats(値1, 値2, ...)` 引数値の両端の半角スペースを除いて結合する

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


### 文字列の長さに合わせてブロックコメントを生成する

- `length()`および`klength()`を組み合わせ、マルチバイト文字を含めたいい感じの文字長を計算し、ブロックコメントを生成する。
  - 実行環境によってはうまくいかないこともある。

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

### proc sqlで取得したクエリ結果を、区切り文字で結合しマクロ変数に代入する
- `into: マクロ変数名 separated by '区切り文字'`を用いる。（複数列から出力する場合でもintoは1つだけ．マクロ変数名にコロンを前置する）
  - [INTO句 - SAS® Help Center](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/mcrolref/n1y2jszlvs4hugn14nooftfrxhp3.htm) 

``` sas
proc sql noprint;
    select Name, Age
        into :name_list separated by ' ', age_list separated by ' '
        from sashelp.class
        where age > 12;
quit;
```

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

### 外部CSVファイルをdataステップで読み込み、データセット化する

``` sas
filename sample "C:\SAS_Study\test2.csv";
data work.test;
    infile sample dim="," firstobs=2;
    input id:$8. name:$32. class:8.;
run;
```

### 外部CSVファイルをproc importプロシジャで読み込み、データセット化する

``` sas
proc import datafile='C:\SAS_Study\test.csv' out=work.work.test dbms=csv replace;
    getnames = yes;
    datarow = 2;
run;
```

``` sas
filename input_data = 'C:\SAS_Study\input_data.txt' 
proc import datafile=input_data out=work.test dbms= replace;
    getnames = yes;
run;
```


### Excelファイルを読み込み、データセット化する
- `proc import`を使用する。

``` sas
%macro ImportExcel(i_file_path=, i_sheet_name=, ods_output_name=);
    proc import datafile=&i_file_path out=&ods_output_name. dbms = xlsx replace;
        getnames = yes;
        sheet = &i_sheet_name.;
    run;
%mend ImportExcel;
```

