### データベースの構築
* **1. 企業モデル**の立案
  * データモデル（情報）  
    * ユーザやアプリケーションプログラム側から見た情報としてモデル化する
    * システム開発のメンバが理解し共有できる設計書として作成する
    * アプリケーションプログラムやハードウェアなどに依存しない形でモデル化する
    * 企業モデルの要求を満足する
  * 情報サブシステム
  * （業務機能）業務プロセスモデル  
* **2. 概念設計（概念データモデリング）**  
  DBに格納するデータを収集・整理した、DBMSに依存しないデータモデルである。E-Rモデルを用いて表す。ボトムアップアプローチで作成し、トップダウンアプローチで見直すことが多い。
  * **機能**: (1)データ定義機能、(2)整合性維持機能
  * **E-Rモデル**(Entity-Relationship Model: 実体関連モデル): 
    * 表記: **バックマン線図**(bachman diagram)が用いられることが多い。
    * 要素
      * エンティティ
        * エンティティタイプ
          * 核エンティティ
          * 記述エンティティ
          * 連関エンティティ: エンティティ間のリレーションシップがエンティティから独立した属性を持っている場合に、リレーションシップをエンティティとして定義したもの（関係モデルでは多対多のリレーションシップを表現することができないため、二つの1対多のリレーションシップに変換する必要がある）
        * エンティティインスタンス
        * 属性
      * リレーションシップ
      * カーディナリティ: リレーションシップを持つエンティティ間で、関連するインスタンス数の最小値と最大値を定める制約である
    * 作成
      * ①エンティティの抽出
      * ②主キーの確認
      * ③エンティティ間のリレーションシップの検討
      * ④エンティティ間のリレーションシップの整理
  * データベースの概念設計
      * **トップダウンアプローチ**: 管理対象であるデータ構造の全体を定義し、それをシステム開発に必要なレベルまで詳細化していく手法である。現場の情報を見落とす可能性が高い。
        * 分析手順
          * 概念データモデルの作成（E-Rモデルなど）
          * 各エンティティの主キーと外部キーを定義
          * 各エンティティのデータ属性の洗い出し
          * データの正規化によるチェック
      * **ボトムアップアプローチ**: 業務で実際に使われている画面や帳票を分析し、そこからエンティティとなるものを取り出す手法である。業務改善を有効に行うことに適している。
        * 分析手順
          * 現行ビジネスデータの調査
          * ドメインの選定：ドメインは同じ特性を持つデータ項目のまとまりである
          * 概念データモデルの作成
            * ビジネスデータごとにデータを正規化する：一般的に第3正規形に基づいて作成される
            * データ項目グループを統合する
            * E-R図を作成する
              * スーパタイプとサブタイプの識別
              * データの導出項目や重複項目：性能などの観点から論理設計においてその存在を判断する方が好ましいので、概念設計では削除しないとこが多い。
          * 標準データ項目の整備：標準名称は、シノニムやホモ二ムの発生を極力抑え、一意性を高めることができるが、長い名称になりがちである。
