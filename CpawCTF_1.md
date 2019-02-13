# CpawCTF Level 1

## Q1. [Misc] Test Problem

問題文に FLAG が含まれているのでコピペして正解した。

## Q6. [Crypto] Classical Cipher

問題文にある通り、アルファベットを 3 文字分ずらすタイプのシーザー暗号を復号を行う問題。  
[外部ツール](http://ango.satoru.net/?n=3&_n=&e=seesaa&tseesaa=fsdz%7BFdhvdu_flskhu_lv_fodvvlfdo_flskhu%7D)を利用して復号して正解した。

## Q7. [Reversing] Can you execute ?

渡された ``exec_me`` のファイルの形式を確認させ、実行して FLAG を確認させる問題。  
まず docker で構築した Arch linux の仮想環境に ``binutils`` を導入し、  
それに含まれる ``objdump`` を用いて ``exec_me`` のフォーマットを確認した。

```bash
$ objdump -f exec_me

exec_me:     file format elf64-x86-64
architecture: i386:x86-64, flags 0x00000112:
EXEC_P, HAS_SYMS, D_PAGED
start address 0x0000000000400440
```

``elf64-x86-64`` とのことなので、``chmod u+x exec_me`` で実行権限を与えて  
``./exec_me`` で普通に実行すると FLAG が標準出力された。

## Q8. [Misc] Can you open this file ?

種類が不明なファイル ``open_me`` を受け取り、適切なアプリケーションで開いて FLAG を見つける問題。  
macOS のターミナルで ``file`` コマンドを使って種類を特定した。

```bash
$ file open_me
open_me: Composite Document File V2 Document, Little Endian, Os: Windows, Version 10.0, Code page: 932, Author: �v��, Template: Normal.dotm, Last Saved By: �v��, Revision Number: 1, Name of Creating Application: Microsoft Office Word, Total Editing Time: 28:00, Create Time/Date: Mon Oct 12 04:27:00 2015, Last Saved Time/Date: Mon Oct 12 04:55:00 2015, Number of Pages: 1, Number of Words: 3, Number of Characters: 23, Security: 0
```

``Composite Document File V2 Document`` とのことなので、とりあえず Microsoft Word で開いてみると  
FLAG が書き込まれていた。

## Q9. [Web] HTML Page

指定されたサイトの HTML に含まれている FLAG を見つけ出す問題。  
Chrome で該当サイトにアクセスして、開発者向けコンソール上で実際の HTML を見ると ``<head>`` 要素内の ``<meta name="description" content="flag is ...">`` 要素に FLAG が含まれていた。
