# telnet-mail

> test smtp, imap and pop3 with telnet

![](assets/telnet-mail.png)

## SMTP

#### Encode username

```sh
perl -MMIME::Base64 -e 'print encode_base64("hello\@example.com");'
```

#### Encode password

```sh
perl -MMIME::Base64 -e 'print encode_base64("world");'
```

### SMTP 25

#### Open connection

```
telnet mail.example.com 25
```
```
Trying 93.184.216.34...
Connected to mail.example.com.
Escape character is '^]'.
220 mail.example.com ESMTP Postfix (Debian)
```
---
```
EHLO localhost
```
```
250-mail.example.com
250-PIPELINING
250-SIZE 10240000
250-ETRN
250-STARTTLS
250-AUTH PLAIN LOGIN
250-AUTH=PLAIN LOGIN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250 DSN
```

#### Authenticate

```
AUTH LOGIN
```
```
334 VXNlcm5hbWU6
```
---
```
aGVsbG8=
```
```
334 UGFzc3dvcmQ6
```
---
```
d29ybGQ=
```
```
235 2.7.0 Authentication successful
```

#### Send email

```
MAIL FROM: email@example.com
```
```
250 2.1.0 Ok
```
---
```
RCPT TO: email@example.org
```
```
250 2.1.5 Ok
```
---
```
DATA
```
```
354 End data with <CR><LF>.<CR><LF>
```
---
```
Subject: Hello

world
.
```
```
250 2.0.0 Ok: queued as 941731C4
```

#### Close connection

```
QUIT
```
```
221 2.0.0 Bye
Connection closed by foreign host.
```


### SMTP 587

#### Open connection