* **3. 論理設計（論理データモデリング）**
  概念データモデルをDBMSの規約に従って表現したデータモデルである。2次元の表を用いて表す関係モデルは、関係データベースの設計に適用されている。
  * **(1)データ操作機能**
  * **階層モデル**: 実体をセグメント、実現値をオカレンスという。階層構造において、子は親を一つしか持つことができない。
  * **ネットワークモデル**: 子が親を複数持つことができる、サイクルやループが許されている。
  * **関係モデル**: 実現値を2次元の表を用いて表すデータモデル。関係表は正規形であることが前提で、実現値が原子性を満たしていることがデータ構造の最低条件である。
    * **集合演算**: 和演算(union)、共通演算(intersection)、差演算(difference)、直積演算(cartesian product)
    * **関係演算**: 選択演算(selection)、射影演算(projection)、結合演算(join)、シータ結合演算(θjoin)、等結合演算(equal join)、自然結合演算(natural join)、外部結合演算(outer join)、商演算(division)
    * **関数従属性**
      * 完全関数従属性: 従属属性Zが、複数の属性X,Yで構成された独立属性の集合全体に関数従属している
        * {X, Y} → Z かつ X → ZとY → Zの両方とも成立しない
        * X → Y かつ　NOT X’ → Y
      * 部分関数従属性: 従属属性Zが、複数の属性X,Yで構成された独立属性の集合全体に関数従属し、同時に独立属性の一部にも関数従属している
        * {X, Y} → Z かつ X → ZまたはY → Zのいずれかが成立する
      * 自明な関数従属性: 属性集合X, Yにおいて、YがXの部分集合である時、X → Yが必ず成立する
      * 関数従属性の定理: 属性集合X、Y、Zで構成されている関係表Rから、関係表R1は射影{X, Y}、R2は射影{X, Z}。R1とR2を自然結合させた場合、元のRを導出できるとは限らない。しかし、XとYにX → Yが成立している時には、R1とR2を自然結合させると、元のRを導出できる。
      * 推移的関数従属性
    * **多値従属性**: X, Y, Z間で、Yの属性値の集合がXに依存していてZに依存していない場合、YはXに多値従属(X→→Y)している
      * 自明な多値従属性: X→→Yが成立しているが、X→→Zが成立していない場合
      * 局所多値従属性: 関係表Rの部分集合上で成立する多値従属性
      * 性質
        * X→Yが成立していれば、X→→Yも成立する
        * ZがX、Yに含まれない属性の集合である場合、X→→Yが成立するならば、X→→Zも成立する
        * X→→YとY→→Zが成立する場合、X→→Z-Yが成立する
      * 定理: X→→Yが成立するならば、{X, Y}と{X, Z}を自然結合させることで元の関係表を導出できる
    * **結合従属性**: X→→YとX→→Zが成立していて、YとZ間に従属関係も存在する場合、関係表には結合従属性が成立している。結合従属性*{A1,A2,…,An}に従うと表現する（A1:{X, Y}、A2:{Y, Z}、A3:{X, Z}）。
      * 自明な結合属性: 結合従属性*{A1,A2,…,An}に従う関係表Rにおいて、一つの属性集合の部分集合であるAiの属性値集合がRと同じ時、これを自明な結合従属という。Rの属性集合Aの部分集合がA1,A2,A3であるとき、Rが結合従属性*{{A1,A2},{A1,A2,A3}}に従うことは自明である。
    * **キー**
      * 候補キー(candidate key): 一意性、極小性
      * 主キー(primary key): 
      * スーパキー(super key): 候補キーに他の属性を加えた属性集合（極小性を満足する必要はない）
      * 非キー属性(non key attribute)
      * 外部キー: →参照制約
    * **正規形**: データの冗長性のデータの不整合（排除と更新時異常）の防止のためにデータの独立性を高めることを正規化という。正規化を進めると、関係表の増加やデータ操作時の関係表の結合の多発などの弊害も発生する。
      * 非正規形: 第1正規形の条件を満たしていないもの。属性に繰り返し項目がある（原子的でない）関係系。
      * 関数従属性に基づく正規形
        * **第1正規形**: 関係表のすべての属性が原子的である時
        * **第2正規形**: 第1正規形の関係表において、すべての非キー属性が各候補キーに完全関数従属する関係表（主キーの一部によって一意に決まる属性を別表に移す）
        * **第3正規形**: 第2正規形の関係表において、すべての非キー属性がいかなる候補キーに対しても推移的関数従属していない関係表（主キー以外の属性によって一意に決まる属性を別表に移す）
        * **ボイス・コッド(Boyce Codd)正規形**: 関係表に関数従属性X→Yが成立している時、X→Yが自明な関数従属性であるか、または、Xがスーパキーである関係表
      * 多値従属性に基づく正規形: **第4正規形**: 関係表に多値従属性X→→Yが成立している時、X→→Yが自明な多値従属性であるか、または、Xがスーパキーである関係表
      * 結合従属性に基づく正規形: **第5正規形**: 自明でない結合従属性*{A1,A2,...,An}が成立しており、Ai(1≦i≦n)がスーパキーである関係表
  * **オブジェクト指向モデル**: 属性とメソッドをカプセル化してオブジェクトとして扱う。
    * 特徴：ポリモフィズム（多相性）、インスタンス等
    * 関係モデルとの違い：**繰返し項目を許す**、属性を識別子とする必要はなく、各インスタンスは内部的なOID（オブジェクト識別子）によって識別される。
    * スーパークラスとサブクラス
    * データの保存
      * オブジェクト指向データベース（OODB: Object Oriented Data Base）：複雑なデータ構造をデータベースに表現しなければならないアプリケーションプログラムには、システム開発の負荷を軽減するに利用
      * オブジェクトーリレーショナルマッピング：事務系のアプリケーションで最も利用されている方法
      * SQLのユーザ定義データ型を用いてオブジェクト機能：オブジェクト指向に親和性の高い形でデータを取り扱うためのデータ型である
        ```sql
        CREAT TYPE {データ型名} [UNDER {スーパークラス名}] AS (
          {属性名}{データ型}, ・・・
        )
        [INSTANTIABLE | NOT INSTANTIABLE]
        NOT FINAL;
        ```
  * データベースの論理設計
    * データベース設計
      * アクセス形態の検討
        * 逐次アクセスと直接アクセス
        * 検索処理と変更処理
        * 少量データアクセスと大量データアクセス
      * データ量の検討
      * アクセスパスの検討：アクセスパスは効率良いデータ処理を行うことを目的として定めたデータの取得方法である。
        * 1. 各エンティティのデータ量とその増加率を分析する
        * 2. 運用形態を識別する
        * 3. 各エンティティに対するアクセス種別とアクセス件数を分析する
        * 4. 各エンティティの使用回数を分析する
        * 5. データ辞書に登録する
      * **UML（Unified Modeling Lnaguage/統一モデリング言語）**
        * 関係線に点線で接続されたクラスは関連クラス、2エンティティの関連の一部となる属性情報を持ちます。
        * N個のエンティティがお互いに多対多で結ばれていることはクラス図で、N項の関連は各クラスから出た線を菱形に接続することで表現します。
    * データベースの論理設計：概念設計で作成したE-R図から、アプリケーションが処理しやすいように、また、ユーザが使いやすいように、データベースの論理設計を行う。
      * テーブル設計
      * 制約：業務上必要な制約、整合性制約、非ナル制約、一意性制約、参照制約、検査制約
      * ビュー
    * 最適化設計
      * 索引（インデックス）
        * ユニークインデックスと非ユニークインデックス
        * クラスタ化インデックスと非クラスタ化インデックス
          * クラスタ化インデックス：索引の順番とレコードの格納順が一致しており、その物理的な格納場所も比較的まとまっている。一つのテーブルに一つしか設定できない。
        * 単一列インデックスと複数列インデックス
      * 索引候補：値の種類が少なく、重複が多い列、挿入・削除・更新の処理が頻繁に行われるテーブルの場合には索引を設定しない方が良い。
      * 索引の決定
        * 検索処理が主体のテーブル：多く設定した方が処理効率が良い。変更処理効率やディスク容量に要注意。
        * 変更処理が頻繁なテーブル：処理効率が低下する場合には設定しない方が良い。或いは索引を外して処理を行いその後付け直す。
        * 変更処理の頻度の低いテーブル
      * 非正規化の検討：処理効率を重視する場合には、テーブルの非正規化を検討する必要がある。
    * 分散設計
      * 分散データベース：業務に必要な機能を提供するデータベースをサイトごとに構築し、これらのDBをコンピュータネットワーク上で共有できるようにしたもの。
        * **透過性**：分散DBは一つのDBをアクセスしているかのように扱うこと
        * 構成：ボトムアップ構成、トップダウン構成
        * 形態：水平分散DB、垂直分散DB
        * 設計：各サイトの役割分担、各サイトで共通に保持するデータと独自に保持するデータの切り分け、分散のために利用するDBMSの機能
    * 論理設計の工夫：導出項目や繰返し項目を持つ場合には、整合性や拡張性を考慮する必要がある。テーブルの分割り、オプティマイザを活用したSQL分やプロトタイプによる処理性能の確認も効果的である
      * エンティティの独立と分割
      * 導出項目の保持
      * 拡張性の検討
        * 履歴データを持つエンティティ
        * 世代管理データを持つエンティティ
      * 自己ループの解消
        * 自己ループとは、エンティティ内の属性が同じエンティティ内の他の属性の外部キーとなっていること。階層構造の段数が決まっているのであれば、階層ごとのエンティティを作成することを検討すべきである。
      * 垂直分割と水平分割
      * 参照制約の実現：SQL、トリガ、アプリケーションプログラム
      * テーブルの結合：サブタイプ別に分割、列の索引化、アプリケーションプログラムで対応、非正規化
      * 効果的なSQL文：DBMSのオプティマイザの活用
        * 索引を利用する：注意事項として、中間一致検索・後方一致検索・式・否定形・索引がないのWHERE句を使う場合、索引を利用できない
        * 抽出する列を減らす
      * プロトタイプの開発：次の処理性能を確認する
        * ハード：ディスクI/O時間、ネットワーク転送時間
        * DB：レコード数と処理時間の相関、索引の型に応じた処理時間、索引が設計どおりに利用されるか、結合するテーブル数と処理時間の相関、パフォーマンス
        * データ管理：DB再編成に要する時間、バックアップに要する時間
