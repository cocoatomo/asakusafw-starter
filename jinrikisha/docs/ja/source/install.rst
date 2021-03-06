===========================
Jinrikisha インストール手順
===========================
この文書ではJinrikishaのインストール手順やインストール時の注意事項を説明します。

前提条件
========

インターネットへの接続
----------------------
インストール環境がインターネットに接続できる必要があります。

インターネットへの接続にプロキシの設定を行う必要がある場合は、 :ref:`pre-configure-maven` を参照してください。

sudoの実行
----------
``setup.sh`` を実行するするOSユーザは ``sudo`` コマンドを実行できる必要があります。

UbuntuやMacOSXではインストール時にデフォルトで ``sudo`` が実行されるよう設定されますが、CentOSではデフォルトで一般ユーザが ``sudo`` コマンドを実行できな>いよう制限されているため、必要に応じてsudoを実行できるよう設定してください。

ホスト名からIPアドレスの解決
----------------------------
Hadoopを実行する際に、自ホスト名からIPアドレスが解決できる必要があります。

UbuntuやMacOSXではインストール時にデフォルトで自ホスト名からIPアドレスが解決されるよう設定されますが、 CentOSではインストール時にデフォルトのホスト名 ( ``localhost.localdomain``) を変更した場合、必要に応じて自ホスト名からIPアドレスが解決できるよう設定してください。

インストール手順
================
Jinrikishaを使ったAsakusa Framework開発環境のインストール手順を説明します。

ここでは主にUbuntu DesktopへLinux-32bit版を使ってインストールする手順を説明しますが、他のプラットフォームにおいてもインストール手順は同様です。

setup.shの実行
--------------
ダウンロードしたJinrikishaのインストールアーカイブを展開し、展開したディレクトリに含まれる ``setup.sh`` を実行します。

..  code-block:: sh

    tar -xf jinrikisha-linux-32bit-*.tar.gz
    jinrikisha-linux-32bit/setup.sh

``setup.sh`` を実行すると、Jinrikishaのインストーラ画面が表示され、インストールが開始されます。

..  code-block:: sh

    ************************************************
                 Jinrikisha (人力車)                 
                                                  
          - Asakusa Framework Starter Package -
                                                  
            Version: x.x.x (xxx)
    ************************************************
    ...

``setup.sh`` を実行した直後にいくつかの環境チェックが行われます。チェックがすべてOKとなった場合、インストールパラメータの入力に移ります。

..  attention::
    以降の手順でインストール中にインストールを中断する場合は ``Ctrl+C`` キーを押してください。

Javaのインストール
~~~~~~~~~~~~~~~~~~
Linux版のJinriksihaは、環境変数 ``JAVA_HOME`` にJDKのパスが設定されている場合は、このJDKをAsakusa Framework開発環境用のJDKとして設定します。

``JAVA_HOME`` を設定していない場合、JinrikishaはOSにインストールされているJDKを検索します。JDKが検出された場合はこのJDKを使用してインストールを続行するかを選択します。Jinrikishaが検出したJDK以外のJDKを使用したい場合は、一旦インストールを中断し、環境変数 ``JAVA_HOME`` に使用するJDKのパスをセットした後、再度 ``setup.sh`` を実行してインストールをやり直してください。

Javaがインストールされていない環境でインストールを行った場合やJDKが検出されなかった場合は、以下の画面が表示され、インストーラによってOpenJDKをインストールしてインストールを続行するかを選択することが出来ます。

..  code-block:: sh

      Java(JDK)がインストールされていないため、
      OpenJDKをインストールしてセットアップを続行します。

      ** WARNING ********************************************************
      OpenJDKを使用せず、OracleJDKを使用する場合は
      インストールを中断してください。
  
      (OracleJDKを使用するには、OracleJDKを手動でインストールしてから
      環境変数JAVA_HOMEにOracleJDKのインストールディレクトリを設定し、
      再度 setup.sh を実行してインストールを行います)

      OracleJDKのインストール方法は以下のサイトなどを参考にしてください。
      http://java.sun.com/javase/ja/6/webnotes/install/index.html
      *******************************************************************

    
    OpenJDKをインストールしてインストールを続行しますか？:[Y/n]:

インストール時にユーザのパスワード入力を促された場合は、パスワードを入力して処理を続行してください。

..  code-block:: sh

    [sudo] password for asakusa: 

..  attention::
    入力を促される表示で ``[Y/n]:`` もしくは ``[y/N]`` と表示された場合、大文字になっている文字がデフォルトの選択肢を表し、何も文字を入力しないで ``Enter`` キーを押下すると、 大文字になっている文字を入力したことと同じになります。 
    
    また、 ``y`` または ``Y`` 以外の文字を入力すると、 ``n`` を選択したことと同じになります。

