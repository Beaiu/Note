# Linux
## 1. 基本的なコマンド
### ファイル操作
* **`ls [option] [file]`**
  * option
    * -a : all
    * -l : long
    * -t : time
    * -r : Reverse 
    * -R : ディレクトリ内容を再帰的に表示する
    * -1 : リストを縦に並べる
    * -S : ファイルサイズ順でソートする
    * -X : ファイルを拡張子ごとにまとめる
    * --full-time : タイムスタンプの詳細を表示する
    * -m : ファイル名をカンマで区切って表示する
  * others
    * 絞り込み (wildcard character)
      *     ls host.*
            ls ???.conf
            ls hosts.????w
* **`cp [option] (copyfrom) (copyto)`**
  * option
    * -i : 処理を行うときに確認をする
    * -r : (recursive：再帰)ディレクトリをコピーする
    * -p : 元ファイルの情報を保存する
* **`mv (movefrom) (moveto)`**
  * option
    * -i
    * -f : 強制的に処理を実行する
* **`rm (file)`**
  * -i
  * -f
  * -r
  * -rfl : delete system

***

### ディレクトリの操作
* **`pwd`** (Print Working Directory)
* **`cd (directory)`**
  * e.g. `cd ~`
* **`mkdir (directory)`**
  * option
    * -p : 指定されたディレクトリの上位ディレクトリも作成する
  * e.g. `mkdir -p dir1/dir2/dir3`
* **`rmdir (directory)`**
  * option
    * -p : 指定した階層までのディレクトリを一括で削除する
