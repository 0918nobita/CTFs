# CpawCTF Level 1

## Q1. [Misc] Test Problem

問題文に FLAG が含まれているのでコピペして正解した。

## Q6. [Crypto] Classical Cipher

問題文にある通り、アルファベットを 3 文字分ずらすタイプのシーザー暗号を復号を行う問題。  
[外部ツール](http://ango.satoru.net/?n=3&_n=&e=seesaa&tseesaa=fsdz%7BFdhvdu_flskhu_lv_fodvvlfdo_flskhu%7D)を利用して復号して正解した。

## Q7. [Reversing] Can you execute ?

渡された ``exec_me`` のファイルの形式を確認させ、実行して FLAG を確認させる問題。  
まず docker で構築した Arch linux の仮想環境に ``binutils`` を導入し、それに含まれる ``objdump`` を用いて ``exec_me`` のフォーマットを確認した。

```bash
$ objdump -f exec_me

exec_me:     file format elf64-x86-64
architecture: i386:x86-64, flags 0x00000112:
EXEC_P, HAS_SYMS, D_PAGED
start address 0x0000000000400440
```

``elf64-x86-64`` とのことなので、``chmod u+x exec_me`` で実行権限を与えて ``./exec_me`` で普通に実行すると FLAG が標準出力された。
