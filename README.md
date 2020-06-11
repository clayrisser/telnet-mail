# telnet-mail

> test smtp, imap and pop3 with telnet

## SMTP

### SMTP 25

**shell command**
```sh
telnet mail.example.com 25
```

_result_
```
Trying 93.184.216.34...
Connected to mail.example.com.
Escape character is '^]'.
220 mail.example.com ESMTP Postfix (Debian)
```

**telnet command**
```sh
EHLO localhost
```

_result_
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

**telnet command**

```sh
AUTH LOGIN
```

_result_
```
334 VXNlcm5hbWU6
```

**telnet command**
```sh
aGVsbG8=
```

_result_
```
334 UGFzc3dvcmQ6
```

**telnet command**
```sh
d29ybGQ=
```

_result_
```
235 2.7.0 Authentication successful
```