```
openssl s_client -starttls smtp -crlf -connect mail.example.com:587
```
```
CONNECTED(00000003)
depth=2 O = Digital Signature Trust Co., CN = DST Root CA X3
verify return:1
depth=1 C = US, O = Let's Encrypt, CN = Let's Encrypt Authority X3
verify return:1
depth=0 CN = mail.example.com
verify return:1
---
Certificate chain
 0 s:CN = mail.example.com
   i:C = US, O = Let's Encrypt, CN = Let's Encrypt Authority X3
 1 s:C = US, O = Let's Encrypt, CN = Let's Encrypt Authority X3
   i:O = Digital Signature Trust Co., CN = DST Root CA X3
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIFYjCCBEqgAwIBAgISAxXZeGDOdodqxT4UhxwNc/6yMA0GCSqGSIb3DQEBCwUA
MEoxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MSMwIQYDVQQD
ExpMZXQncyBFbmNyeXB0IEF1dGhvcml0eSBYMzAeFw0yMDA2MTAxNjAwMDVaFw0y
MDA5MDgxNjAwMDVaMB8xHTAbBgNVBAMTFHRlc3Quc2lsaWNvbmhpbGxzLmNvMIIB
IjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAy+pcYHNQKOCe/VqzM4lNpnh8
725ApwmNr2UlB8j6eB2dMTI3y4TO5hdOto1YmLXU+KMTjYMlPGcNM4n3aetDccVH
QVHYCPx0Bup9zhQg9kr/wTxXkK24QLKYfsY/WbavnfIG2VQjrTqx8l6sZ6zh0Dnk
MySFXXfsTpvW5eCzxzxuVPZfAIvIqx085XwlWYJ0EEkR86rI78F8+m13RJwufNz5
P6SLCWyBDeGyX86M1LYPIgw18XZHBNNuWx3x2AK5wQbIcc1/RKmVc3NgKkf5EXr9
jtaPzhPpr5vqrultt+BGlOJoqds8MDtg8rrYtEpl1o5YQzKRbSzoTgdD4/ukFwID
AQABo4ICazCCAmcwDgYDVR0PAQH/BAQDAgWgMB0GA1UdJQQWMBQGCCsGAQUFBwMB
BggrBgEFBQcDAjAMBgNVHRMBAf8EAjAAMB0GA1UdDgQWBBTEW5ZE870KtGjlAZaj
SjHRjtFcWzAfBgNVHSMEGDAWgBSoSmpjBH3duubRObemRWXv86jsoTBvBggrBgEF
BQcBAQRjMGEwLgYIKwYBBQUHMAGGImh0dHA6Ly9vY3NwLmludC14My5sZXRzZW5j
cnlwdC5vcmcwLwYIKwYBBQUHMAKGI2h0dHA6Ly9jZXJ0LmludC14My5sZXRzZW5j
cnlwdC5vcmcvMB8GA1UdEQQYMBaCFHRlc3Quc2lsaWNvbmhpbGxzLmNvMEwGA1Ud
IARFMEMwCAYGZ4EMAQIBMDcGCysGAQQBgt8TAQEBMCgwJgYIKwYBBQUHAgEWGmh0
dHA6Ly9jcHMubGV0c2VuY3J5cHQub3JnMIIBBgYKKwYBBAHWeQIEAgSB9wSB9ADy
AHcAXqdz+d9WwOe1Nkh90EngMnqRmgyEoRIShBh1loFxRVgAAAFynyyvJgAABAMA
SDBGAiEA1tdH0cgg5v0/Ricjg3sDs1xAJNdv136kYu2x1kxf3vUCIQDhC7Bwf+ox
vo5LzLsqdyI1c6gGWDM/lADggl9nHjtuOAB3ALIeBcyLos2KIE6HZvkruYolIGdr
2vpw57JJUy3vi5BeAAABcp8srxEAAAQDAEgwRgIhAOhX6wmVg7T+lvzbKYcaVf92
GHGJ1eao/UFStdc4VKFzAiEA0x+p6CU//x5J97CME+85nI9Rw++mTtKNJbm5gKcV
XdcwDQYJKoZIhvcNAQELBQADggEBAAWTVlFwPCCiKA5jP0H1jgtOumpqHkGVPo0o
xPv9k47EBh6UnU+L5W2RtJjAWcZF1U75pR3zQMXk0TSDtxI22dbQH5aYkYaKYWYU
AlXS5PdauSnKI1Fb1OB8Gn13U4z9lihyHC0ylYcoJYg5lh/cpRHwEErpJS5j0rKL
gFYgzt+P94BYROW6KBnFCLr6RLM9SfwTEX4+f2l5dqpFOMc9Im6tAdyHCOPnkkdq
GA3TAJzNDmFhf26DLfqRS6IzfBOgIleCJ/qGNc5G+hLAO8/4gT3ZkHbKwcW8LIDM
xWz91SqRjdsILDV9hLT/9G1uWlliqYSbeGWE8StA2gyWbo+X8mw=
-----END CERTIFICATE-----
subject=CN = mail.example.com

issuer=C = US, O = Let's Encrypt, CN = Let's Encrypt Authority X3

---
No client certificate CA names sent
Peer signing digest: SHA512
Peer signature type: RSA
Server Temp Key: ECDH, P-256, 256 bits
---
SSL handshake has read 3461 bytes and written 481 bytes
Verification: OK
---
New, TLSv1.2, Cipher is ECDHE-RSA-AES256-GCM-SHA384
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES256-GCM-SHA384
    Session-ID: 4C458E6D4FBE5636024E418E05A83EB2CE76FC12118C8155513CE01695A485BD
    Session-ID-ctx: 
    Master-Key: E0D221502C950C8B8603C7F3CCD6C3929570119F1F929E32F97D8F3B78D76A5D8473428AA67F473A1A1F16112A626921
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 9a 0c ac 1f 3f e4 9f be-b1 3d 24 da 4f 93 a8 20   ....?....=$.O.. 
    0010 - e0 19 dc e6 8f 6c aa 46-79 ef fe 04 22 10 8e c0   .....l.Fy..."...
    0020 - 3b 8a 55 a9 86 16 64 2b-e0 a5 89 d1 3d f7 5e e3   ;.U...d+....=.^.
    0030 - 0a df 4a d9 59 77 c1 33-25 a4 b3 f9 a1 7d 05 24   ..J.Yw.3%....}.$
    0040 - f8 e9 71 7c c3 75 0b 29-05 41 13 d2 f2 e1 e2 db   ..q|.u.).A......
    0050 - c6 8d 3a 34 b0 54 8d 47-41 32 a0 ef 5d 1e 6a ce   ..:4.T.GA2..].j.
    0060 - 2e f7 85 28 8a 3a 88 74-40 85 b1 e4 8d 8e 79 93   ...(.:.t@.....y.
    0070 - 10 88 08 d6 4b fa 42 5a-31 6c f5 71 07 48 65 48   ....K.BZ1l.q.HeH
    0080 - 51 e9 7a 39 26 cc f8 9a-b6 40 c7 48 9d 87 cb 33   Q.z9&....@.H...3
    0090 - 78 a7 17 67 bb 0d 7e e7-2e f9 93 3b 2c 57 fe 7d   x..g..~....;,W.}
    00a0 - bb b2 ec 9e cd d6 92 73-f1 9d 53 6f c0 f6 c4 98   .......s..So....
    00b0 - 8c a5 0e 51 34 23 24 64-dd 6f e3 b4 1f dc 5c e5   ...Q4#$d.o....\.

    Start Time: 1591881644
    Timeout   : 7200 (sec)
    Verify return code: 0 (ok)
    Extended master secret: yes
---
250 DSN
```
---
```
EHLO localhost
```
```
250-mail.example.com
250-PIPELINING
250-SIZE 10240000
250-ETRN
250-AUTH PLAIN LOGIN
250-AUTH=PLAIN LOGIN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250 DSN
```

