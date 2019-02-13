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

## Q10. [Forensics] River

渡された画像から位置情報を取り出し、川の名前を FLAG とする問題。  
macOS の「プレビュー」アプリで該当の画像を開いて位置情報を表示させ、マップ上で位置を確認して答えがわかった。

## Q11. [Network] pcap

パケットを記録した ``.pcap`` ファイルを受け取り、中から FLAG を見つけ出す問題。  
``tcpdump`` というツールを使えば内容が見れるらしい。(参考: [serverfault](https://serverfault.com/questions/38626/how-can-i-read-pcap-files-in-a-friendly-format))  
macOS にいつの間にかインストールされていたので以下のコマンドを実行して FLAG を発見した。

```bash
$ tcpdump -n -A -r network10.pcap
reading from file network10.pcap, link-type EN10MB (Ethernet)
12:12:51.815374 IP 169.254.144.80 > 169.254.144.81:  ip-proto-0 20
E..(....@..7...P...Qcpaw{<FLAG>}......
12:12:51.815454 IP 169.254.144.81 > 169.254.144.80: ICMP 169.254.144.81 protocol 0 unreachable, length 48
E..Ds...@..D...Q...P........E..(....@..7...P...Qcpaw{<FLAG>}
```

``tcpdump`` のオプションについて：

| オプション | 意味 |
| --- | --- |
| -n | アドレス変換 (192.168.1.123 => examplehost) やポート番号変換 (80 => http) をしない |
| -A | キャプチャデータを ASCII 表示 |
| -r [filename] | 読み込むファイルの指定 |

## Q12. [Crypto] HashHashHash!

ハッシュ関数 SHA1 を用いて生成されたハッシュ値を受け取り、元に戻す問題。  
[外部ツール](https://sha1.gromweb.com/?hash=e4c6bced9edff99746401bd077afa92860f83de3)を利用して元の文字列を得た。

## Q14. [PPC] 並び替えろ！

与えられた整数値の配列をソートして文字列に変換したものを FLAG とする問題。  
以下の JavaScript プログラムを作成して解いた。

```javascript
let arr = [15,1,93,52,66,31,87,0,42,77,46,24,99,10,19,36,27,4,58,76,2,81,50,102,33,94,20,14,80,82,49,41,12,143,121,7,111,100,60,55,108,34,150,103,109,130,25,54,57,159,136,110,3,167,119,72,18,151,105,171,160,144,85,201,193,188,190,146,210,211,63,207];
arr.sort((a, b) => b - a);
console.log(arr.join(''));
```
