### ホスト名を確認する

``` sh
uname -n
```

### ファイル名を変更しながら複製する

``` sh
for i in `ls /home/data/data_*.csv`; do cp -a $i `echo $i | sed “s/beforetxt/aftertxt/g”`; done
```

### ファイル名を一括変更する
``` sh
for i in `tree -if /home/data/data_*.csv`; do rename before_name after_name $i; done
```

### 直前のコマンドの返り値を取得して表示

``` sh
echo $?
```

### ファイルを指定行数で分割する

``` sh
split -l [分割する行数] [分割するファイル] [分割後ファイルのプレフィックス]
```

### 正規表現を無視してファイル内検索を行う

fgrepコマンドを使用する。

### 一括リネーム（サブディレクトリ内のファイルも対象）

``` sh
for i in `tree -if --charset=C /home/user/ | grep .csv`; do echo ----- $i -----; rename before_text after_text $i; done 
```
> -i ... 階層構造をなくす / -f ... フルパスで表示する / --charset=C ... 罫線の文字化けを回避する

### 改行文字の置換（CRLF → LF）

``` sh
sed -i 's/¥r//g' [置換したいファイル]
```

### 改行文字の置換（LF → CRLF）

``` sh
sed -i 's/$/¥r/g' [置換したいファイル]
```

### 文字コードの置換（SHIFT-JIS → UTF-8）

``` sh
iconv -f cp932 -t utf-8 data_sjis.csv -o data.csv
```

### 文字コードの置換（UTF-8　→　SHIFT-JIS）

``` sh
iconv -f utf-8 -t cp932 data.csv -o data_sjis.csv
```

### 条件に一致するディレクトリ内の複数ファイルについて、ファイル内容を取得し、区切り線を入れながら新しいファイルとして出力する

``` sh
for dir in `find NUMBER* -maxdepth 0 -type d` do echo > ./$dir/new_file_name_$dir.log; for i in `tree -if /log_directory/$dir/ | grep .log`; do echo -------- $i -------- >> ./$dir/new_file_name_$dir.log; cat $i | grep -e WARNING: -e ERROR: >> new_file_name_$dir.log; echo >> new_file_name_$dir.log; done; done;

```
