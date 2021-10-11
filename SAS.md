## 基本コンセプト
### SASプログラム
* **構成**: SASプログラムはDATAステップやPROCステップやDATAステップとPROCステップの任意の組み合わせで構成できます。
  * DATAステップ：通常SASデータセットを作成または編集します。また、独自のレポートを作成するためにも使用できます。
  * PROC（プロシジャ）ステップ：SASデータセットのデータを分析し処理することのできる、あらかじめ準備されたルーチンを起動したり、呼び出したりします。
* **SASステートメント**（DATA、SET、RUN、その他）
  * SASキーワードで始まり、必ずセミコロンで終わる。
  * フリー・フォーマット
  * （空白や特殊文字はSASステートメントのワードを区切ります。大文字でも小文字でもSASステートメントを指定できます。ほとんどの場合、引用符で囲まれたテキストは、大文字小文字を区別します。）
* **編集**
  * **省略形の利用**：文字列を入力してタブキーまたはEnterキーを押すと、その文字列が展開されるように定義することができる
    * 作成：Tool > Abbreviation
  * **複数のファイルのビューを開く**：そのファイルをアクティブ・ウィンドウにして、Window > New Windowを選択する（1つビューでの変更を同時に反映される）
  * **拡張エディタのオプションの設定**：Tool > Option > Enhanced Editor（行番号の表示などを設定できる）
    * **拡張エディタのエラーチェック**
      * Ctrl+]で終了の角括弧や丸括弧
      * Alt+[でDO-ENDペアをマッチさせる
  * **クリア**：「編集」＞「すべてクリア」
    * ウィンドウからクリアされたテキストはリコールできません。しかし、「編集」＞「元に戻す」を選択してクリアしたテキストを再表示することができる。
* **DATAステップデバッガでのデバッグ**：DATAステップの論理的エラーをデバッグすることができる。デバッガは対話モードでのみ使用することができる。
  * ***DEBUG***オプション
    ```sas
    data perm.publish / debug;
      infile pubdata;
      input BookID $ Publisher & $22. Year;
    run;
    proc print data=perm.publish;run;
    ```
  

***

### 処理の結果
* 形式をリストか、HTMLかに指定することができる（Proferenceで設定）
* その他のプロシジャ出力
  * 対話レポート・ウィンドウ（データを直接変更するために使用するウィンドウ）
  ```sas
  proc report data=sasuser.admit; 
      columns id name sex age actlevel; 
  run;
  ```
  * Tabulateプロシジャ
  ```sas
  proc tabulate data=sasuser.admit; 
      class sex; var height weight; table sex*(height weight),mean; 
  run;
  ```

***

### ライブラリ
SASファイルをさん参照する前に、ファイルが保存されるライブラリに名前を割り当てなければなりません。
* **タイプ**
  * 一時：ファイル作成する際にライブラリ名を指定しない時（あるいはworkを指定）、セッション終了時、一時ライブラリとそのすべてのファイルは削除される。
  * 永久：Work以外のライブラリ名を指定
    * デフォルトライブラリ(Sashelp, Sasuser, Work)
* SASライブラリを削除する場合、ライブラリへのポインタが削除され、SASはそのライブラリにもうアクセスしなくなります。しかし、ライブラリの内容は、オペレーティング環境にはまだ存在します。
* 2レベルの名前：LIBREF.FILENAME
* **LIBNAMEステートメント**
  * ***LIBNAME libref (engine) ‘SAS-data-library’;***
    * libref: 1文字から8文字で、アルファベットかアンダースコアから初め、アルファベット、数字、アンダースコアだけを含みます
    * (engine): 他の形式のファイルの参照（SASファイルだけでなく他のソフトウェア・プロダクトで作成されたファイルも参照することができる。その時、SASが自動的に適したエンジンを選択するものがありますが、どのエンジンを使用するのか伝える必要があるものもあります）
      * インターフェイス・ライブラリ・エンジン：BMDP, OSIRIS, SPSS
      * SAS/ACCESSエンジン：お使いのサイトにSAS/ACCESSソフトウェアがライセンスされていれば、LIBNAMEステートメントを使用してDBMSファイルに保存されているデータにアクセスすることができます。 
    * 有効時間：現在のSASセッションの間のみ（SASエクスプローラから新規ライブラリ・ウィンドウを使用して作成されるライブラリは、「起動時に有効」を選択すると起動時に自動的に割り当てることができる）
      ```sas
      libname recipe 'c:\Users\...';
      data recipe.yakisoba1;
          set recipe.yakisoba0;
      run;
      ```
* **ライブラリコンテンツの表示**
  * **PROC CONTENTSの利用**
    * ***PROC CONTENTS DATA=SAS-file-specification NODS;RUN;***
      * **NODS**は_ALL_を指定する場合に各ファイルについての詳細情報の印字を省略します（_ALL_を指定する場合のみNODSを指定できます）
      ```sas
      proc contents data=clinic._all_ nods;
      run;
      ```
  * **PROC DATASETSの利用**
    * SASファイルのコピー、削除、修正などの多くの管理タスクを実行することができる。
    * ***PROC DATASETS<options>; CONTENTS<options>; QUIT;***
      ```sas
      proc datasets;
        contents data=clinic._all_ nods;
      quit;
      ```
  * **VARNUMオプションの指定**
    * PROC CONTENTSとPROC DATASETSに利用でき、変数一覧表示をアルファベット順の代わりに、データセットの論理的な場所の順（作成順）に変数名を列記する
    ```sas
    proc contens data=sasuser.admit varnum; run;
    
    proc datasets;
      contents data=sasuser.admit varnum;
    quit;
    ```

***

### SASデータセット
* **データセットの部分**
  * **ディスクリプタ部**：データセット属性、変数属性
  * **データ部**
    * 変数（列）
    * オブザベーション（行）
  * **インデックス**
* **名前（変数名・データセット名など）のルール**：1から32文字の長さで、アルファベット（A-Zの大文字または小文字）かアンダースコアで開始しなければならず、数字、またはアルファベット、またはアンダースコアの任意の組合わせを続けることができます。

***
### システム・オプション
* **OPTION**ステートメントでそれ以降の設定を変更できる。SASセッションを終えるまで有効。
* System Option Windowからも設定できる（Tool > Option > System)（セッションの終了まで有効）
* ***DATE|NODATE, NUMBER|NONUMBER***オプション
  * ページ番号と日付の出力（デフォルトで表示される）
  ```sas
  ods listing;
  options nonumber nodate;
  proc print data=sasuser.diabetes;
    var id sex age height weight;
    where age>=30;
  run;
  ods listing close;
  ```