* **4. 物理設計**
  * DBMSの動作原理：ディスクから読み込んだデータはメモリ上の入出力バッファに格納される。アクセスは入出力バッファに対して行われ、あるタイミングで入出力バッファからディスクへ書き込まれる。
  * DBの物理設計：チューニングの目的や具体的な数値目標を設定し、投入可能な資源を把握することが重要である。トランザクション処理であればレスポンスタイム、バッチ処理であれば処理時間、バックアップリカバリであれば停止時間や最長回復所要時間などが数値目標となる。
    * 作業内容
      * システム資源の割付け
        * CPU時間のチューニング：同時に実行させる必要のないプログラムを同時に実行させないように制御することが最も効果的である。
        * メモリのチューニング
          * DBMS使用領域
            * プログラムコード領域：最優先で確保すべき領域。
            * アプリケーション作業領域と入出力バッファ領域：アプリケーション作業領域には、セッション情報や構成情報などが保存される。チューニングの大きなポイントは、SQL文の解析情報を保存する**SQL解析情報保存領域**（ライブラリキャッシュやプロシージャキャッシュなどともいう）の大きさ。この二つの領域に保存されている情報のヒット率が70%以下であれば、メモリを追加することによってパフォーマンスの改善が期待できる。しかし、90％を超えている場合にはあまり期待できない。
            * （設定ファイルに指定された領域サイズを使用するものもあり、設定ファイルの領域サイズを変更しなければならない）
          * OS使用領域
            * プログラムコード領域
            * ファイルキャッシュ領域：書込みにも関係する場合、データの不整合が発生する可能性がある。DBMSにはOSのファイルキャッシュ機能を停止する機能がある。
            * 他アプリケーションの使用領域
      * テーブルの物理格納方式：同時アクセスの可能性の高いファイル同士、データファイルと索引ファイルやログファイルなど、ディスクI/Oが競合しないようにファイルは分散して格納する。インタフェースの性能にも留意する。
        * ハードディスクの物理的な性質：DBサーバのような高速処理と耐障害性が要求されるサービスでは**RAID**(Redundant Arrays of Inexpensive Disks)が用いられることが多く、RAID1とRAID5が広く利用されている
        * DBMSが使用するファイル配置を考える際には、次の点を考慮する
          * ログファイルは専用のディスクを用意し、RAID5を使用しない。これは、ログファイルはシーケンシャルアクセスファイルであるためにRAID5と相性が悪いこと、ランダムアクセスファイルとの混在によって性能が低下すること、ログファイルはボトルネックになりやすいことに起因する
          * データファイルとその索引を異なるディスクに配置し、ディスクI/O競合を回避する
          * 結合処理するファイルを異なるディスクに配置し、ディスクI/Oの競合を回避する
          * 複数のディスクを作成した場合、障害発生率が高くなり、容量管理や障害対策の手間が増えるため、ログファイル以外のファイルを配置するディスクにはRAID5を使用する
          * RAID5のままで必要なパフォーマンスを確保できる場合は、ディスク分割を行わない
        * テーブルの格納方式
          * OSの提供するファイルシステムを利用する
          * RAWデバイスを利用する（RAWデバイス：OSで確保されていないパーティション。DBMSが直接デバイスドライバを介して読み書きを行う。利用すると、OSのファイルシステムによるオーバヘッドが発生しないため、パフォーマンスが向上する。また、OSのパーティションサイズやファイルサイズの制限を受けない。反面、OSのファイルシステムを利用しないために、ファイルの運用・保守は煩雑になる。RAWデバイスの利用については、必要なパフォーマンスとメリット/デメリットを考慮して決定する必要がある）
      * 索引：データの変更とともに索引の変更も生じる。また、直接アクセスは逐次アクセスよりも処理時間がかかる。そのため、索引は有効な絞込みができるものを必要最低限設定する必要がある。
        * 一般に、索引による検索には限界があり、索引を設定して効率の良いアクセスが期待できるのは、検索によってデータが1/10以下に絞り込まれる場合と言われる。
        * 留意点
          * 索引の設定：B+木ファイルが用いられる。可能な検索方法は、完全一致検索、前方一致検索、範囲検索。また、索引を利用する際の検索条件は項目値と定数を比較することしかできないため、非正規化した導出項目をテーブルに持たせることの必要である。
          * 複合索引の設定：複合索引は、各列を繋いで一つの列にしたものに索引を設定したものである。
          * 索引利用の注意点：長期間使用しているテーブルではデータ構成が変わって統計情報が劣化してしまい、効率の悪いアクセスが行われる可能性がある。統計情報を更新する必要がある。自動更新機能を持つDBMSもあるが、負荷が高いため装備していないものもあるため、定期的に統計情報を更新するコマンドを実行する必要がある。
          * 適切な索引を設定しても、それがプログラムで利用されるとは限らない。そのため、索引の種類とそれが有効になるアクセス方法を整理してプログラム担当者に提示し、必要に応じてプログラミング担当者の見解も取り入れるような体制を整備することが重要である。
      * パーティショニング：ディスクを区切ってパーティションを作り、特定のキーに基づいてテーブルを分割して格納する。該当するパーティションのみをアクセスすればよく、ディスクI/Oにかかる時間を短縮できる
        * 長時間保存データを時系列でパーティショニングする：パーティションキーに基づいてパーティショニングする。そのキー値によってアクセスすることで、ディスクI/Oにかかる時間を大きく削減することができる。ただし、パーティションキーを用いないアクセスではオーバヘッドが発生する。また、パーティションキーに関しては、変更に制限がある、運用が複雑になるなどの問題もある。
        * ディスクI/Oの競合を防ぐためにパーティショニングする：パーティションキーを用いず、それぞれのパーティションにランダムにデータを格納する
        * メンテナンスなどの処理時間を短縮するためにパーティショニングする：データの発生日をパーティションキーとしてパーティショニングしたテーブルであれば、削除対象のパーティションを無効化すれば良いだけで、処理時間は短縮できる。
      * 論理設計へのフィードバック：チューニングによって処理効率の向上を図る必要が生じた場合、論理設計に戻ってテーブルの非正規化を行わなければならない。
