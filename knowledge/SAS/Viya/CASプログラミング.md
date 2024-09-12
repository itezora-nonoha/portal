

## 全てのCASライブラリをローカルに持ってくる

``` sas
caslib _all_ assign;
```

## CASセッションを確立する

``` sas
cas casauto;
```

## tableアクションセット

### 概要
- `proc cas`内で使用可能。
- 各アクションセットにおいて、下記のようにパラメータを記述する。
  - アクション名(tableExists)などの後ろ 〜 スラッシュの前 ... アクションセットからの応答値を受け取る引数名(result=およびstatus=)
  - スラッシュの後ろ 〜 セミコロンまで ... アクションセットに入力するパラメータ(name=やcaslib=など)

### テーブルの操作

#### table.Exists
- テーブルが存在するかどうかをチェックする

``` sas
cas casauto;
proc cas;
  table.tableExists result = rs /
    name = "table_name" /* 必須パラメータ */
    caslib = "caslib_name"
  ;
quit;
```

 