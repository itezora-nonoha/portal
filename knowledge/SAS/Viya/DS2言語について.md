## 基本的な記述ルール
- 行末にはセミコロンを記述する
- 関数名や演算子について、大文字小文字は区別されない
- 一重引用符で文字を括ると、文字定数となる
- 二重引用符で文字を括ると変数となる
（マルチバイト文字を変数名として利用したい時のみ、変数名を`""`で括る）

## 演算子

[式の演算子 - SAS Viya Platform Programming](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2pg/p0vinswrxk1819n1qizkibfin0cy.htm)

### 算術演算子

|演算子|機能|
|-|-|
|+|加算|
|-|減算|
|*|乗算|
|/|除算|
|**|累乗|
|><|左辺右辺の小さい方を取得|
|<>|左辺右辺の大きい方を取得|

### 比較演算子

|演算子|機能|
|-|-|
|LT, <|より小さい|
|GT, >|より大きい|
|LE, <=|以下|
|GE, >=|以上|
|EQ, =|等しい|
|NE, ^=|等しくない|

### 論理演算子

|演算子|機能|
|-|-|
|OR, \|, !|論理和|
|AND, &|論理積|
|NOT, ^, ~|否定|

### IN演算子
- リスト内に存在しているかどうかをIN演算子を使うことで判定できる

|入力値|式|判定値|
|-|-|-|
|a = 2| a in (1, 2, 3, 4)|True|
|b = 3| b not in (1, 2)|True|


## その他リンク

|概要|リンク|
|-|-|
|Pythonコードファイルについて|[Pythonコードファイル - SAS Intelligent Decisioning](https://documentation.sas.com/doc/ja/edmcdc/v_047/edmug/n04vfc1flrz8jsn1o5jblnbgx6i3.htm)|
|DS2言語におけるnullおよび欠損値の処理について|[DS2によるnullおよびSAS欠損値の処理方法 - SAS Viya Platform Programming](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2pg/n19ec5j2guhvrrn1w53nlti9aten.htm)|
|datagridパッケージの概要|[Using the Datagrid Package - SAS Viya Platform Programming](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2pg/n1vcyhfhq2l0p4n1x3ggi7i6g0aa.htm#p03w47hvyb6753n1tefly13hjazf)|
|httpパッケージの概要|[Using the HTTP Package - SAS Viya Platform Programming](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2pg/n1vcyhfhq2l0p4n1x3ggi7i6g0aa.htm#n0tay4njptq0ywn1lh43a6fmo7ir)|
|jsonパッケージの概要|[Using the JSON Package - SAS Viya Platform Programming](https://documentation.sas.com/doc/ja/pgmsascdc/v_052/ds2pg/n1vcyhfhq2l0p4n1x3ggi7i6g0aa.htm#n01ivvcmauuzh8n1e61zov8aeta3)|