#### Authenticate

```
AUTH LOGIN
```
```
334 VXNlcm5hbWU6
```
---
```
aGVsbG8=
```
```
334 UGFzc3dvcmQ6
```
---
```
d29ybGQ=
```
```
235 2.7.0 Authentication successful
```

#### Send email

```
MAIL FROM: email@example.com
```
```
250 2.1.0 Ok
```
---
```
RCPT TO: email@example.org
```
```
250 2.1.5 Ok
```
---
```
DATA
```
```
354 End data with <CR><LF>.<CR><LF>
```
---
```
Subject: Hello

world
.
```
```
250 2.0.0 Ok: queued as 941731C4
```

#### Close connection

```
QUIT
```
```
221 2.0.0 Bye
Connection closed by foreign host.
```

## IMAP

### IMAP 143

#### Open connection

```
telnet mail.example.com 143
```
```
Trying 93.184.216.34...
Connected to mail.example.com.
Escape character is '^]'.
* OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE LITERAL+ STARTTLS LOGINDISABLED] Dovecot ready.
```

#### Authenticate

```
. LOGIN hello world
```

### IMAP 993

#### Open connection

```
openssl s_client -connect mail.example.com:993
```
```
CONNECTED(00000003)
depth=2 O = Digital Signature Trust Co., CN = DST Root CA X3
verify return:1
depth=1 C = US, O = Let's Encrypt, CN = Let's Encrypt Authority X3
verify return:1
depth=0 CN = mail.example.com
verify return:1
---
Certificate chain
 0 s:CN = mail.example.com
   i:C = US, O = Let's Encrypt, CN = Let's Encrypt Authority X3
 1 s:C = US, O = Let's Encrypt, CN = Let's Encrypt Authority X3
   i:O = Digital Signature Trust Co., CN = DST Root CA X3
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIFYjCCBEqgAwIBAgISAxXZeGDOdodqxT4UhxwNc/6yMA0GCSqGSIb3DQEBCwUA
MEoxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MSMwIQYDVQQD
ExpMZXQncyBFbmNyeXB0IEF1dGhvcml0eSBYMzAeFw0yMDA2MTAxNjAwMDVaFw0y
MDA5MDgxNjAwMDVaMB8xHTAbBgNVBAMTFHRlc3Quc2lsaWNvbmhpbGxzLmNvMIIB
IjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAy+pcYHNQKOCe/VqzM4lNpnh8
725ApwmNr2UlB8j6eB2dMTI3y4TO5hdOto1YmLXU+KMTjYMlPGcNM4n3aetDccVH
QVHYCPx0Bup9zhQg9kr/wTxXkK24QLKYfsY/WbavnfIG2VQjrTqx8l6sZ6zh0Dnk
MySFXXfsTpvW5eCzxzxuVPZfAIvIqx085XwlWYJ0EEkR86rI78F8+m13RJwufNz5
P6SLCWyBDeGyX86M1LYPIgw18XZHBNNuWx3x2AK5wQbIcc1/RKmVc3NgKkf5EXr9
jtaPzhPpr5vqrultt+BGlOJoqds8MDtg8rrYtEpl1o5YQzKRbSzoTgdD4/ukFwID
AQABo4ICazCCAmcwDgYDVR0PAQH/BAQDAgWgMB0GA1UdJQQWMBQGCCsGAQUFBwMB
BggrBgEFBQcDAjAMBgNVHRMBAf8EAjAAMB0GA1UdDgQWBBTEW5ZE870KtGjlAZaj
SjHRjtFcWzAfBgNVHSMEGDAWgBSoSmpjBH3duubRObemRWXv86jsoTBvBggrBgEF
BQcBAQRjMGEwLgYIKwYBBQUHMAGGImh0dHA6Ly9vY3NwLmludC14My5sZXRzZW5j
cnlwdC5vcmcwLwYIKwYBBQUHMAKGI2h0dHA6Ly9jZXJ0LmludC14My5sZXRzZW5j
cnlwdC5vcmcvMB8GA1UdEQQYMBaCFHRlc3Quc2lsaWNvbmhpbGxzLmNvMEwGA1Ud
IARFMEMwCAYGZ4EMAQIBMDcGCysGAQQBgt8TAQEBMCgwJgYIKwYBBQUHAgEWGmh0
dHA6Ly9jcHMubGV0c2VuY3J5cHQub3JnMIIBBgYKKwYBBAHWeQIEAgSB9wSB9ADy
AHcAXqdz+d9WwOe1Nkh90EngMnqRmgyEoRIShBh1loFxRVgAAAFynyyvJgAABAMA
SDBGAiEA1tdH0cgg5v0/Ricjg3sDs1xAJNdv136kYu2x1kxf3vUCIQDhC7Bwf+ox
vo5LzLsqdyI1c6gGWDM/lADggl9nHjtuOAB3ALIeBcyLos2KIE6HZvkruYolIGdr
2vpw57JJUy3vi5BeAAABcp8srxEAAAQDAEgwRgIhAOhX6wmVg7T+lvzbKYcaVf92
GHGJ1eao/UFStdc4VKFzAiEA0x+p6CU//x5J97CME+85nI9Rw++mTtKNJbm5gKcV
XdcwDQYJKoZIhvcNAQELBQADggEBAAWTVlFwPCCiKA5jP0H1jgtOumpqHkGVPo0o
xPv9k47EBh6UnU+L5W2RtJjAWcZF1U75pR3zQMXk0TSDtxI22dbQH5aYkYaKYWYU
AlXS5PdauSnKI1Fb1OB8Gn13U4z9lihyHC0ylYcoJYg5lh/cpRHwEErpJS5j0rKL
gFYgzt+P94BYROW6KBnFCLr6RLM9SfwTEX4+f2l5dqpFOMc9Im6tAdyHCOPnkkdq
GA3TAJzNDmFhf26DLfqRS6IzfBOgIleCJ/qGNc5G+hLAO8/4gT3ZkHbKwcW8LIDM
xWz91SqRjdsILDV9hLT/9G1uWlliqYSbeGWE8StA2gyWbo+X8mw=
-----END CERTIFICATE-----
subject=CN = mail.example.com

issuer=C = US, O = Let's Encrypt, CN = Let's Encrypt Authority X3

---
No client certificate CA names sent
Peer signing digest: SHA512
Peer signature type: RSA
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 3220 bytes and written 415 bytes
Verification: OK
---
New, TLSv1.2, Cipher is ECDHE-RSA-AES256-GCM-SHA384
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES256-GCM-SHA384
    Session-ID: 56FBA976FFC4E8F6C9495D8465DF4769442917E575E85405E7BFCB761764CA64
    Session-ID-ctx: 
    Master-Key: 991FD5BF3BA4284AC21534127FB7D477F17BCF39DBEA6C51F4ED70473C006BE1FBEA00F379147B240FE268ABBE39DDDA
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - e0 77 c3 48 a2 e0 91 f2-d0 d9 5f 49 f1 cb 06 3b   .w.H......_I...;
    0010 - 4a 02 e6 1b 56 5c 3b e8-42 6f 70 fd d0 bb 34 0d   J...V\;.Bop...4.
    0020 - c3 8e 7e 73 91 fd 79 2b-64 48 7d 1a 8d 49 ab f1   ..~s..y+dH}..I..
    0030 - 91 52 68 c2 d7 23 d3 53-69 42 87 db 5b d0 ce 39   .Rh..#.SiB..[..9
    0040 - 43 c9 40 08 bc 4f 5c df-54 8b 1b dd ed 3c 64 6a   C.@..O\.T....<dj
    0050 - f4 d2 b3 7b c1 b7 41 5e-3a 25 47 74 33 91 af e3   ...{..A^:%Gt3...
    0060 - a9 0f f4 05 cc 4f 6e ca-cd d8 c0 68 09 3b 09 25   .....On....h.;.%
    0070 - 82 85 fb 4b bd 61 b7 94-b6 00 ef 21 80 6a 18 8d   ...K.a.....!.j..
    0080 - 79 69 02 ff e9 bc 6c ef-0b 78 bb 85 ca d1 97 92   yi....l..x......
    0090 - bf 0f 46 69 b5 27 eb 9d-e9 bf c1 14 c1 eb f5 da   ..Fi.'..........
    00a0 - 92 48 e5 b2 c3 ac 40 b7-2a c5 43 97 17 07 65 50   .H....@.*.C...eP
    00b0 - 1f 21 75 04 e8 1b 4a 94-c2 91 6d 7c cc 37 73 cc   .!u...J...m|.7s.

    Start Time: 1591882065
    Timeout   : 7200 (sec)
    Verify return code: 0 (ok)
    Extended master secret: yes
---
* OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE LITERAL+ AUTH=PLAIN AUTH=LOGIN] Dovecot ready.
```

