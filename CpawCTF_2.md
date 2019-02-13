# CpawCTF Level 2

## Q13. [Stego] 隠されたフラグ

与えられた画像から、隠された FLAG を見つけ出す問題。  
左上と右下に現れる点と線がモールス信号を表しており、[外部ツール](http://morse.ariafloat.com/en/) を用いて復号することで FLAG を得た。

## Q15. [Web] Redirect

アクセスすると別のページにリダイレクトするサイトから FLAG を見つけ出す問題。  
``curl`` を用いてリダイレクト前の HTTP 通信のレスポンスの内容を確認して見つけた。

```bash
$ curl -v http://q15.ctf.cpaw.site
* Rebuilt URL to: http://q15.ctf.cpaw.site/
*   Trying 157.7.52.186...
* TCP_NODELAY set
* Connected to q15.ctf.cpaw.site (157.7.52.186) port 80 (#0)
> GET / HTTP/1.1
> Host: q15.ctf.cpaw.site
> User-Agent: curl/7.54.0
> Accept: */*
>
< HTTP/1.1 302 Found
< Server: nginx
< Date: Wed, 13 Feb 2019 09:30:44 GMT
< Content-Type: text/html; charset=UTF-8
< Content-Length: 0
< Connection: keep-alive
< X-Powered-By: PHP/7.1.8
< X-Flag: cpaw{<FLAG>}
< Location: http://q9.ctf.cpaw.site
<
* Connection #0 to host q15.ctf.cpaw.site left intact
```

## Q17. [Recon] Who am I ?

ストーカーのようなことをすれば解けた (ユーザー名が含まれている画像をググって探した) 。

## Q22. [Web] Baby's SQLi - Stage 1 -

特設サイト上で SQL 文を実行して FLAG を得る問題。  
**Stage1. Writing SQL** については ``SELECT * from palloc_home`` を実行してテーブルの内容を表示させることで FLAG を得た。  
同時に、Stage 2 で使用するサイトの URL も与えられた。
