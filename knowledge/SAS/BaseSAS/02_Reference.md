- dataステップ
  - attribステートメント https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lestmtsref/n1wxb7p9jkxycin16lz2db7idbnt.htm
  - dropステートメント https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lestmtsref/n1capr0s7tilbvn1lypdshkgpaip.htm
  - setステートメント https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lestmtsref/p00hxg3x8lwivcn1f0e9axziw57y.htm
    - curobsオプション
    - endオプション
- データセット
  - keepデータセットオプション https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/ledsoptsref/p0vw9lyyxk1cxkn0zzfemrsr3t9a.htm

> 💡 SASデータセットの行と列の表現について
本来の表記は「オブザベーション」および「変数」ですが、本ドキュメントにおいては「行」と「列」で表記しています。


### データセットオブション
- dataステップのsetステートメントなどで、データセット名の直後に`データセット名(～～～)`のように記述し、利用する
  - 複数のオプションを指定する場合はスペースで区切る
  - [カテゴリ別のデータセットオプション - SAS® Help Center](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/ledsoptsref/n03k13diwk36x1n1az3mpixdfbfp.htm)

|オプション|指定する内容|説明|
|-|-|-|
|keep=|保持したい列名|保持したい列名をスペース区切りで記載<br>ここに記載されていない列はそもそも読み込まれないため、dataステップなどで使用不可となる|
|drop=|除去したい列名|除去したい列名をスペース区切りで記載<br>ここに記載した列はそもそも読み込まれないため、dataステップなどで使用不可となる|
|where=|行の取得条件|条件部分は`()`で括り、`where=(Height>60 and Weight>100)`のように記述する必要がある<br>(`keep=`や`drop=`の後に`where=`が処理される）|
|rename=|変更したい列名と新しい列名|`rename=(name1=new_name1 name2=new_name2)`のようにセットで指定<br>(`keep=`や`drop=`の後に`rename=`が処理される）|

#### データセットオプションのサンプル

``` sas
data work.test;
	set sashelp.class(keep=Name Height Weight where=(Height>60 and Weight>100));
run;
```




### データセットのソート(proc sort)

- [proc sortステートメント - SAS® Help Center](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/proc/p02bhn81rn4u64n1b6l00ftdnxge.htm)

#### サンプル

``` sas
proc sort data=sashelp.cars out=work.cars_sorted;
	by Model;
run;
```

#### オブション
`proc sort`から`;`までの間にスペース区切りで記述する。

|オプション|指定する内容|説明|
|-|-|-|
|data=|データセット名|ソート対象のデータセットを指定|
|out=|データセット名|ソートした内容を格納するデータセットを指定（指定しない場合は上書きされる）|
|nodupkey|-|重複の削除を行う場合は指定<br>（キー変数について、重複するデータがあった場合は重複している2件目以降の行を削除する）|

#### プロシジャ内ステートメント
`proc sort ～;`から`run;`までの行に記述する。

|ステートメント|指定する内容|説明|
|-|-|-|
|by|列名|ソートのキーとする列を指定<br>（列名の直前に`descending`を付与することで、降順でソートできる）|


### データセットの転置(proc transpose)

- [proc transposeステートメント - SAS® Help Center](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/proc/p1r2tjnp8ewe3sn1acnpnrs3xbad.htm)

#### サンプル

``` sas
proc sort data=sashelp.cars out=work.cars_sorted;
	by Model;
run;

proc transpose data=work.cars_sorted out=work.cars;
	by Model;
run;
```

> 💡 単純に転置する場合は、byステートメントなどの記述は不要。

#### オブション
`proc transpose`から`;`までの間にスペース区切りで記述する。

|オプション|指定する内容|説明|
|-|-|-|
|data=|データセット名|転置対象のデータセットを指定|
|out=|データセット名|転置後のデータセットを指定|
|prefix=|文字列|転置後の列名に付与したい接頭辞を指定|
|suffix=|文字列|転置後の列名に付与したい接尾辞を指定|

#### プロシジャ内ステートメント
`proc transpose ～;`から`run;`までの行に記述する。

|ステートメント|指定する内容|説明|
|-|-|-|
|by|列名|転置せずにそのまま残す列を指定<br>（byを使用する場合は、指定する列で事前にソートしておく必要がある）|
|var|列名|転置する列を指定|
|id|列名|列の内容を「転置後の列名」として使用する列を指定|
|idlabel|列名|転置後の変数ラベルとして使用する変数を指定|



### 外部ファイルの読み込み(proc import)

- [proc importステートメント - SAS® Help Center](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/proc/n18jyszn33umngn14czw2qfw7thc.htm)

#### サンプル

``` sas
proc import datafile='C:\SAS_Study\test.csv' out=work.test dbms=csv replace;
    getnames = yes;
    datarow = 2;
run;
```

``` sas
filename input_data = 'C:\SAS_Study\input_data.txt'
proc import datafile=input_data out=work.test dbms=tab replace;
    getnames = yes;
run;
```

#### オブション
`proc import`から`;`までの間にスペース区切りで記述する。

|オプション|指定する内容|説明|
|-|-|-|
|datafile=|ファイルパス<br> or ファイル参照名|「入力ファイルのフルパス」または「filenameステートメントで定義したファイル参照名」を指定|
|out=|データセット名|転置後のデータセットを指定|
|dbms=|データソース名|外部ファイルのデータの種類を指定<br>xlsx / csv / tab / dlm(delimiterステートメントと併用)|
|replace|-|出力データセットを上書きする場合は指定|

#### プロシジャ内ステートメント
`proc import ～;`から`run;`までの行に記述する。

|ステートメント|指定する内容|説明|
|-|-|-|
|getnames=|yes / no|列名情報をファイルから取得するかどうかを指定|
|sheet=|Excelシート名|読み込むシート名を指定（dbms=xlsxの場合のみ有効）|

### 外部ファイルの読み込み(dataステップ, infileステートメント)

- [infileステートメント](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lestmtsref/n1rill4udj0tfun1fvce3j401plo.htm)

``` sas
filename test_data "C:\SAS_Study\test2.csv";
data work.test;
    infile test_data dim="," firstobs=2;
    input id:$8. name:$32. class:8.;
run;
```



### データセットを外部ファイルへ出力(proc export)

- [proc exportステートメント - SAS® Help Center](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/proc/n0ku4pxzx3d2len10ozjgyjbrpl9.htm)

#### サンプル

``` sas
proc export data=sashelp.class outfile='C:\SAS_Study\class.csv' dbms=csv replace;
run;
```

``` sas
proc export data=sashelp.class outfile='C:\SAS_Study\class.xlsx' dbms=xlsx replace;
    sheet = class;
run;
```

#### オブション
`proc export`から`;`までの間にスペース区切りで記述する。

|オプション|指定する内容|説明|
|-|-|-|
|data=|データセット名|出力対象のデータセットを指定|
|outfile=|ファイルパス<br> or ファイル参照名|「出力先ファイルのフルパス」または「filenameステートメントで定義したファイル参照名」を指定|
|dbms=|データソース名|外部ファイルのデータの種類を指定<br>xlsx / csv / tab / dlm(delimiterステートメントと併用)|
|replace|-|出力データセットを上書きする場合は指定|

#### プロシジャ内ステートメント
`proc export ～;`から`run;`までの行に記述する。

|ステートメント|指定する内容|説明|
|-|-|-|
|putnames=|yes / no|列名情報をファイルに書き出すかどうかを指定（デフォルトはyes）|
|sheet=|Excelシート名|読み込むシート名を指定（dbms=xlsxの場合のみ有効）|


### データセットやレポートをHTMLファイルへ出力(ods htmlステートメント)
- [ods htmlステートメント](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/grmapref/n06apepzft3fsen1x9ku6sr8ytfi.htm)

``` sas
ods html file='C:\SAS_Study\class.html';
proc means data=sashelp.class;
run;
ods html close;
```


### 要約統計量の計算(proc univariate)

- 各種統計量を算出し、レポートを出力する
  - [univariateプロシジャ - SAS® Help Center](https://documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/procstat/procstat_univariate_toc.htm)

``` sas
proc univariate data=sashelp.class normal plot;
    var height;
    output out=work.result n=n mean=mean std=std median=median;
run;
```

### dataステップ内で利用できる関数

[最も一般的に使用される関数 - SAS® Help Center](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lefunctionsref/n1b7dkf9vhuqczn16qfd41j6dd54.htm)

#### 文字列の結合

|関数|内容|
|-|-|
|`cat(X, Y, ...)`|文字列Xと文字列Yを結合する|
|`cats(X, Y, ...)`|文字列Xと文字列Yの先頭・末尾のスペースを取り除き、結合する|
|`catx(', ', X, Y, ...)`|文字列Xと文字列Yの先頭・末尾のスペースを取り除き、', 'で結合する|

#### 文字列の置換
|関数|内容|
|-|-|
|`tranwrd(X, 'ABC', 'XYZ')`|文字列Xの'ABC'を'XYZ'に置換する|
|`compress(X)`|文字列Xからスペースを取り除く|
|`compress(X, '\')`|文字列Xから'\'を取り除く|
|`trim(X)`|文字列Xの末尾のスペースを取り除く|

#### 文字列に関する情報の取得
|関数|内容|
|-|-|
|`count(X, 'A')`|列Xに存在する'A'の個数をカウントする|
|`length(X)`|列Xの長さを1バイト単位で取得する|
|`klength(X)`|列Xの長さを文字数単位で取得する（1バイト文字も2バイト文字も長さ1としてカウント）|
|`index(X, 'ABC')`|文字列Xの最初の'ABC'の開始位置を返す|

#### 部分文字列の抽出
|関数|内容|
|-|-|
|`substr(X, 5, 8)`|文字列Xの5～8文字目を取得する|

#### 日時データの利用

- [YYMMDDx出力形式](https://documentation.sas.com/doc/ja/vdmmlcdc/8.1/leforinforref/p0iptsg6780kzfn1k5f0b8s7k7dq.htm)

``` sas
data work.date_info;
	now_datetime = datetime(); 
	now_date = datepart(now_datetime);
	now_time = timepart(now_datetime);
	
	yyyymmddn = put(now_date, yymmddn8.); /* yyyymmdd(n:区切り文字なし) */
	yyyymmddb = put(now_date, yymmddb8.); /* yyyymmdd(b:スペース区切り) */
	yyyymmddc = put(now_date, yymmddc8.); /* yyyymmdd(c:コロン区切り) */
	yyyymmddd = put(now_date, yymmddd8.); /* yyyymmdd(d:コロン区切り) */
	yyyymmddp = put(now_date, yymmddp8.); /* yyyymmdd(p:ピリオド区切り) */
	yyyymmdds = put(now_date, yymmdds8.); /* yyyymmdd(s:スラッシュ区切り) */
	yyyymdd10 = put(now_date, yymmdd10.); /* yyyy-mm-dd */
	ddmmmyyyy = put(now_date, date10.); /* ddmmmyyyy */
	hhmmss = put(now_time, time.); /* hh:mm:ss */
	
	year = year(now_date);
	month = month(now_date);
	day = day(now_date);
	
	hour = hour(now_time);
	min = minute(now_time);
	second = second(now_time);
	
	format until_time time8.;
	until_day = input('20250101', yymmdd8.) - now_date;
	until_sec = input('24:00:00', hhmmss8.) - now_time;
run;
```

#### 関数の使用例

``` sas
data work.test;
	set sashelp.class;
	
	height_cm = round(height * 0.0254, 0.01); /* inchをmに変換 */
	height_str = put(height_cm, best8.); /* 数値を文字列に変換 */
	height_str = tranwrd(height_str, '.', 'm'); /* 文字列を置換 */
	height_str = compress(height_str); /* 空白を削除 */
	name_height = cats(Name, '_', height_str, 'cm'); /* 文字列を結合 */
run;
```

### first変数とlast変数
- dataステップ内でbyステートメントを適用すると、byステートメントで指定した列ごとに「first.列名」および「last.列名」の列が生成される。
  - [SASによるBYグループの処理方法](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lestmtsref/p0yeyftk8ftuckn1o5qzy53284gz.htm#n1gd3sjlellqfxn1q67on4bzgsfs)
- それぞれ、下記の値が格納されている。 
  - first.列名 ... byステートメントでグループ化された各行のうち、最初の行にのみ'1'が格納されており、それ以外は'0'
  - last.列名 ... byステートメントでグループ化された各行のうち、最後の行にのみ'1'が格納されており、それ以外は'0'
- この変数を使用すると、「最初だけ処理、最後だけ処理」といった処理を行わせることができる。

```

``` sas
proc sort data=sashelp.fish out=work.fish_sorted;
	by species;
run;

data work.fish;
	set work.fish_sorted(where=(not missing(Weight))); /* 欠損値の行を削除 */
	by species; /* species列でグループ化 */

	/* 計算用の列を初期化 */
	if first.species then do;
		count = 0;
		weight_sum = 0;
	end;
	
	/* グループ内の行について、件数と合計を計算 */
	retain count weight_sum;
	count = count + 1;
	weight_sum = weight_sum + Weight;

	/* 平均値を計算し、行を出力 */
	if last.species then do;
		weight_avg = round(weight_sum / count, 0.1);
		output;
	end;
	
	/* 出力したい列を指定 */
	keep species count weight_sum weight_avg; 
run;
```

### libnameステートメント(グローバルステートメント)
- SASデータセットが格納されるSASライブラリを定義する
  - [libnameステートメント](https://documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lestmtsglobal/n1nk65k2vsfmxfn1wu17fntzszbp.htm)
- データセット以外にも「SASエンジン」を指定することで、他のタイプのファイルへのアクセスを可能にすることもできる

### filenameステートメント(グローバルステートメント)
- 外部ファイル（.csvや.xlsxなど）がある場所に「ファイル参照名」を付ける（パス情報を特別な変数に入れる）
  - [filenameステートメント](https://documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lestmtsglobal/p05r9vhhqbhfzun1qo9mw64s4700.htm)

``` sas
filename test_data = 'C:\SAS_Study\test.csv';
proc import datafile=test_data out=work.test dbms=csv replace;
    getnames = yes;
    datarow = 2;
run;
```

### xステートメント(グローバルステートメント)

- OSコマンドを実行する
  - [xステートメント](https://go.documentation.sas.com/doc/ja/pgmsascdc/9.4_3.5/lestmtsglobal/p11ba12uypvfazn1jk7acffuzlbl.htm)
