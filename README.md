DCImageAnimation
===============================

画像のタイマーアニメーションとコマ送りアニメーションを実行する「DCImageAnimation」クラスです。

アニメーションは主に以下のパラメータを渡して使用します。

1. アニメーションの種類（終了時のメソッド処理分岐用）
2. アニメーションのフレーム数
3. アニメーション画像ファイル名の接頭詞
4. アニメーションのリピート回数／ループの有無

デリケードメソッドを指定する事もでき、アニメーション終了後に任意のメソッドを呼ぶことができます。