#### Authenticate

```
. LOGIN hello world
```

```
. OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT SORT=DISPLAY THREAD=REFERENCES THREAD=REFS THREAD=ORDEREDSUBJECT MULTIAPPEND URL-PARTIAL CATENATE UNSELECT CHILDREN NAMESPACE UIDPLUS LIST-EXTENDED I18NLEVEL=1 CONDSTORE QRESYNC ESEARCH ESORT SEARCHRES WITHIN CONTEXT=SEARCH LIST-STATUS BINARY MOVE SNIPPET=FUZZY LITERAL+ NOTIFY SPECIAL-USE] Logged in
```

#### List Folders

```
. LIST "" "*"
```

```
* LIST (\HasNoChildren \Trash) "." Trash
* LIST (\HasNoChildren \UnMarked \Sent) "." Sent
* LIST (\HasNoChildren \UnMarked \Junk) "." Junk
* LIST (\HasNoChildren \Drafts) "." Drafts
* LIST (\HasNoChildren \UnMarked) "." Archive
* LIST (\HasNoChildren) "." INBOX
. OK List completed (0.001 + 0.000 secs).
```


#### Get Inbox

```
. EXAMINE INBOX
```

```
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS ()] Read-only mailbox.
* 0 EXISTS
* 0 RECENT
* OK [UIDVALIDITY 1594507084] UIDs valid
* OK [UIDNEXT 1] Predicted next UID
* OK [HIGHESTMODSEQ 1] Highest
. OK [READ-ONLY] Examine completed (0.002 + 0.000 + 0.001 secs).
```