* **Others**
  * Directory
    * **.** : 現在いるdirectory
    * **..** : 親directory
    * **~** : home directory
    * **/** : root directory
  * 絶対指定と相対指定
    * 絶対指定 : /usr/bin/xxx
    * 相対指定 : ../bin/xxx

***

### ファイルの内容を表示
* **`cat (file)`**
  * option
    * -n : 行番号を付加して表示する
* **`more (file)`**
  * command
    * `(space)` : next page
    * `b` : 前の一画面に戻る
    * `f` : 次の一画面に進む
    * `/(word)` : 単語を検索する。`n`で検索結果をジャンプする
    * `q` : quit
* **`less (file)`**
  * command
    * `(space)` : next page
    * `b` : 前の一画面に戻る
    * `f` : 次の一画面に進む
    * `↑` : 前の行に進む
    * `↓` : 次の行に進む
    * `/(word)` : 単語を検索する。`n`で検索結果をジャンプする
    * `q` : quit
 
***

### ファイルの検索
* **`find (path) -name (file)`**

***

### コマンドのパス
* **`which (command)`**
  PATH環境変数に含まれるディレクトリ配下に配置されているコマンドのパスを表示することができる
* **`(command) --help`**

***

### ファイルのタイムスタンプの変更 (touch) : 指定ファイルが存在しない場合、空のファイルを作成する。
* **`touch [option] (file)`**
  * option
    * -t : ファイルの更新時刻[MMDDhhmm]を指定するオプション

***

### ファイルの一部の取得 (head, tail)
* **`head [option] (file)`**
  * option
    * -n 行 : 先頭から指定した行を標準出力する (default: 10)
    * -c バイト : 先頭から指定したバイト分を標準出力する
  * e.g. `cat FILE | head -
* **`tail [option] (file)`**
  * option
    * -n 行
    * -c バイト
    * -f : 変更をリアルタイムでモニタすることが可能（例：ログファイルのモニタ）

***

### テキストファイルのソート (sort)
* **`sort [option] (file)`**
  * option
    * -r : 逆順
    * -k n : n列目のデータをソートする
    * -n : 数値としてソートする

***

### 行の重複の消去 (uniq)
* **`uniq (file)`** : 直前の行と同じ内容があった場合、対象行を出力する

***

### 文字列の置き換え (tr)
* **`tr (文字列1) (文字列2)`** : 標準入力からのデータを文字毎に置き換える
  * e.g. `cat FILE | tr abc ABC

***

### ファイルの比較 (diff)
* **`diff [option] (file1) (file2)`**
  * option
    * -c : context diff形式で差分を出力する
    * -u : unified diff形式で差分を出力する

***

### マニュアルの使い方
* **`man (command)`**
  * option
    * `-k` : '単語'が含まれるエントリ一覧を出力する
* **session**
  * |  項目  |  内容  |
    | ---- | ---- |
    |  1  |  ユーザコマンド  |
    |  2  |  システムコール  |
    |  3  |  システムライブラリや関数  |
    |  4  |  デバイスやデバイスドライバ  |
    |  5  |  ファイルの形式  |
    |  6  |  ゲームやデモなど  |
    |  7  |  その他  |
    |  8  |  システム管理系のコマンド  |
    |  9  |  カーネルなどの情報  |

***
***

## 2. 正規表現とパイプ
### リダイレクト & 標準エラー出力
* **`>`** : redirect
  * 出力のリダイレクト : `ls > ls-output` (すでにls-outputが存在しているときは、前のls-outputが削除されて新しく作成される。上書きせず追記したい場合は、`>>`を用いる)
  * catコマンドによるファイル作成 : `cat > cat-output` (自由な内容でファイルを作成することができる)
* 標準エラー出力
  * e.g. `ls -l tekitou 2> ls-l-output` (エラーは画面に現れず、指定したファイルに出力される)
  * e.g. `ls -l tekitou > ls-l-output-second 2>&1` (標準出力とエラー出力）

***

### パイプ（|）
コマンドとコマンドをパイプ（|）でつなげることでパイプの前のコマンドを後ろのコマンドの標準入力とすることができる 
  * e.g. `ls -l /usr/bin | less`

***

### grepコマンド
* **`grep [option] (検索条件) [file]`**
  ファイルの中からデータを検索する
  * option
    * -e : 文字列を検索パターンとして扱う
    * -i : 検索パターンと入力ファイルの双方で、英大文字と小文字の区別を行わない
    * -v : 検索パターンとマッチしなかった行を選択する
  * e.g. `grep abc /etc/*`
* **正規表現**
  * |  記号  |  意味  |  例  |
    | ---- | ---- |  ----  |
    |  ^  |  行頭を表す  |  ^a  |
    |  $  |  行末を表す  |  b$  |
    |  .  |  任意の一字を意味する  |  a.b  |
    |  *  |  直前文字の0回以上の繰り返しを意味する  |  -  |
    |  [...]  |  ...の中の任意の一字を意味する  |  [ab]ab  |
    |  [^...]  |  ...の文字が含まれないことを意味する  |  [^ab]ab  |
    |  \  |  正規表現の記号をエスケープする  |  -  |

***
***

## 3.vi editor
### 基本操作
* **`vi (file)`** : open or create file
* **`:q`** : close
* **`:w`** : save
* **`:wq`** : save and close
* **`:q!`** : close without save
* **`:i`** : insert
* **`A`** : 行末でinsert
* **`a`** : 次の文字でinsert
* **`h`** or **`←`** : カーソルを左へ移動
* **`l`** or **`→`** : カーソルを右へ移動
* **`j`** or **`↓`** : カーソルを下へ移動
* **`k`** or **`↑`** : カーソルを上へ移動
* **`0`** : カレント行の行頭へ移動
* **`$`** : カレント行の行末へ移動
* **`Ctrl + f`** : next page
* **`Ctrl + b`** : last page
* **`:(number)`** : 行番号を指定した移動
* **`gg`** : 1行目へ移動
* **`G`** : 最終行へ移動
* **`x`** : 1文字削除
* **`dd`** : 1行カット・削除  
* **`yy`** : 1行コピー
* **`(number)yy`** : n行コピー 
* **`p`** : カーソルの文字の次または次の行にペースト 
* **`P`** : カーソルの文字の前または前の行にペースト
* **`u`** : カット、ペーストを一回取り消し

***

### 置換と検索
* **`/(検索文字列)`** : 文字列の検索
* **`n`** : 下方向へ再検索
* **`N`** : 上方向へ再建策
* **`:(row number)s/(検索文字列)/(置換文字列)/[option]`** : 文字列を置換する
  * option
    * g : 全て置換。指定しないと行ごとに1回だけ置換する
    * c : 置換のたびに確認を求める
  * e.g. `:(number)s/old/new`
  * e.g. `:%s/old/new/g` (ファイル全体の検索結果を置換する)

***
***

## 4.管理者の仕事
### グループとユーザ
* **`useradd (user name)`**
  * option
    * -c : comment
    * -g : primary group (/etc/group)
    * -G : sub group
    * -d : home directory
    * -s : シェル (default: /bin/bash)
    * -u : user ID
  * e.g. `useradd -g users -u 1001 penguin`
  * e.g. `grep penguin /etc/passwd'
* **`usermod (user name)`** : ユーザアカウントの変更
  * option
    * -c
    * -g
    * -G
    * -l : user name
    * -u
  * e.g. `usermod -c "Comment change" penguin
* **`userdel (user name)`**
  * option
    * -r : delete home directory
* **`groupadd (group name)`**
  * option
    * -g : group ID number
* **`groupmod [-g gid] [-n new-group-name] (変更対象のグループ）`**
  * option
    * -n : 既存のグループ名を変更する場合に指定する
    * -g : 既存のグループIDを変更する（100未満のグループIDはシステムで使われているため、指定できません）
* **`groupdel (group name)`**
* **`su`**
* **`su - (user)`**
* **`sudo (command)`** : sudoの設定は/etc/sudoersファイルで管理する。visudoコマンドで編集できる
  * option
    * -u : user
  * `visudo`
  * `vigr`

***

### Password & Password file
* パスワードは/etc/shadowファイルに暗号化されて記録される(`account:password:last_changed:may_be_changed:must_be_changed:warned:expires:disabled:reserved`)
* グループの定義は/etc/groupファイルに記述されている(`group_name:password:GID:user_list`)
* ユーザの定義は/etc/passwd記述されている（`account:password:UID:GID:GECOS:directory:shell`）
* **`passwd (user name)`** (パスワードの設定）

***
***

## 5.ユーザ権限とアクセス権
### ファイルの所有者と所有グループ
* **`chown (user)[.(group)] (directory or file)`**
  * option
    * -R : directoryを対象にする
* **`chgrp (group) (directory or file)`**
  * option
    * -R : directoryを対象にする

***

### ファイルとアクセス権
* **drwx-rwx-rwx**
  * d : ファイル種別
  * rwx(1) : 所有ユーザー (u)
  * rwx(2) : 所有グループ (g)
  * rwx(3) : その他 (o)
  * r : read (8進数：4)
  * w : write (8進数：2)
  * x : 実行またはディレクトリの移動 (8進数：1)
* **`chmod (mode[, mode]...) (directory or file)`** or **`chmod (8進数表記のモード) (directory or file)`**
  * u, g, oの全てに同じ権限を指定する時はaを指定する
  * モードの指定に特殊な属性
    * setuid : setuidビット(s)が付いたプログラムを実行すると、ファイル所有者の権限で実行される
    * setgid : setgidビット(s)が付いたプログラムを実行すると、ファイル所有グループの権限で実行される
    * sticky : stickyビット(t)が付いたディレクトリ内のファイルは所有者以外が削除できなくなる（ファイルについてのstickyは無視される）
  * option 
    * -R
  * e.g. `chmod u+rw-x, go+r-wx chownfile`
  * e.g. `chmod 664 chownfile`
  * e.g. `chmod u+s,g+s,+t idbitfile` : -rwSr-Sr-T
* **`umask [8進数のモードのマスク値]`** : 指定したバーミッションでファイルを作成するように制限できる（umaskコマンドで指定するマスク値は、すべてのユーザーが読み書きできるパーミッション666から、設定したいパーミッションを引き算することでマスク値を求めることができる）

***
***

## 6.シェルスクリプト
シェルにより入力するコマンドは、シェルスクリプトにより順次実行可能な形式になります。それを応用することにより、コマンド入力による作業を自動化することが可能です。
* シェルスクリプトの作成
  ```shell
  #!/bin/bash     ※ファイルの1行目には、利用するシェルの種類とそのコマンド位置を記述する。2行目以降に実行するコマンドを1行ずつ入力する。

  echo Message test

  abc=123
  echo $abc       # > 123
  read abc        # aabbccを入力
  echo $abc       # > aabbcc
  export abc
  export xyz=234

  abc[0]=123
  abc[1]=456
  echo ${abc[0]}
  index=1
  echo ${abc[$index]}
  #-------------------
  set
  env
  unset abc
  ```
  * echo [Option] 文字列
    * -n：改行を抑制する 
    * 変数は、$を付けて出力することができる
  * 変数
    * bashでは、一次元の配列変数を使用することができる。要素は角括弧[]で囲む。配列変数の内容を表示する場合は、$の後ろに波括弧{}で配列変数を囲む
    * シェル変数
      * 実行しているシェルの内部でのみ有効
      * シェル変数の一覧を表示する場合は、setコマンドを利用する。また削除する場合は、unsetを利用する
    * 環境変数
      * exprot
      * そこから実行されたコマンド内でも有効。シェル変数から作成できる
      * 現在の環境変数の一覧を表示する場合、envコマンドを利用する。また登録済の環境変数を削除するときは、unsetコマンドを利用する
  * read 変数名
    * 標準入力からデータを読み込む。すでに変数にデータが入っていた場合、新しいデータに上書きされる
  * sourceコマンド
    * bash等のシェルの内部コマンドで、指定されたファイルを読み込んでシェル環境を設定します
  * 引用符
    * シングルクオート（''）：中の文字列は「文字列」として認識され、引用符内に変数があったとしても文字として出力される
    * ダブルクオート（""）：引用符内に変数があった場合、変数が展開されて出力される
    * バッククオート（``）：引用符内にコマンドがあるとそのコマンドを実行した結果が展開される
    ```shell
    #例
    var=date
    echo "$var"
    # ⇒ date
    echo `$var`
    # ⇒ (日付)
    echo '$var'
    # ⇒ $var
    ```
  * 引数
    * 実行時にオプションを引数として参照することができる。引数は$の後に引数の番号を指定することで参照できる
    ```shell
    $ cat args.sh
    #!/bin/bash
    echo '$1:' $1;
    echo '$2:' $2;
    echo '$3:' $3;
    echo '$0:' $0;
    echo '$#:' $#;

    # --------------------------
    $ ./args.sh aaa bbb ccc
    $1: aaa
    $2: bbb
    $3: ccc
    $0: ./args.sh
    $#: 3
    ($1-$3は引数、$0は実行コマンド名、$#は引数の数を示す)
    ```
  * shift文
    * 引数の順序をずらします。$2が$1に、$3が$2に・・・になります
    ```shell
    $ cat argsshift.sh
    #!/bin/bash
    echo '$1:' $1;
    echo '$2:' $2;
    echo '$3:' $3;
    shift
    echo '$1:' $1;
    echo '$2:' $2;

    # --------------------------
    $ ./argsshift.sh aaa bbb ccc
    $1: aaa
    $2: bbb
    $3: ccc
    $1: bbb
    $2: ccc
    ```
  * エスケープシーケンス
    * バックスラッシュ（\）で特別な文字を出力するために付ける
    ```shell
    #例：ダブルクオートの出力
    $ echo "\""
    # ⇒ "

    #例：改行（途中でバックスラッシュを入れても改行は無視され同じ結果となる）
    $ echo "I am Cat.\
    > My name is nothing."
    # ⇒ I am Cat. My name is nothing.
    ```
  * sourceコマンド
    * bash等のシェルの内部コマンドで、指定されたファイルを読み込んでシェル環境を設定します。
* 条件分岐
  * if 条件式1 then ... elif 条件式2 ... else ... fi
  * ファイル属性の確認
    * 演算子：-f -d -e -L -r -w -x -s
    * ファイルに関する比較演算子は属性以外にもあります。「man test」で参照してください
    ```shell
    #パスがディレクトリであれば真の値を返す
    #書き方1
    if test -d パス; then ...
    #書き方2
    if [-d] パス; then ...
    ```
  * 複数の条件を重ねる
    * 論理積：[条件1 -a 条件2]　或いは　[条件1] && [条件2]
    * 論理和：[条件1 -o 条件2]　或いは　[条件1] || [条件2]
  * 一対多の条件分岐
    * if/elif或いはcase文で記述する
    ```shell
    #!/bin/bash

    #例1
    case $1 in
            a|A)
              echo "引数にaまたはAが入力されました";;
            b|B)
              echo "引数にbまたはBが入力されました";;
    esac

    #例2
    case $1 in
            1)
              echo "引数に1が入力されました";;
            2)
              echo "引数に2が入力されました";;
            *)
              echo "1、2以外が入力されました";;
    esac
    ```
  * 繰り返し
    * for文
    ```shell
    for 変数 in 値のリスト
    do
      処理
    done
    
    #例：lsの実行結果を代入し、ループが実行される
    for i in `ls`
    ```
  * while/until文
  ```shell
  while 条件式
  do
    処理
  done
  until 条件式
  do
    処理
  done
  ```
* select文
  * ユーザに対し数値による入力を促します
  * break（繰り返しを終了）やcontinue（繰り返しの先頭に戻す）で繰り返しを制御することができる
  ```shell
  select 変数 in リスト
  do
    処理
  done

  #例1
  select name in "apple" "banana" "orange"
  do
    echo "You selected $name";
  done

  #例2
  while true
  do
    echo "Continue? (y/n)"
    read input
    case $input in
      n) break
        ;;
      y) continue
        ;;
      *) echo "Please input y or n."
        ;;
    esac
  done
  ```
* サブルーチン（関数）
  * 引数は関数の内部で$1, $2で参照することができる
  ```shell
  #Type1
  function 関数名
  {
    処理
  }
  #Type2
  関数名（）
  {
    処理
  }

  #結果を返すときはreturn文を実行する
  return 変数名
  ```
* デバッグ
  * shコマンドに「-x」オプションを付けて引数にシェルスクリプトを指定すると、コマンドや変数の中身を表示しながらスクリプトを実行します。（例：``sh -x ./sample.sh``）

***
***

## 7.ネットワークの設定と管理
* IPアドレスの確認：``ping ターゲット``
  * 「-c」オプションを付けて、pingを発行する回数を指定することもできる

## 8.ファイル管理 

.bashrc > alias (name)="blabla"