***
### システム開発方法論
* **データ中心アプローチ**(DOA: Data Oriented Approach):データの整合性や一貫性の維持が行いやすく、プログラム変更が極小化できる
  * **開発手順**
    * 要求分析
    * データモデリング
    * 標準プロセス設計: 標準データの**データライフサイクル**（CRUD: データの発生(Create)・参照(Retrieve)・更新(Update)・消滅(Delete)）の機能を網羅したデータライフサイクルプログラム（DLCP: Data Life Cycle Program）を設計する。このプログラムには標準データの基本処理機能と整合性維持のチェック機能を組み込む。これによって、標準データと標準プロセスは**1対1**の関係となり、同じ標準データを操作する処理が複数のプログラムに存在するような事がなくなる。
      * トランザクションの正規化：トランザクションを標準データと標準プロセスのカプセル化と対応させて分割すること。これによって、トランザクションの簡略化と保守性の向上が実現できる。処理の冗長性が排除でき、安定的なデータ処理ができる。
      * エンティティライフサイクルの分析
        * エンティティの識別
        * イベントの識別
        * エンティティ機能関連マトリックスの作成
        * データモデルの検証
      * **エンティティ機能関連**マトリックスの検証
        * プロセスの関連が全くないエンティティがあるか
        * CRUDが揃っていないエンティティがあるか
        * CRUDが一つのプロセスに集中しているエンティティがあるか
        * 複数のエンティティのインスタンスの発生や消滅に関連するプロセスがあるか
        * あるエンティティのインスタンスの発生に関連する複数のプロセスがあるか
    * カプセル化：標準データと標準プロセスをカプセル化
    * アプリケーション設計