#### Get Drafts

```
. EXAMINE Drafts
```

```
* OK [CLOSED] Previous mailbox closed.
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS ()] Read-only mailbox.
* 1 EXISTS
* 0 RECENT
* OK [UIDVALIDITY 1594507085] UIDs valid
* OK [UIDNEXT 25] Predicted next UID
* OK [HIGHESTMODSEQ 69] Highest
. OK [READ-ONLY] Examine completed (0.001 + 0.000 secs).
```

#### Get First Draft

```
. FETCH 1 BODY[]
```

```
* 1 FETCH (BODY[] {919}
MIME-Version: 1.0
Date: Sat, 11 Jul 2020 23:18:11 +0000
Content-Type: multipart/alternative;
 boundary="--=_RainLoop_201_134599085.1594509491"
X-Mailer: RainLoop/1.13.0
From: email@example.com
Message-ID: <b41fa1b8d04f482c5c70d05cb4da791f@siliconhills.co>
Subject: draft
To: someone@example.com


----=_RainLoop_201_134599085.1594509491
Content-Type: text/plain; charset="utf-8"
Content-Transfer-Encoding: quoted-printable

I am a draft

----=_RainLoop_201_134599085.1594509491
Content-Type: text/html; charset="utf-8"
Content-Transfer-Encoding: quoted-printable

<!DOCTYPE html><html><head><meta http-equiv=3D"Content-Type" content=3D"t=
ext/html; charset=3Dutf-8" /></head><body><div data-html-editor-font-wrap=
per=3D"true" style=3D"font-family: arial, sans-serif; font-size: 13px;"><=
br>I am a draft<signature></signature></div></body></html>

----=_RainLoop_201_134599085.1594509491--
)
. OK Fetch completed (0.001 + 0.000 secs).
```