* ***PAGENO***オプション
  * ページ番号を印字する場合、リストレポートの開始ページ番号を指定することができる
  * 指定しない場合、出力はSASセッションを通して1ページから連続的に数字が付けられる
  ```sas
  ods listing;
  options number pageno=3;
  proc print data=hrd.funddrive;
  run;
  ods listing close;
  ```
* ***PAGESIZE***オプション(PS=)
  * 各ページに何行含めるかを指定する
* ***LINESIZE***オプション(LS=)
  * 印字ライン幅を指定する。ラインサイズに入らないオブザベーションは次の行に続けられる。
* ***YEARCUTOFF***オプション
  * 2桁の年の扱い：2000年問題。どの100年の期間が使用されるのかを指定する（デフォルト=1920）。
  * SASは1582年~20000年までの日付を正確に表す。
  ```sas
  options yearcutoff=1925;
  ```
* ***FIRST / OBS***オプション
  * FIRSTOBS=に対して処理する最初のオブザベーション番号を指定する（デフォルトは1）
  * OBS=に対して処理する最後のオブザベーションの番号を指定する（デフォルトはMAX、お使いのオペレーティング環境で表せる最大の8バイトの符号付き整数）
  * システム・オプション或いはデータセット・オプションとして指定できる
  ```sas
  * システム・オプションとして指定
  options firstobs=10 obs=15;
  proc print data=clinic.heart;
  run;
  * データセット・オプションとして指定
  proc print data=clinic.heart(firstobs=10 obs=15);
  run;
  ```
* **追加機能**
  * ***FORMCHAR = 'formatting-characters'***
  * ***FORMDLIM = delimit=character'***
  * ***LABEL | NOLABEL***
  * ***REPLACE | NOREPLACE***
  * ***SOURCE | NOSOURCE***
  * ***ERRORS***
  * ***FMTERR | NOFMTERR***
  * ***SOURCE | NOSOURCE***
***
### 基本レポートの作成
* **変数の選択**：***VAR*** variables(s);（1つまたはそれ以上の空白で区切られた変数名）
* **OBS列の削除**：***NOOBS***オプション
* **オブザベーションの識別**：***ID*** variables(s);（レポートの各行の最初に、オブザベーション番号の代わりに印字する1つ以上の変数を指定する）
  * オブザベーションが1行で印字するには長すぎる場合に特に便利。