***
### SQL
* **SQL**
  * **データ定義言語**(DDL:Data Definition Language)
    * データ型
      * NUMERICは必ず指定した通りの精度を持つのに対し、DECIMALは指定した以上の精度を持つことが許される（4桁までの10進数は2バイトで内部表現を行なっているDBMSを考えた場合、NUMERIC(4,0)では-9999 ~ 9999の値しか格納できないが、DECIMAL(4,0)では-32767 ~ 32768の値を格納してもエラーとならない）
    * 制約
      * PRIMARY KEY制約
      * UNIQUE制約
      * NOT NULL制約（列制約、テーブル制約にはない）
      * CHECK制約
      * **FOREIGN KEY制約**：ON DELETEおよびON UPDATEで、参照先テーブルで参照行の削除や更新が行われたときのアクション（NO ACTION, RESTRICT(default), CASCADE, SET DEFAULT, SET NULL）を指定する。
    * CREATE VIEW文
      * 最後にWITH CHECK OPTIONを書けば、ビューのデータ操作を制限することができる。このオプションを指定せずにビューを作成した場合、そのビューからは見ることができない行の挿入や更新をビューに対して行うことができる。
      * 実テーブルのデータを直接参照していない場合、ビューに対する更新は出来ない例
        * DISTINCT, GROUP BY句、HAVING句を使用されているビュー
        * 列名リストに集合関数が含まれているビュー
        * 計算式によって導出された列
    * CREATE DOMAIN文：データ型、デフォルト値や条件指定の定義が同じデータが頻繁に使用される場合、新しいデータ型として定義することができる。
      ```sql 
      CREATE DOMAIN {定義域名}[AS]{データ型}[DEFAULT句][CHECK制約]
      ```
  * **データ操作言語**(DML:Data Manipulation Language)
    * 問い合わせ
      * 結合
        ```sql
        --結合
        {table1} JOIN {table2} USING {列名リスト}
        {table1} JOIN {table2} ON {結合条件}
        --クロス結合
        {table1} CROSS JOIN {table2}
        SELECT * FROM T1 CROSS JOIN T2 CROSS JOIN T3
        --内部結合
        {table1} JOIN {table2} [USING {共通列名リスト} | ON {結合条件}]
        --外部結合
        {table1} LEFT [OUTER] JOIN {table2} [USING {列名リスト} | ON {結合条件}]  -- table1のすべての行+table2の結合条件を満足する行
        {table1} RIGHT [OUTER] JOIN {table2} [USING {列名リスト} | ON {結合条件}]
        {table1} FULL [OUTER] JOIN {table2} [USING {列名リスト} | ON {結合条件}]
        ```
      * その他
        * COALESCE：引数を最初から1つずつ走査し、NULLでない値が見つかった場合にその値を返します。COALESCE(A, B)はAがNULLのときBを返し、そうでないときAを返す。
        * SAVEPOINT（セーブポイント）：長いトランザクションの途中で、マーキングをし途中からの処理を取り消すのに使用されます。その部分までロールバックすることができます。
    * 副問合せ
      * **ALL/SOME/ANY**：副問合せの結果いずれかに一致にしたい場合に利用
      * **EXIST**
    * 複数のSELECT文の結果の併合
      ```sql
      {SELECT文1} UNION {SELECT文2}      --1または2に含まれる行（重複無）
      {SELECT文1} UNION ALL {SELECT文2}  --重複を排除せずに抽出する場合に利用
      {SELECT文1} INTERSECT {SELECT文2}  --1及び2に含まれる行
      {SELECT文1} EXCEPT {SELECT文2}     --1に含まれるが、2には含まれない行
      ```
    * カーソル：SQLでは条件に該当する複数行に一括アクセスする。1行ずつのアクセスが必要な場合には、カーソルを用いて一括アクセスしたものから１行ずつ取り出すことができる。
      ```sql
      --1. カーソルを定義する
      DECLARE {カーソル名} CURSOR
        FOR {SELECT文}
        [FOR READ ONLY | FOR UPDATE [OF {列名リスト}]]
      --2. カーソルを開く
      OPEN {カーソル名}
      --3. ループ処理
      FETCH {カーソル名}
        INTO {ホスト変数名} [INDICATOR {標識変数名}], ......
        -- 標識変数は、空値を扱えない親言語で空値を扱うための仕組みです。ホスト変数に対応する列の値が空値であれば負の値、空値出なければ0が入る。標識変数を指定していない列の値が空値だった場合はエラーとなる。そのため、値が空値を取る可能性がある列には、標識変数を指定する必要がある。
        -- カーソル末尾判定：SQLCODEやSQLSTATEを利用して、直前に実行したSQL文の処理状態を示す。
          -- SQLSTATE：推奨。5桁の文字列であり、直前のSQL文が正常に終了した場合には'00000'、カソールが末尾に到達した場合には'02000'が設定される。
          -- SQLCODE：互換性のために存在する。整数で、直前のSQLが正常に終了した場合には0、カーソルが末尾に到達した場合には+100が設定される。
        -- UPDATE
        UPDATE {テーブル名}
          SET {列名} = {ホスト変数名} [INDICATOR {標識変数名}], ......
          WHERE CURRENT OF {カーソル名}
        -- DELETE
        DELETE FROM {テーブル名}
          WHERE CURRENT OF {カーソル名}
      --4. カーソルを閉じる
      CLOSE {カーソル名}
      ```
    * その他
      * **入れ子ループ法**(ネストループ法)：2つの表に含まれる行を1つずつ比較しながら結合を行っていく手法です。計算量はn^2回に比例する。結合演算の処理量を最適化するための手法です。
      * マージジョイン法（ソートマージ法）
  * **データ制御言語**(DCL:Data Control Language)
    * 権限管理
      ```sql
      GRANT {権限}, ......        -- 権限：SELECT, UPDATE, INSERT, DELETE, ALL PRIVILEGES
        ON {DBオブジェクト名}
        TO {ユーザ名}, ......     -- ユーザ名の他に、PUBLICを指定することができる
        [WITH GRANT OPTION]
      
      REVOKE {権限}, ......
        ON {DBオブジェクト名}
        TO {ユーザ名}, ......
      ```
    * トランザクション管理：複数ユーザーからの要求や障害の発生時にデータを矛盾なく一連の処理を行うように管理すること。
      ```sql
      -- トランザクションを開始する命令
      START TRANSACTION ISOLATION LEVEL {分離レベル}
        -- 分離レベル1：READ UNCOMMITTED　⇒　Phantom Read, Non-Repeatable ReadとDirty Readが起きる
        -- 分離レベル2：READ COMMITTED　⇒　Phantom Read, Non-Repeatable Readが起きる
        -- 分離レベル3：REPEATABLE READ　⇒　Phantom Readが起きる
        -- 分離レベル4：SERIALIZABLE
      
      -- トランザクションの終了
      COMMIT [WORK]
      -- トランザクション開始後のすべてのデータ操作を取り消す
      ROLLBACK [WORK]
      -- 新しいトランザクションを開始する
      SET TRANSACTION ISOLATION LEVEL {分離レベル}
      ```
***
### DBMSの仕組み
* **3層スキーマ**：DBMSは３層スキーマによってデータをアプリケーションから独立させて管理する
  * **外部スキーマ**(external schema)；個々のアプリケーションプログラムで必要なデータを、概念スキーマから抜き出して定義したものである。外部スキーマは、ネットワークモデルではサブスキーマといい、関係モデルではビューという。論理データモデルのデータ操作に対応する。
    * 論理データの独立
    * データの安全保護
  * **概念スキーマ**(conceptual schema)：論理データモデルそのものと考えてよいが、使用するDBMSの性能を考慮して処理効率の改善などの変更を加えて作成する。基本的に、論理データモデルの一つのエンティティが概念スキーマにおける一つのテーブルに対応する。
  * **内部スキーマ**(internal schema)：概念スキーマを物理ファイルとしてコンピュータ上に作成するために定義したもの。物理ファイル名と格納位置、ブロック長や領域サイズ、ファイル編成方式などを定義する。内部スキーマは、ネットワークモデルでは記憶スキーマといい、関係モデルでは特に決まった呼び方はない。
    * 物理データの独立
