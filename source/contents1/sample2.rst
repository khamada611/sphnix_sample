.. コメントはこんな感じ
..
   インデントする事で複数行にまたがってコメントできます。
   こんな感じで

.. warning::
    | 図が小さい場合は、以下のようにして対策することが可能です。
    | ・ブラウザの全画面とかで広げてみて下さい。
    | ・ブラウザの機能で図だけ見られる場合（「画像のみ」とか「別タブ」でとか）は、その機能で見る事が可能です。
    | ・ただし、使われている図はPNG（透過を利用）です。そのため、ブラウザソフトの標準背景が黒である場合、図がかなり見づらいです
    | ・この場合は、図を右クリックで保存して別の画像ビューワで見る方法もあります（PNG描画時に背景が明るい色である事）

##############################################################
サンプルシーケンス図あんどフローチャート
##############################################################

シーケンス図の例
=======================================

* 以下の処理は例で内容の整合性は無いのでご注意をw

以下は横長の例です。

.. seqdiag::

    seqdiag {

        #設定
        default_fontsize = 14

        test1 -> test2[label = "label1"]
        test2 -> test3[label = "label2"]
        test3 -> test4[label = "label3"]
        test4 -> test5[label = "label4"]
        test5 -> test6[label = "label5"]
        test6 -> test7[label = "label6"]
        test7 -> test8[label = "label7"]
    }

以下は込み入ったシーケンス図の例です。

.. seqdiag::

    seqdiag {

        #設定
        default_fontsize = 14

        #ノード定義 （この順番で出ます）
        h_app[shape = box]
        h_driver[shape = box]
        p_driver[shape = box]
        p_app[shape = box]

        # グループ定義
        group {
            label = "ホスト側";
            color = "#dcdcdc";
            h_app; h_driver;
        }

        group {
            label = "ペリフェラル側";
            color = "#afeeee";
            p_app; p_driver;
        }

        # シーケンス
        h_app -> h_driver[label = "CBW:ReadCapacity"]
        h_driver -> p_driver[label = "Bulk Out"]
        p_driver -> p_app[label = "CBW:ReadCapacity"]
        p_app -> p_app[label = "メディアのチェック"]

        p_driver <- p_app[label = "CSW:Response", rightnote = "コマンドの処理結果をここでは\n返す事が出来ます"];

        === USBは正常だけど相手が忙しい場合（ここから） ===

        h_driver <-- p_driver[label = "Bulk In", rightnote = "ペリフェラルで時間がかかって\n返せない場合\nNAKを戻すようになってます"]

        === USBは正常だけど相手が忙しい場合（ここまで） ===

        === ペリフェラルがテンパり、USB通信もできない場合（ここから） ===

        h_driver <-- p_driver[label = "Bulk In", failed, color="#ff0000"]
        h_driver <-- h_driver[rightnote = "ペリフェラルがNAKも\n戻せない（無通信）の時は\nエラーが発生"]

        === ペリフェラルがテンパり、USB通信もできない場合（ここまで） ===

        === 以下、正常例の続きです ===

        h_driver <- p_driver[label = "Bulk In"]
        h_app <- h_driver[label = "CSW:Response"]

        h_app -> h_driver[label = "CBW:RequestSense", leftnote = "SCSIの状態取得"]
        h_driver -> p_driver[label = "Bulk Out"]
        p_driver -> p_app[label = "CBW:RequestSense"]
        p_app -> p_app[label = "デバイス状態を確認"]
    }

書き方を変更した例です。通信内容の流れとかを記載する際はこういうやり方もあるかも。色も変えてみました。

.. seqdiag::

    seqdiag {

        #設定
        default_fontsize = 14
        activation = none
        default_note_color = "#ccffcc"

        #ノード定義 （この順番で出ます）
        client[shape = box]
        server[shape = box]

        # シーケンス
        client -> server[label = "TCP/SYN"]
        server -> client[label = "TCP/SYNACK", rightnote = "ACKだけじゃないのです"]
        client -> server[label = "TCP/ACK"]

    }

後は以下とか参照です。

* http://blockdiag.com/ja/seqdiag/examples.html

フローチャートの例
=======================================

* orientation = portrait で方向が縦になるみたいです。
* 以下の処理は例で内容の整合性は無いのでご注意をw
* 改行を使う場合はlabelを使い、￥ｎを入れます。
* ellipseとかは長いと省略されます
* 横幅が狭い時は node_width でデフォルトの幅を変更できます。何もしないと128らしいです。これは192です。

.. blockdiag::

    blockdiag {

        # 設定
        default_fontsize = 14
        node_width = 192
        orientation = portrait

        # 開始と終了
        SCSI初期化 [shape = ellipse, color = "#c0c0c0"]
        return[shape = ellipse, color = "#c0c0c0"]

        # 各処理の定義
        ReadCapacity送信処理[shape = roundedbox]
        RequestSense処理[shape = roundedbox]
        Senseデータを調べて処理を振り分ける[shape = box]

        Senseは？[shape = flowchart.condition]

        Resetの場合[shape = note]
        
        NoMediaの場合[shape = note]
        メディア待ち [shape = box, label="メディア待ち\n10msec待つ"]
        待ちのメモ[shape = minidiamond, color = "#e6e6fa", label="※１"]
        
        MediaChangedの場合[shape = note, label="Media\nChangedの場合"]
        メディアを検出[shape = box, label="メディアを検出\n初期化完了です"]

        Errorの場合[shape = note]

        正常終了[shape = box, color = "#a00000"]
        異常終了[shape = box, color = "#00a000"]

        # グループ定義
        group {
            color = "#77ffff";
            NoMediaの場合; メディア待ち; 待ちのメモ;
        }

        # 処理の流れを記述

        SCSI初期化 ->  ReadCapacity送信処理 -> RequestSense処理 ->  Senseデータを調べて処理を振り分ける -> Senseは？
        Senseは？ -> Resetの場合, NoMediaの場合, MediaChangedの場合, Errorの場合

        Resetの場合 -> ReadCapacity送信処理
        NoMediaの場合 -> メディア待ち
        メディア待ち <- 待ちのメモ[style = dotted]
        メディア待ち -> ReadCapacity送信処理
        MediaChangedの場合 -> メディアを検出 -> 正常終了 -> return
        Errorの場合 -> 異常終了 -> return

    }

* ※１：メディアが無い場合は常にここに来ますので、少しウェイトを入れて繰り返す感じです。

後は以下とか参照です。

* http://blockdiag.com/ja/blockdiag/examples.html


パケットフォーマット
=======================================

データ定義を表現する際にこんな感じでやれます。

.. packetdiag::

   packetdiag {
        default_fontsize = 16
        colwidth = 32
        node_height = 72

        0-31: First ULONG
        32: R
        33-34: 001
        35-63: Operation Code
        64-95: 16
        96-127: Data（16 bytes）[colheight = 4]
    }


* RはReservedです


後は以下とか参照です。

    http://blockdiag.com/ja/nwdiag/packetdiag-examples.html