..  attention::
    Asakusa FrameworkのOpenJDKによる動作検証はOracleJDKと比べて十分に行われていません。またOpenJDKを使ったインストール時に、稀にJavaのコンパイルエラーが発生しインストールに失敗する事象が報告されています。

    動作の安定性を重視する場合は、OracleJDKの利用を推奨します。

MacOSX版では、Javaがセットアップされていない場合、OracleJDKのインストーラが起動します。インストール画面の指示に従ってインストールを行った後、インストールを続行してください。

インストールパラメータの入力
-------------------------------
インストールの課程で、いくつかのインストールパラメータの入力を行います。

1. インストールディレクトリの入力
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Asakusa Frameworkの開発環境をインストールするディレクトリパスを指定します。何も入力しない場合、デフォルトで ``$HOME/asakusa-develop`` が指定されます。

..  code-block:: sh

    1) Asakusa Framework開発環境のインストールディレクトリ(ASAKUSA_DEVELOP_HOME)を入力してください。: /home/asakusa/asakusa-develop: 

..  note::
    インストール時に既に同名のディレクトリが存在した場合は、既に存在するディレクトリを ``<元ディレクトリ名>_<タイムスタンプ(YYYYMMDDHHMMSS)>`` に変更してからインストールが行われます。

2. Asakusa Framework バージョンの入力
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
インストールするAsakusa Frameworkのバージョンを入力します。何も入力しない場合、デフォルトでJinriksihaの推奨バージョンが指定されます。

..  code-block:: sh

    2) Asakusa Frameworkのバージョンを入力してください。: 0.2.6: 

..  attention::
    指定可能なバージョンは ``0.2.4`` 以降です。 指定可能なバージョン文字列の一覧は、以下のURLで公開されているAsakusa Frameworkのアーキタイプカタログを参照して下さい。アーキタイプカタログのうち、 archetypeIdが ``asakusa-archetype-windgate`` を持つ archetypeに含まれる ``version`` の文字列を指定することが可能です。

    http://asakusafw.s3.amazonaws.com/maven/archetype-catalog.xml

.. _configure-profile:

3. ログインプロファイルに対する環境変数追加の設定
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
OSユーザのログイン時に読み込まれるプロファイルにAsakusa Frameworkを使った開発に必要な環境変数を追加するかを選択します。

この設定を行うと、OSユーザのログイン時に以下の画面説明に示す環境変数がログイン時に読み込まれます。OSユーザをAsakusa Frameworkの開発専用のユーザとして使用する場合は、環境変数を追加すると便利でしょう。

..  code-block:: sh

    3) /home/asakusa/.profile に環境変数の設定を追加しますか？

    ** WARNING ********************************************************
    * この設定を行う場合、以下の環境変数が設定されます。
      - JAVA_HOME=/usr/lib/jvm/java-6-openjdk-i386
      - ASAKUSA_DEVELOP_HOME=/home/asakusa/asakusa-develop
      - ASAKUSA_HOME=${ASAKUSA_DEVELOP_HOME}/asakusa
      - M2_HOME=${ASAKUSA_DEVELOP_HOME}/maven
      - HADOOP_HOME=${ASAKUSA_DEVELOP_HOME}/hadoop
      - PATH: $JAVA_HOME/bin:$M2_HOME/bin:$HADOOP_HOME/bin: \
              $ASAKUSA_DEVELOP_HOME/eclipse:$ASAKUSA_HOME/yaess/bin: \
              $PATH

    * インストールする環境にすでに
      Java,Maven,Hadoop,Asakusa Frameworkがインストールされている場合、
      これらの環境変数による影響に注意してください。

    * この設定を行わない場合、
      Jinrikishaでインストールした各ソフトウェアを使用する前に、
      シェルに対して以下のように環境変数を適用する必要があります。

    ### シェルに対して環境変数を追加
    $ . /home/asakusa/asakusa-develop/.rikisha_profile

    *******************************************************************

    /home/asakusa/.profile に環境変数の設定を追加しますか？:[Y/n]: 

..  note::
    ログインプロファイルは、 OSユーザの環境に ``$HOME/.bash_profile`` が存在した場合は ``$HOME/.bash_profile`` に対して追加し、 ``$HOME/.bash_profile`` が存在しない場合は ``$HOME/.profile`` に追加します。

4. Eclipseのショートカット追加の設定
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
:ref:`configure-profile` で環境変数設定の追加を行った場合、 デスクトップにEclipseのショートカットを追加するかを選択出来ます。

..  code-block:: sh

    4) デスクトップにEclipseのショートカットを追加しますか？:[Y/n]:

5. (MacOSX版のみ) デスクトップ環境に対する環境変数追加の設定 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
MacOSX版では、:ref:`configure-profile` で環境変数設定の追加を行った場合でも、OSユーザログイン時にデスクトップ環境に対して環境変数を読み込ませるために、ログインプロファイルの他に ``/etc/launchd.conf`` に環境変数を追加する必要があります。