* **メモリ管理**：データはディスクから入出力バッファにページ単位で格納される。ディスクからの読込は入出力バッファにない場合にだけ発生し、入出力バッファに空きがなくなると古いページを追い出す。
  * 入出力バッファ：（補助記憶装置上のDBが常に最新状態であるとは限らない。そのため、DBの障害復旧時には注意が必要である）
    * **LRU**(Least Recentyly Used)：ページ置換えのアルゴリズムの代表的なもの
    * **LFU**(Least Frequently Used)
  * ページ管理：補助記憶装置上のDBから、入出力バッファのページにレコードを格納する場合のレコードの配置方法には下記二つある
    * **接近配置方法**：少ないページアクセスで大量のレコードを見つけることができる。
      * ファイル内接近配置
      * ファイル間接近配置
    * **ランダム配置方法**：レコードを格納する際に、キー値によってページを決定する方法や、OSが任意の空きページを指示する方法などがある。キー値によってページを直接アクセスできるため、少量レコードをアクセスするには有効である。
  * アクセス方法
    * 逐次アクセス(sequential access)：対象レコードを見つけるまでのアクセス時間は、レコード数nに比例する⇒アクセス時間が**O(n)**（オーダエヌ）である。
      * 格納順（物理順）アクセス
      * 論理順アクセス：レコードが次のレコードへのポイントを持つリンクリスト形式のファイルにおいて、ポインタをたどってアクセスする。
    * 直接アクセス
      * 索引：キー値とその値を持つレコードの格納場所情報を保持するファイル。（索引を検索する際は逐次アクセスとなるため、アクセス時間はレコード数nに比例してO(n)となる）
        * **索引順編成**：レコードをキー値の昇順・降順でページに格納し、ページ内のキー値の最大・小値とページへのポインタをセットで索引に持つ編成。
          * データ領域⇒ページ内あふれ域⇒共用あふれ域
          * 特徴：逐次アクセスも直接アクセスも可能。レコードの追加・削除を繰り返すとアクセス効率が悪くなる。
        * **B木編成**：索引順編成での問題点を解決するために、動的に索引保守を行う機能を持っている。キー値の順にレコードを格納した複数のページを、キー値によって上位ページと下位ページとして関連付けた木構造形式のファイル。根ページ以外のページには、格納されているレコード数が格納可能な数の半分以上になるように、更新時に動的な保守が行われる。
          * 特徴：直接アクセスの効率は良いが、逐次アクセスの効率はあまり良くない。ページ使用率は50%以上となり、索引順編成のようにレコードが特定のページに偏ることがない。
          * 2分探索⇒レコード数nのB木ファイルのアクセス時間は、**O(log2n)**
        * **B+木編成**：根、節、葉で構成された、キー値を木構造形式で格納したファイルである。葉ページ同士を前後に結ぶポインタを持っているため、逐次アクセスも効率良くできる。
          * **ページ分割**：ページに格納可能なキー値の数を超えた場合に行う。分割によって追加されたページは、ポインタによって親ページと関連付けられる。
          * **ページ統合**
          * **特徴**
            * 検索を一定のアクセス時間で行うことができる。B+木の高さをnとすると、n+1回のページアクセスで目的のレコードが取得できる。
            * レコードの挿入や削除時には、動的に更新される。
            * 葉ページ同士を前後に結ぶポインタをたどることによって、逐次アクセスを効率良く行うことができる。
        * 主索引と副次索引：主キー以外の項目に対するのは副次索引。
      * ハッシング：レコードのキー値をハッシュ関数によってアドレスに変換すること。ハッシュ関数には、キー値をページ数で割った余りを利用するものが多い。異なったキー値から同じアドレスが算出されるシノニム(synonym)が発生することがある。**シノニム**レコードは、ページ内に空きがあればそこに格納されるが、ページに空きがない場合は**オーバフロー領域**に格納される。本来格納すべきページから実際に格納されたオーバフロー領域にポインタチェーンが張られる。（オーバフロー領域を使用した場合、その増加に伴って性能が低下し、レコード数がnこの場合、最大アクセス時間はO(n)になる。）
        * 適応型ハッシング：レコード数によってハッシュ関数とページ数を動的に変化させる適応型ハッシング。オーバフロー領域の増加に伴ってアクセス時間の増大を解消できる。
          * 拡張型ハッシング：メモリ上にページとページを指す索引を持つ。ページに空きがなくなった場合に、ページの追加と索引の更新を動的に行うので、オーバフロー領域を使用する必要がない。
* **DBMSの基本構成**
  * 目的：整合性の維持、スループットの向上、応答時間の短縮、不正アクセスの防止
  * 機能
    * **同時実行制御**
      * 直列可能性
        * 直列可能性の判定方法
      * **ロック法**
        * **2相ロック方式**：ロックが増加していく成長フェーズとロックが減少していく縮退フェーズの2相によって同時実行制御を行う。直列可能性を保証するが、デッドロックが起こる可能性がある。
          * 第1相（成長フェーズ）：トランザクションは、データ操作を行う前に排他資源に対して一斉にロックをかける。必要なすべてのロックが完了する前にロックを解除することはないので、ロックの数が増加していく。
          * 第2相（縮退フェーズ）：データ操作の後に一斉にすべてのロックを解く。
        * 木制約：データに対して順序付けを行い、その順序に従ってロックをかける方法で同時実行制御を行う。直列可能性を保証するだけでなくデッドロックが起こらないことも保証する。
      * 楽観アルゴリズム：アクセス対象の資源が、他のトランザクションによって使用中か否かに関係なくアクセスする。そして、資源を更新する段階で、他のトランザクションが該当資源を更新したかどうかをチェックする。その結果、更新されていない場合にだけ更新し、更新されていた場合はトランザクションをやり直す。
    * **障害回復**：DBのバックアップコピー、トランザクションログ、チェックポイントなどの情報を事前にとっておく
      * **ロールバック処理**：ログファイルの更新前ログを参照して、トランザクションの実行結果をDBから取り消す処理である
      * **ロールフォワード処理**：ログファイルの更新後ログを参照して、トランザクションの実行結果をDBに反映する処理である
    * **コミットメント制御**
      * **2相コミットメント制御**(2PC: 2 Phase Commitment)：実行結果を確定する前にコミットもアボートも可能な不確定状態（セキュア状態）を設定し、その後に実行結果を確定するという二つのフェーズで制御を行う方法である。DBに実行結果を反映できない関係プロセスが一つでもあった場合はトランザクションがロールバックされるため、分散データベースの一貫性を保証することができる。通信障害やプロセス障害に対して有効である。
        * 手順
          * 第1フェーズ（投票相）
          * 第2フェーズ（決定相）
        * 関係プロセスの状態
          * アイドル状態
          * 不確定状態
          * コミット状態
          * アボート状態
        * 問題点⇒ブロック状態
        * 終結プロトコル
          * 単純終結プロトコル：不確定状態の関係プロセスは、指揮プロセスが障害から復旧するのを単純に待つ終結プロトコルである
          * 協調的終結プロトコル：不確定状態の関係プロセスは、他のすべての関係プロセスにSTATE-REQを送信する　→　受信した各関係プロセスは自分の状態を返信する　→　応答に従ってCOMMIT/ABORTを決定する
    * **セキュリティ**
    * **最適化**
    * 利用者インタフェース
  * クライアントサーバモデル
    * 2層クライアントサーバシステム
    * 3層クライアントサーバシステム