* **オブザベーションの選択**：***WHERE*** where-expression;（複数のWHEREステートメントが発行されると、最後のステートメントのみが処理される）
  * **比較演算子**：=/eq, ^=/ne, >/gt, </lt, >=/ge, <=/le
  * **CONTANTS演算子**：CONTAINS/?（指定した部分文字列を含むオブザベーションを選択する）
```sas
libname patients 'c:\records\patients';
proc print data=patients.therapy noobs;
  var age height weight fee; 
  id idnum lastname;
  where (firstname ? 'Jon' and ID>'1050') or fee in (124.80,178.20);
run;
```
* **並べ替え**：***PROC SORT DATA=(SAS-data-set) <OUT=(SAS-data-set>;BY <DESCENDING> BY-variable(s);RUN;***
  ```sas
  proc sort data=clinic.admit out=work.wgtadmit;
    /*weightの昇順で、その中をageの昇順でオブザベーションを表示している*/
    by weight age;  
  run;
  proc print data=work.wgtadmit;
    var age height weight fee;
    where age>30;
    sum fee;
    by actlevel;
  run;
  ```
* **列合計の生成**：***SUM*** variables(s);（レポートの最後に表示される）
  * **BYステートメント**:***BY <DESCENDING> BY-variable-1 <...<DESCENDING> BY-variable-n>> <NOTSORTED>;
    * BYステートメントに2つ以上の変数がある場合、DESCENDINGはすぐ後の変数にだけ適用される
    * BY変数の同じ値を持っているオブザベーションが連続していないならば、プロシジャは各連続しているセットを別々のBYグループとして扱う
    * **BYグループとID変数でのカスタム・レイアウトの作成**：ID、BY、SUMを同時に使用する
  * **別ページでの小計の要求**：***PAGEBY*** BY-variable;（指定される変数は、PROC PRINTステップのBYステートメントにも指定されていなければなりません）
    ```sas
    proc sort data=clinic.admit out=work.activity;
      by actlevel;
    run;
    proc print data=work.activity;
      var age height weight fee;
      sum fee;
      by actlevel;
      id actlevel;
      pageby actlevel;
    run;
    ```
* **ダブルスペース・リスト出力**：***DOUBLE***オプション（HTML出力には適用されません）
  ```sas
  proc print data=clinic.stress double;
    var resthr maxhr rechr;
    where tolerance='I';
  run;
  ```
* **タイトルとフットノートの指定**：***TITLE/FOOTNOTE***<n> 'text';
  * 使用すると、プロシジャ出力に最大10個のタイトル・フットノートを関連付けることができる
  * どこにでも記述できる。デフォルトのタイトルは「SASシステム」、フットノートは指定されるまで印字されない
  * TITLESおよびFOOTNOTESウィンドウの利用：TITLESコマンド・FOOTNOTESコマンドを発行して指定する。（現SASセッションの間のみ効果を持続する）
  * 編集と取り消し：再定義すると、すべてのより大きい番号のタイトルやフットノートをそれぞれ取り消す（すべて取り消すには「***title1;***」「***footnote;***」を指定する）
  ```sas
  title1 'Heart Rates for Patients with';
  title3 'Increased Stress Tolerance Levels';
  footnote1 'Data from Treadmill Tests';
  proc print data=clinic.stress;
    var resthr maxhr rechr;
    where tolerance='I';
  run;
  ```
* **説明的なラベルの割り当て（一時的に）**：***LABEL*** _variable1='label1' variable2='label2' ...;_（PROCステップにだけ適用される）
  ```sas
  proc print data=clinic.therapy label;
    label walkjogrun='Walk/Jog/Run' height='Height in Inches';
  run;
  ```
* **データ値のフォーマット化（一時的に）**：***FORMAT*** _variable(s) format-name;_（PROCステップにだけ適用される）
  ```sas
  proc print data=clinic.admit;
    format fee dollar4.;
    format net comma5.0 gross comma8.2;
    format net gross dollar9.2;
  run;
  ```
* **永久的に割り当てられるラベルと出力形式**：DATAステップで割り当てる
  ```sas
  data sasuser.paris;
    set sasuser.laguardia;
    where dest="PAR" and (boarded=155 or boarded=146);
    label data='Departure Date';
    format date date9.;
  run;
  ```
* **ラベルのテキスト文字の区切り位置の制御**：***SPLIT***オプション
  ```sas
  proc print data=reps split='*';
    var salesrep type unitsold net commisiion;
    label salesrep='Sales*Representative';
  run;
  ```
* **独自の出力形式の作成**
  ```sas
  proc format;
    value $repfmt 'TFB'='Bynum' 'MDC'='Crowley' 'WKK'='King';
  run;
  proc print data=vcrsales;
    var salesrep type unitsold;
    format salesrep $repfmt.;
  run;
  ```
***
### 外部ファイルからのSASデータセットの作成
* **外部ファイルの参照**：***FILENAME*** _fileref 'filename';_
  * SASセッション終了するまで効果が持続。参照されているファイルの詳細を表示するには「ファイルショートカット」
  * 参照（外部ファイルの識別）：***INFILE*** _file-specification <options>;_
  * 保存場所内のファイルの参照：括弧内の個々のファイル名によってファイル参照名を指定する
    ```sas
    infile tax('refund');
    ```
  * 生データファイルの内容や構造をよく知らないならば、***PROC FLIST***を使用して表示できる
* **カラム入力**：***INPUT*** _variable <$> startcol-endcol...;_
  * ＄は文字としての変数タイプを指定する（変数が数値ならば、ここに何も指定しない）
  * データは標準文字値か標準数値でなければなりません（他の入力はフォーマット入力などを利用）
  * データは固定長フィールドでなければなりません（固定長フィールドでない場合、リスト入力を利用）
  * DATAステップは無効なデータの結果であっても失敗せず、実行を継続する（ログのNOTEで確認できる）
  ```sas
  filename exer 'c:\users\exer.dat';
  data exercise;
    infile exer;
    input ID $ 1-4 Age 6-7 ActLevel $ 9-12 Sex $ 14;
  run;
  ```
* **変数の作成と編集&データのサブセット化**
  * 演算子
    * 負の接頭辞(-)：優先順位I（例：-x）
    * 指数(\**)：優先順位I（例：x\**y）
  * 数値演算で使用された値が欠損の場合、式の結果は欠損です。
  * **サブセットIFステートメント**：式がfalseならば、そのレコードやオブザベーションに対する後続の処理は行われず、制御がDATAステップの最初に戻る。
  ```sas
  data sasuser.stress;
    infile tests;
    input ID 1-4 Name $ 6-25 RestHR 27-29 MaxHR 31-33
      RecHR 35-37 TimeMin 39-40 TimeSec 42-43
      Tolerance $ 45;
    TotalTime=(timemin*60)+timesec;
    IF tolerance='D';
    TestData='01jan2000'd;  /*TestDateに日付値を割り当てる*/
  run;
  ```
* **ストリーム内データの読込**：***DATALINES;***ステートメント
  * DATAステップの最後のステートメントとして使用、直後にデータ行を記述する
  * 入力データの最後を示すために、NULLステートメント（単一のセミコロン）を使用する
  * DATAステップに1つだけDATALINESステートメントを使用できる。複数のデータのセットを入力するには、別のDATAステップを使用してください
  * DATAステップの最後でデータ行のすぐ前のステートメントとして、LINES;やCARDS;も使用できる。LINESもCARDSもDATALINESステートメントの別名
  * データがセミコロンを含むならば、DATALINES4ステートメントと4つのセミコロン(;;;;)からなるNULLステートメントを使用する
  * 最後にRUNステートメントが必要ない
  ```sas
  data sasuser.stress;
    input ID 1-4 Name $ 6-25 RestHR 27-29 MaxHR 31-33
      RecHR 35-37 TimeMin 39-40 TimeSec 42-43
      Tolerance $ 45;
    if tolerance='D';
    TotalTime=(timemin*60)+timesec;
    datalines;
  2458 Murray, W 72 185 128 12 38 D
  2590 Wilcox, E 78 189 138 14 57 I
  ;
  ```
* **生データファイルの作成**
  * _NULL_キーワードの利用
  * **生データファイルの指定**：***FILE*** _file-specification <options> <operating-environment-options>;_
  * **データの記述**：***PUT*** _variable startcol-endcol…;_
  ```sas
  data _null_;
    set sasuser.stress;
    file ‘c:\clinic\patients\stress.dat’;  /*ここにファイル参照名としてPRINTと指定すると、SASはプロシジャアウトプットファイルに行を書き出す*/
    put ID 1-4 Name $ 6-25 RestHR 27-29 MaxHR 31-33
      RecHR 35-37 TimeMin 39-40 TimeSec 42-43;
  run;
  ```
* **Microsoft Excelデータの読み込み**
  * **SAS/ACESS LIBNAME**：***LIBNAME*** _libref ‘location-of-Excel-workbook’ <options>;_
    * 物理位置を指し示すことによってExcelワークブックにSAS名を関連付ける。ExcelはSASの新しいライブラリになり、ワークブック内のワークシートがそのライブラリの個々のSASデータセットになる。
    * SAS/ACCESS Interface to PC Filesのラインセンスが必要
    * 名前定義：Excel内で定義し、名前を割り当てたワークシート内のセルの範囲
    * ライブラリ参照名の解放：SASがExcelワークブックに割り当てたライブラリ参照名を持つ場合、ワークブックはExcelで開くことができない
    * **<options>**
      * ***DBMAX_TEXT=n***：nは256から32767の間の整数（デフォルト1024）
      * ***GETNAMES=YES|NO***：（デフォルトYES）
      * ***MIXED=YES|NO***：文字と数値の両方をインポートし、すべてのデータを文字に変換するかどうかの指定（デフォルトNO。NOは文字のタイプが割り当てられている場合に、数値データが欠損値になることを指定する。数値データが割り当てられている場合に、文字データは欠損値になる）
      * ***SCANTEXT=YES|NO***：データ列全体を読み込み、見つかった最大の長さの文字をSAS列の幅として使用するかどうかの指定（デフォルトYES。NOは調べず、255をデフォルトとする）
      * ***SCANTIME=YES|NO***：日時列の全ての行の値を調べ、時間値だけが存在する場合に、TIME.出力形式を自動的に決定するかどうかの指定（デフォルトNO）
      * ***USEDATE=YES|NO***：日時値に対してDATE9.出力形式を使用するかどうかの指定（デフォルトYES。NOはDATETIME.出力形式）
    ```sas
    libname results ‘c:\users\exercise.xlsx’;
    proc contents data=results._all_; /*読込の確認*/
    run;
    data work.stress;
      set results.’ActLevel$’n;  /*’ActLevel$’nはSAS名前リテラルです*/
    run;
    proc print data=stress; /*読込の確認*/
    run;
    libname results clear;  /*ライブラリ参照名の解放*/
    ```
  * **Excelワークシートの作成**
    ```sas
    libname clinic ‘c:\users\newExcel.xlsx’ mixed=yes;
    data clinic.high_stress;
      set work.high_stress;
    run;
    ```
  * **インポート・エクスポートウィザード**
    * [File] > [データのインポート・エクスポート]
    * さまざまなタイプの外部ファイルからSASデータセットを作成することができる
***

### プログラミング・ワークスペース利用のTips
* **メニューバーの表示**：表示されていないメニューバーを表示するには、コマンドボックス（またはツールボックス）かコマンド行にpmenusと入力する。
* **Key**：「ツール」⇒「オプション」⇒「キー」
* **Code Editor Window**
  * 拡張エディタ・プログラムエディタ
    * 様々な方法でSASプログラムを開く
      * ***X***: 保存されているプログラムのファイル名が確かでない場合、「X」コマンドを発行してSASセッションを一時停止してホストシステムに入ることができる（Windows:EXIT, UNIX:exit, z/OS:RETUREN/ENDでSASセッションを再開できる）
      * ***INCLUDE 'file-specification'***: プログラムを開く
      ```sas
      include 'd:\programs\sas\myprogl.sas`
      ```
    * SASプログラムの入力、編集、サブミット（プログラムエディタでSASプログラムをサブミットする場合は、ウィンドのコードは自動的にクリアされます）
    * コマンドラインまたはメニューの使用
    * SASプログラムの保存
    * 内容のクリア
    * (拡張エディタのみ)SASプログラム言語のカラーコードや構文チェック
    * (拡張エディタのみ)セクションの展開や折りたたみ
    * (拡張エディタのみ)記録可能なマクロ
    * (拡張エディタのみ)キーボードのショートカットのサポート（AltまたはShiftキーと同時に押す）
    * (拡張エディタのみ)複数回の「元に戻す」と「やり直し」


***
### プログラムエディタ
* **行番号**：***nums***
  * SASセッションの間だけ表示されている。（永久的に行番号を表示するために、Tool > Option > Program Editorで設定できる）
* **テキストエディタの行コマンド**：テキストエディタの行コマンドによって、テキストの削除、挿入、移動、コピー、繰り返しができる。
  * ***Cn,Dn,In,Mn,Rn,A,B***
* **ブロック・テキストエディタ行コマンド**
  * ***DD,CC,MM,RR,A,B***
* **SASプログラムのリコール**：SASステートメントはサブミットする際にプログラムエディタ・ウィンドウから消去されます。しかし、「実行」＞「最後のサブミットをリコール」を選択して、プログラムエディタへプログラムをリコールすることができる。