この設定を行うことで、OSユーザのログイン時にAsakusa Frameworkを使った開発に必要な環境変数が読み込まれます。

..  code-block:: sh

    5) EclipseをGUI(Finder,Dock,Spotlightなど)から起動するために
       必要な環境変数を /etc/launchd.conf に追加しますか？

    ** WARNING **********************************************
    この設定はOS全体に適用されるため、
    他のアプリケーションに影響を与える可能性があります。

    この設定を行わない場合、
    Eclipseはターミナルまたはデスクトップのショートカットから
    起動してください。

    (EclipseをGUIから起動してもAsakusa Frameworkを使った
    アプリケーションのテストが正常に動作しません)
    *********************************************************

    /etc/launchd.conf に環境変数を追加しますか？:[Y/n]: 

インストールの実行
------------------
インストールのパラメータ入力が完了すると、以下の画面が表示されてインストールの続行を促されます。注意事項を確認し、 ``Enter`` キーを押してください。

..  code-block:: sh

    ------------------------------------------------------------
    インストールの準備が完了しました。
    以下の注意事項を確認した上で、[Enter]キーを押してください。
    ------------------------------------------------------------

    ** WARNING ***********************************************************
    1) Mavenリモートリポジトリからライブラリをダウンロードするため、
       インストールには10分以上かかる可能性があります。
    **********************************************************************

    インストールを続行するには[Enter]キーを押してください。: 

インストールが完了したら、以下の画面が表示されます。

..  code-block:: sh

    ------------------------------------------------------------
    インストールが成功しました。
    ------------------------------------------------------------

:ref:`configure-profile` で環境変数設定の追加を行った場合、以下の画面が表示されOSの再起動が促されますので、再起動を行なってください。

..  code-block:: sh

    デスクトップ環境に対して /home/asakusa/.profile の変更を反映するためOSを再起動してください。
    今すぐにOSを再起動しますか？:[y/n]: 

..  attention::
    OSの再起動(デスクトップ環境への再ログイン)が行われていない場合、デスクトップ環境からEclipseを起動しても環境変数が適用されていないためAsakusa Frameworkが正常に動作しません。

..  attention::
    インストールに失敗・中断した場合、ターミナルの最下行に以下のメッセージが表示されます。

    ``Finished: ABORT``

    この場合、画面に表示されているエラーメッセージを確認してください。

README(Getting Started)の表示
-----------------------------
インストール完了後、インストールディレクトリ(デフォルトは ``$HOME/asakusa-develop`` )  配下に ``README`` ファイルが作成されています。これは、Asakusa Frameworkの開発環境で使用するコマンドやEclipseの使い方などを簡単にまとめた Getting Started が記述されています。

インストールディレクトリ構成
----------------------------
JinrikishaによってインストールされたAsakusa Framework開発環境のインストールディレクトリ構成を以下に示します。

..  list-table::
    :widths: 3 7
    :header-rows: 1

    * - ディレクトリ/ファイル
      - 説明
    * - ``asakusa``
      - Asakusa Frameworkのインストールディレクトリ
    * - ``eclipse``
      - Eclipseのインストールディレクトリ
    * - ``hadoop``
      - Hadoopのインストールディレクトリ
    * - ``maven``
      - Mavenのインストールディレクトリ
    * - ``repository``
      - Mavenのローカルリポジトリ用ディレクトリ
    * - ``workspace``
      - Eclipseのワークスペース用ディレクトリ
    * - ``README``
      - Asakusa Framework開発環境の使い方が簡単にまとめたGetting Startedが記述されたテキストファイル
    * - ``.rikisha_profile``
      - Jinrikshaでセットアップした各ソフトウェアの動作に必要な環境変数の設定ファイル

インストールオプション
======================
``setup.sh`` のインストールオプションを以下に示します。

  ``-r`` [Mavenリポジトリのtarアーカイブファイル名]

    指定したMavenローカルリポジトリの内容をJinrikishaのインストールディレクトリ配下に展開します。これにより、Mavenリポジトリからのダウンロード時間を短縮することができます。

    例えばJinrikishaを再インストールする場合は、以下のようにするとよいでしょう。

..  code-block:: sh
    
    tar -cf /tmp/repository.tar.gz -C ~/jinrikisha repository
    ./setup.sh -r /tmp/repository.tar.gz

.. _pre-configure-maven:

インストール前にMavenの設定を行う
=================================
インターネットへの接続にプロキシサーバを経由する必要がある環境については、Mavenに対してプロキシの設定を行う必要があります。

Mavenの設定を変更する場合は、 ``setup.sh`` を実行する前にJinrikishaのアーカイブファイルに含まれる ``_template/maven/conf/settings.xml`` を編集し
、Mavenに対して適切な設定 [#]_ を行ってください。

..  [#] Mavenのプロキシ設定については、Mavenの次のサイト等を確認してください。 http://maven.apache.org/guides/mini/guide-proxies.html