* **DBMSの拡張機能**
  * **ルール**
  * **ストアドプロシージャ**：一連のSQL文で構成されるプロシージャをDBサーバ内に登録しておき、クライアントから**遠隔手続き呼出し**（RPC:Remote Procedure Call）によって実行させる機能。
  * **トリガ**
* **トランザクション**：トランザクションは原子性、一貫性、独立性、耐久性が保証されていなければならない。トランザクションの同時実行は、データベースの一貫性を損なう原因になるため、制御が必要である。
  * **ACID特性**
    * 原子性(Atomicity)：トランザクションが終了した時に、DBに対するすべての処理が終了しているか、全く行われていないかのどちらかの状態であること
    * 一貫性(Consistency)
    * 独立性(Isolation)
    * 耐久性(Durability)：トランザクションが完了した後にトラブルが発生しても、DBの更新内容が保たれること
  * **トランザクションの実行履歴**
    * 各DBサーバにおける副トランザクションの実行履歴をローカル履歴という。
  * **インタリーブ実行**：トランザクションに入出力処理の終了待ちが発生した場合は、その間にCPUに他のトランザクションの処理をさせることによって処理効率を維持するようにしている。
  * **並列実行**
  * 復旧可能な履歴
  * **正しくない履歴**
    * ダーティリード：参照処理と更新処理が競合し、あるトランザクションが他のトランザクションのコミットの前にデータを参照している。
    * ロストアップデート(lost update)：トランザクションT1はトランザクションT2によって無効になり、更新結果が失われる状態。
    * ノンリピータブルリード(non-repeatable read)：トランザクションで参照を繰り返し実行できないこと。
    * ファントムリード(phantom read)：トランザクションT1の終了前にトランザクションT2がレコードを挿入した場合、トランザクションT1が同じ条件で再度抽出すると、前回抽出されなかったレコードが抽出される
  * トランザクション独立性のレベル
      
***
### 関連技術
* データウェアハウス
  * **利用技術**
    * VLDB (Very Large DB)
    * OLAP (Online Analytical Processing)：多次元データベースを構築することによって，集計及び分析を行う方式である
    * データマイニング
  * データウェアハウスの索引
    * ビットマップ索引：キー値と各データがそのキー値に合致するか否かのリストを持つ
      * 索引へのディスクI/Oが減るため、絞込み率の条件が緩和される
      * 論理和や論理積を求めることで、複数の索引を組み合わせることが可能にある
    * 部分一致検索と後方一致検索
***
* **DRM** (Data Resource Management): データ資源管理（電子化された情報のみを共有資源とみなして管理を行う）
* **IRM** (Information Resource Management): 情報資源管理（紙の資料など電子化されていないデータも共有資源とみなして管理を行う）
* **DBMS** (DataBase Management System): データベース管理システム

***
### その他
* データベース応用
  * **CAP定理**：分散処理システムにおいては、一貫性・可用性・分断耐性の3つの特性のうち、最大でも同時に2つまでしか満たすことができないとする定理です。一貫性と可用性を保証しようとすると必然的に単一のデータベースとなり、分断耐性がありません。また、一貫性と分断耐性を保証しようとすると、データベースの2相ロックや3相ロックが必要となり可用性が損なわれます（ロック中は利用できない）。そして、可用性と分断耐性を保証するシステムでは、ロックを掛けないので一貫性が損なわれます。
    * 一貫性 (Consistency)：すべてのデータ読み込みにおいて、最新の書き込みデータもしくはエラーのどちらかを受け取る。
    * 可用性 (Availability)：ノード障害により生存ノードの機能性は損なわれない。つまり、ダウンしていないノードが常に応答を返す。単一障害点が存在しないことが必要。
    * 分断耐性 (Partition-tolerance)：データを複数のサーバに分散して保管していること。システムは任意の通信障害などによるメッセージ損失に対し、継続して動作を行う。通信可能なサーバーが複数のグループに分断されるケース（ネットワーク分断）を指し、1つのハブに全てのサーバーがつながっている場合は、これは発生しない。ただし、そのようなネットワーク設計は単一障害点をもつことになり、可用性が成立しない。RDBではそもそもデータベースを分割しないので、このような障害とは無縁である。
  * **NoSQLデータベースシステム**
    * **BASE特性**：ACID特性と同様にデータベースの一貫性モデルであり、厳密な一貫性を要求せず結果整合性を保証する考え方です。BASE特性の概念は、クラウド上でビッグデータを扱うNoSQLのトランザクションに取り入れられています
      * BA（Basically Available）：基本的に利用可能である（可用性優先）
      * S（Soft State）：柔軟な状態（厳密な状態を要求しない）
      * E（Eventually Consistent）：結果整合性を保証する
  * **CEP**（Complex Event Processing, 複合イベント処理）：刻々と発生する膨大なデータをリアルタイムで解析し、条件に合致したものだけを処理する技術の総称です。データに対する処理条件や分析シナリオを事前に定義しておき、それに合致した場合に決められたアクションを即座に実行する仕組みです。
