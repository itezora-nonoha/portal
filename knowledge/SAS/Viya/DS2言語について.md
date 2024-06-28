## その他リンク

|概要|リンク|
|-|-|
|Pythonコードファイルについて|[Pythonコードファイル - SAS Intelligent Decisioning](https://documentation.sas.com/doc/ja/edmcdc/v_047/edmug/n04vfc1flrz8jsn1o5jblnbgx6i3.htm)|
|DS2言語におけるnullおよび欠損値の処理について|[DS2によるnullおよびSAS欠損値の処理方法 - SAS Viya Platform Programming](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2pg/n19ec5j2guhvrrn1w53nlti9aten.htm)|
|datagridパッケージの概要|[Using the Datagrid Package - SAS Viya Platform Programming](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2pg/n1vcyhfhq2l0p4n1x3ggi7i6g0aa.htm#p03w47hvyb6753n1tefly13hjazf)|
|httpパッケージの概要|[Using the HTTP Package - SAS Viya Platform Programming](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2pg/n1vcyhfhq2l0p4n1x3ggi7i6g0aa.htm#n0tay4njptq0ywn1lh43a6fmo7ir)|
|jsonパッケージの概要|[Using the JSON Package - SAS Viya Platform Programming](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2pg/n1vcyhfhq2l0p4n1x3ggi7i6g0aa.htm#n01ivvcmauuzh8n1e61zov8aeta3)|

## よく使うDS2コードの関数
> 💡 関数名について、ガイドでは英大文字となっているが英小文字で記述しても問題なく動作する。

### 文字列操作

|関数|関数の概要|返り値|引数|
|-|-|-|-|
|[CAT関数](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2ref/n0somj6dsx2dmkn1vqjabhvt6nv4.htm)|文字列を結合|string|`CAT(文字列1, 文字列2, ...)`|
|[FIND関数](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2ref/n1v0tzf4xq363mn15mnppqtk2zti.htm)|部分文字列を検索|double|`FIND(文字列, 検索文字列, (省略可)開始位置)`|
|[TRANSTRN関数](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2ref/p04zd4tk5ar3p1n1qnuy7xx8hjvu.htm)|文字列を置換|char|`TRANSTRN(元の文字列, 検索文字列, 置換先文字列)`|
|[SUBSTRN関数](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2ref/p1j2tg0wxxvfhcn1ood3eijiwv6r.htm)|文字列を切り出す|char|`SUBSTRN(文字列, 開始位置, 長さ)`

### 数値計算

|関数|関数の概要|返り値|引数|
|-|-|-|-|
|[SUM関数](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2ref/p1gx49uqv8fv68n1vtjx2erav0tz.htm)|合計を取る|(引数に依る)|`SUM(合計対象1, 合計対象2, ...)`|
|[LOG関数](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2ref/n0hamnrj2doddtn13dd6oc92fg0u.htm)|自然対数を計算|double|`log(対数を取りたい正の数値)`|

### 値の判定

|関数|関数の概要|返り値|引数|
|-|-|-|-|
|[MISSING関数](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2ref/p0lok2xwfrbw8ln1n9bnbci2euuq.htm)|欠損値だったら1、そうでなければ0を返す|integer|`missing(チェックしたい値)`|
|[NULL関数](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2ref/p029q47a9bpklkn1esldr24qfw9l.htm)|null値だったら1、そうでなければ0を返す|integer|`null(チェックしたい値)`|

### 正規表現

|関数|関数の概要|返り値|引数|
|-|-|-|-|
|[PRXPARSE関数](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2ref/p11e5tc3fns4fvn1w3u6cp3urlq5.htm)|正規表現パターンをコンパイルする|integer|`prxparse('正規表現パターン')`|
|[PRXMATCH関数](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2ref/p0m49np18q0pdxn1laab98pazajo.htm)|正規表現で位置を検索|integer|`prxmatch(コンパイル済みの正規表現パターン, 検索対象文字列)`|
|[PRXCHANGE関数](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2ref/p1jpfpfp9qw3ron1501s0x5lpjw9.htm)|正規表現で文字列を置換する|char|`prxchange(コンパイル済みの正規表現パターン, 置換回数(-1を指定すると末尾まで置換), 置換文字列)`|


