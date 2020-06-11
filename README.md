# telnet-mail

> test smtp, imap and pop3 with telnet

## SMTP

### SMTP 25

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
---
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
---
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
---
```
QUIT
```
```
221 2.0.0 Bye
Connection closed by foreign host.
```