* **トランザクション処理**
  * セミジョイン法（準結合演算法）：分散型演算に特有の転送コストを少なくするための結合手法です。
  * 多版同時実行制御(MVCC：MultiVersion Concurrency Control)：複数の利用者から同時にデータベースの更新要求を受け取った場合でも、同時並行性と一貫性の両方を保証する仕組みです。
    * （更新中のデータに対する参照要求に対してトランザクション開始時点のデータのコピー（スナップショット）を提供します。同様に、参照中のデータに対する更新要求に対して、トランザクション開始前のデータのコピーを提供します。これにより、読込みのロックと書込みのロックが競合することがなくなり、トランザクションの同時並行性が向上します。）
  * WAL (Write Ahead Log)プロトコル：トランザクションがログを安定記憶(ディスク)に書き出すタイミングについての取り決めで、"Write Ahead Log"(まずログを書け)という意味のとおり、実際の操作に先行してログの即時書き出しを求めるもの。
* **情報セキュリティ**
  * ハイブリッド暗号方式：当事者同士の認証処理の後、公開鍵暗号方式を使用して共通鍵を共有し、それ以降の暗号化通信はその共通鍵を用いて行います。この仕組みにより、鍵管理の安全性を保ちつつ、高速な暗号化／復号処理が可能になっています。
  * MIME (Multipurpose Internet Mail Extension)：ASCII文字しか使用できないSMTPを利用したメールで、日本語の2バイトコードや画像データを送信するための仕組みですが、暗号化の機能はありません
  * S/MIME (Secure MIME)：ハイブリッド暗号方式を採用している電子メールを盗聴や改ざんなどから守るための技術
  * AES (Advanced Encryption Standard)：アメリカ合衆国の次世代暗号方式として規格化された共通鍵暗号方式です
  * IPsec (Security Architecture for IP)：通信で使用するのは共通鍵ですが、鍵の交換には公開鍵暗号方式ではなく鍵交換プロトコルIKE(Internet Key Exchange)を使用します
  * ベイジアンフィルタ：利用者が振り分けた迷惑メールと正規のメールから特徴を学習し，迷惑メールであるかどうかを統計的に判定する。
  * DNS水責め攻撃(Water Torture Attack，ランダムサブドメイン攻撃)：脆弱性のある公開キャッシュサーバ(オープンリゾルバ)や欠陥を持つホームルータに対して、実際には存在しない幾つものサブドメインに対するDNSクエリを発行することで、権威DNSサーバへの問合せを意図的に大量発生させる攻撃です。この攻撃を受けた権威DNSサーバは、過負荷状態となりサーバダウンやサービス停止に陥ってしまう可能性があります。権威DNSサーバがダウンすると、管理下のドメインの名前解決ができなくなるため多数のWebサイトの利用に影響を与えます。
  * カミンスキー攻撃：DNSキャッシュポイズニングの一種。標的のキャッシュサーバに，ランダムかつ大量に生成した偽のサブドメインのDNS情報を注入する。
  * DNSフラッド攻撃：標的のサーバに，ランダムに生成したサブドメインのDNS情報を格納した，大量のDNSレスポンスを送り付ける。
  * DNSアンプ攻撃：標的のサーバに，ランダムに生成したサブドメインのDNS情報を格納した，データサイズが大きいDNSレスポンスを送り付ける。
  * CSIRT (Computer Security incident Response Team)：対象とする範囲でセキュリティ上の問題が起きていないかどうかを監視するとともに、発生したセキュリティインシデントについて対応するチームや組織の総称。
  * SSH (Secure Shell)：公開鍵暗号や認証の技術を利用して、安全にリモートコンピュータと通信するためのプロトコルであり、SSLと同様にトランスポート層とアプリケーション層で通信を暗号化する。
*  開発プロセス・手法
   * BPEL (Business Process Execution Language)：複数のWebサービスを呼出し、実行する手順を結び合わせて一連の複雑なビジネスプロセスとして記述するための言語です。
   * BPMN (Business Process Modeling Notation)：ビジネスプロセスモデリング表記法のこと。ビジネスプロセスをワークフローとして視覚的に表現するための手法です。
   * SOA (Start Of Authority)レコード：DNSサーバの動作を制御するための設定情報が記述されているファイルです。シリアル番号はSOAレコードの改定番号。
   * ESB (Enterprise Service Bus)：SOA(サービス指向アーキテクチャ)を支えるミドルウェアで、SOAのサービスコンポーネント同士をバス型のトポロジーで接続します。ESBは、内部で様々な形式やプロトロルのメッセージの相互変換、データのルーティング、非同期連携などを行い、様々なアプリケーションの統合を支援します。
   * SOAP (Simple Object Access Protocol)：ソフトウェア同士がメッセージを交換する遠隔手続き呼び出し(RPC)のためのプロトコルです。汎用なデータ形式であるXMLに基づいて記述されます。
* 知的財産適用管理
  * DTCP-IP (Digital Transmission Content Protection over IP)：著作権保護されたディジタルコンテンツを伝送するための暗号化技術であるDTCPを、主にAV機器を含む家庭内LANなどのIPネットワークで使用できるようにした規格です。DTLAが発行する証明書を用いて相互認証を行い、コンテンツ保護が行えることが確認したときのみ録画・再生が可能となる仕組みになっています。
  * CPRM (Content Protection for Recordable Media)：BSデジタル放送や地上デジタル放送に採用され，コピーワンスの番組を録画するときに使われる方式。
  * CSS (Content Scramble System)：DVDに採用され、映像コンテンツを暗号化して、複製できないエリアにその暗号化鍵を記録する方式。
  * HDCP (High-bandwidth Digital Content Protection)：HDMI端子が搭載されたディジタルAV機器に採用され、HDMI端子から表示機器にディジタル信号を送るときに受信する経路を暗号化する方式。
* コンピュータ構成要素
  * メモリ
    * ECC (Error Check and Correct)：誤り訂正符号としてハミング符号や垂直水平パリティを用いることで、記録内容に発生した誤りを検知・自動訂正できる誤り制御方式。
