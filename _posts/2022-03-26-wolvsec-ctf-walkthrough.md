---
layout: post
title: wolvsec-ctf-walkthrough
date: 2022-03-28 00:06:52
categories: CTF
---

# Web
## Challenge: Warmup: Burp

### 100

@Author: green_r00t Easy

Felt a little dizzy and loopy when I wrote this! My blood sugar is probably a little low...should grab some cookies!

[https://burp-bvel4oasra-uc.a.run.app](https://burp-bvel4oasra-uc.a.run.app/)

- Abri el URL en el browser de burp
- Me redirige a un video de youtube - <https://www.youtube.com/watch?v=5mGuCdlCcNM>
- Al revisar el historial HTTP en Burp, veo que hay varios requests con un parametro `count` que aumenta con cada request
```http
GET	/
GET	/?count=1
GET	/?count=2
GET	/?count=3
GET	/?count=4
GET	/?count=5
GET	/flag?count=1
GET	/?count=7
GET	/?count=8
GET	/?count=9
GET	/?count=10
GET	/?count=11
GET	/?count=12
GET	/?count=13
GET	/?count=14
```

- Al revisar los requests, noté que todos, excepto el que hace GET a `flag?count=1` tienen este header en el _response_
```http
Interesting: d3JvbmcgcGFnZS4uLnJlZGlyZWN0aW5n=
```

Al enviarlo a _Decoder_ y decodificarlo como `base64`, me tira:
```
wrong page...redirecting=
```

- Revisando los responses que tienen este header, por ejemplo el `count=5`, todos muestran contenidos similares:
>Request

```http
GET /?count=5 HTTP/2
Host: burp-bvel4oasra-uc.a.run.app
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.74 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "(Not(A:Brand";v="8", "Chromium";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9

```

>Response

```http
HTTP/2 302 Found
X-Powered-By: Express
Interesting: d3JvbmcgcGFnZS4uLnJlZGlyZWN0aW5n=
Location: /flag?count=1
Vary: Accept
Content-Type: text/html; charset=utf-8
X-Cloud-Trace-Context: c06f5e541f2d5ad5a5f4521ef9b50f83
Date: Sun, 27 Mar 2022 05:26:53 GMT
Server: Google Frontend
Content-Length: 70
Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000,h3-Q050=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"

<p>Found. Redirecting to <a href="/flag?count=1">/flag?count=1</a></p>
```

Luego, si veo el request y response del que tira a `/flag?count=1`:
>Request

```http
GET /flag?count=1 HTTP/2
Host: burp-bvel4oasra-uc.a.run.app
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.74 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "(Not(A:Brand";v="8", "Chromium";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9

```

>Response

```http
HTTP/2 302 Found
X-Powered-By: Express
Location: /?count=7
Vary: Accept
Content-Type: text/html; charset=utf-8
X-Cloud-Trace-Context: 675999f39f4c295e3e9dc73b3260bcd5
Date: Sun, 27 Mar 2022 05:26:54 GMT
Server: Google Frontend
Content-Length: 62
Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000,h3-Q050=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"

<p>Found. Redirecting to <a href="/?count=7">/?count=7</a></p>
```

Aquí, se me ocurrió modificar el request para que ahora tire el GET hacia `flag?count=7`, con lo que obtuve lo siguiente:
>Request

```http
GET /flag?count=7 HTTP/2
Host: burp-bvel4oasra-uc.a.run.app
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.74 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "(Not(A:Brand";v="8", "Chromium";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9

```

>Response

```http
HTTP/2 200 OK
X-Powered-By: Express
Set-Cookie: PASSWORD=NONE
Content-Type: text/html; charset=utf-8
Etag: W/"53-O2qFcGQIEGc4VTKhC9/vq8ZpUkI"
X-Cloud-Trace-Context: 026aeda6dcdef56f375bdd8a4a6b7fc0;o=1
Date: Sun, 27 Mar 2022 05:48:09 GMT
Server: Google Frontend
Content-Length: 83
Expires: Sun, 27 Mar 2022 05:48:09 GMT
Cache-Control: private
Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000,h3-Q050=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"

You must be authenticated to reach this page! <! –– password: "DESSERT" ––>
```

Decidí enviarlo a _Repeater_ y, luego de intentar un montón de veces con codificar el valor de la cookie como `base64`, vi que no funcionaba.

Luego de **mucho** _trial and error_, se me ocurri'o poner la cookie como plaintext, y así fue como obtuve la flag:
>Request

```http
GET /flag?count=7 HTTP/2
Host: burp-bvel4oasra-uc.a.run.app
Cookie: PASSWORD=DESSERT
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.74 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "(Not(A:Brand";v="8", "Chromium";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9

```

>Response

```http
HTTP/2 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Etag: W/"11-pQnjkcyZT0SwAsR7uAyUYN5xqgQ"
X-Cloud-Trace-Context: 5eb04568300efa36372a8c984ced8e18
Date: Sun, 27 Mar 2022 06:00:22 GMT
Server: Google Frontend
Content-Length: 17
Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000,h3-Q050=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"

wsc{c00k1e5_yum!}
```

>FLAG

```
wsc{c00k1e5_yum!}
```

# OSINT
## U of M Student Orgs!

### 293

@Author: green_r00t

What is the name of the student organization that hosted a “Hats & Scarves Sale” in December 2012? Also go check out our club on MaizePages: [https://maizepages.umich.edu/organization/wolvsec](https://maizepages.umich.edu/organization/wolvsec) Flag format: wsc{clubinitials}

- Abrí la página
![](/assets/u-of-m-student-orgs-site.png)

- Como tiene una sección _EVENTS_, la abrí
![](/assets/u-of-m-student-orgs-events.png)

- Estando ahí, busqué `hats sale 2022`. Para poder ver eventos pasados, le hice click en _SHOW PAST EVENTS_
![](/assets/u-of-m-student-orgs-search.png)

- Al abrir el link de _Hat $ Scarf Sale_, encontré en la descripción que fue organizado por GDC
![](/assets/u-of-m-student-orgs-sale-description.png)

>FLAG

```
wsc{GDC}
```