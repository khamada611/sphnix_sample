.. コメントはこんな感じ
..
   インデントする事で複数行にまたがってコメントできます。
   こんな感じで

#################################
サンプルドキュメント（レベル１）
#################################

ちなみに以下書いている事の技術的な整合性は問うてはなりません。

サンプルドキュメント（レベル2）
=======================================

サンプルドキュメント（レベル3）
---------------------------------------

**普通に文章をダラダラ書くと以下のようになります(強調文字ですこれ)**

Windows Vistaのデバイスドライバに、従来からのWDMに加えて新しいKMDFとUMDFが登場した。開発効率と安定性、セキュリティの向上をもたらす新しいデバイスドライバの仕組みと開発手法について、USBストレージシステムを実際に開発しながら解説する。 

普通に改行（一行空ける）した後の文章がこれです。

**以下は改行を任意に行った例です。(強調文字ですこれ)**

| ドライバ開発者はWDFを覚えなくてはならない。
| オブジェクト指向をC言語で実現すべく立ちはだかったKMDF
| 更にはCOMと結託し強大な力で襲い掛かるUMDF
|
| これらの強敵に立ち向かうべく、
| ドライバ本はパワーアップを遂げないといけない
| かくして「WindowsXPデバイスドライバプログラミングは
| 「Windows Vistaデバイスドライバ」となった。

項目はこんな感じで出来ます

* 項目1
* 項目2
* 項目3

#. 番号付き項目その１
#. 番号付き項目その２
#. 番号付き項目その３

画像はこんな感じで。

.. image:: images/sample.png

数式はこんな感じで。

.. math::

      \csc x & = & \frac{1}{\sin x} \\
      \sec x & = & \frac{1}{\cos x} \\
      \cot x & = & \frac{1}{\tan x}

| もしくは :math:`y = x^{2 \pi}`. みたいに文章内に入れることも出来ます。数式はLaTexで書くのですが、書き方は以下とかが参考になります。
| http://www.latex-cmd.com/

リンクとかはこんな感じで列挙も出来ます。

* http://sphinx-doc.org/
* `オープンな設計図でお馴染みのGitHub <https://github.com>`_

.. _seq1:

シーケンス図（レベル3）
---------------------------------------

以下は簡単なシーケンス図とテーブルの例です。実際はsample2の方が参考になるかと思います。

.. seqdiag::
   :desctable:

   seqdiag {
      default_fontsize = 14
      Browser -> Server[label = "追加リクエスト"];
      Server -> DataBase[label = "データ追加"];
      Browser [description = "フロント側でUIを担当します"];
      Server [description = "バックエンド側で処理を行います"];
      DataBase [description = "各種データをとりまとめます"];
   }

上のはseqdiagで書いたものです。reSTでは下のように書けますので、こちらを使った方が良いかと思います。

=========  =====  ===============================
名前        ID     内容
=========  =====  ===============================
Browser    1       フロント側でUIを担当します
Server     2       バックエンド側で処理を行います
DataBase   3       各種データをとりまとめます
=========  =====  ===============================

以下はCSVファイルから作成する例です。固定枠ならこれが楽かもしれません。

.. csv-table::
   :file: sample1.csv
   :encoding: utf-8
   :header-rows: 2

ソースコード（レベル3）
---------------------------------------

ソースコードは以下のように記載できます。**code-blockは上下に空行を一行設ける必要があります。**
インデントをつける必要があるのがちょっと厄介です。VisualStdioCodeなら選択肢て「Ctrl + ]」でやれます（参考までに。Ctrl + [で左側に戻す事も可能）

以下はPythonの例。こことcode-blockにも改行が必要です。

.. code-block:: python

      #!/usr/bin/env python
      # -*- coding: shift-jis -*- 
      
      import datetime

      print '現在時刻書きます'
      now = str(datetime.datetime.today())
      out = open('nowtime.txt', 'w')
      out.write('現在時刻：' + now)
      out.close()
      print 'complete ' , now

次にC言語の例

.. code-block:: c

      #include <initguid.h>
      #include <wdm.h>
      #include "usbdi.h"
      #include "usbdlib.h"

      //　デバイス固有データに関する定義
      typedef struct _DEVICE_CONTEXT {
            PUSB_CONFIGURATION_DESCRIPTOR	ConfigurationDescriptor;
            USHORT							wTotalLength;
            WDFUSBDEVICE                    WdfUsbTargetDevice;
            WDFUSBPIPE						BulkReadPipe;
            WDFUSBPIPE						BulkWritePipe;
      } DEVICE_CONTEXT, *PDEVICE_CONTEXT;

      VOID 
      VistaSample3_DbgPrintFileName(
      IN WDFFILEOBJECT FileObject
      )
      {
      PUNICODE_STRING fileName;

      // WDFFILEOBJECTからファイルネームを取得
      fileName = WdfFileObjectGetFileName(FileObject);
      if (fileName->Length > 0) {
            // ファイルネームは，UNICODEという，Windowsでは
            // おなじみの2バイトを1文字とした文字コードで
            // 発行されるので，文字数はWCHAR，つまり2で割る
            ULONG nameLength = (fileName->Length / sizeof(WCHAR));

            DbgPrintEx(DPFLTR_IHVDRIVER_ID ,DPFLTR_ERROR_LEVEL,\
                  "Filename = %ws nameLength = %d\n", fileName->Buffer, nameLength);
      }
      else
            DbgPrintEx(DPFLTR_IHVDRIVER_ID ,DPFLTR_ERROR_LEVEL,\
                  "Filename not found.\n");   
      }

ハイライトする言語は以下で定義されているものという事です。

* http://pygments.org/docs/lexers/


preタグっぽい事（レベル3）
---------------------------------------

以下の感じでpreタグっぽい事もできます。

例::

    <pre></pre>みたいな
    そのまま使いたいテキストをこうやって配置できます。



リンクの例（レベル3）
---------------------------------------

* 文章内へ飛ぶ場合

:ref:`seq1` に飛びます。

飛び先でもしかけが必要になるので注意です。飛ばしたい文節の前に定義を入れます。その後は一行空行が必要です。参照する場合はrefの後に空白一文字あった方が良いです。定義側では_をつけ、参照する側では_を取った形で使います。

* ファイルへのリンク

後ZIPとかダウンロードさせたい時はこういう書き方も出来ます。

右クリックで保存も出来るよ！（→） :download:`絵のサンプル  <images/sample.png>`

なので資料とか、参考ソースコードをzipして取り出させるみたいな使い方も出来ますよ

最後に(レベル４)
^^^^^^^^^^^^^^^^^^^^^

これでおしまいです。

.. note::
    こうやって注釈をつけることが可能です。

.. warning::
    こうやって注意を促すことも可能です。
