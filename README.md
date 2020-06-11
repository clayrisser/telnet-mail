# telnet-mail

> test smtp, imap and pop3 with telnet

## SMTP

### SMTP 25

**shell command**
```
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
```
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
```
AUTH LOGIN
```

_result_
```
334 VXNlcm5hbWU6
```

**telnet command**
```
aGVsbG8=
```

_result_
```
334 UGFzc3dvcmQ6
```

**telnet command**
```
d29ybGQ=
```

_result_
```
235 2.7.0 Authentication successful
```

**telnet command**
```
MAIL FROM: email@example.com
```

_result_
```
250 2.1.0 Ok
```

**telnet command**
```
RCPT TO: email@example.org
```

_result_
```
250 2.1.5 Ok
```

**telnet command**
```
DATA
```

_result_
```
354 End data with <CR><LF>.<CR><LF>
```

**telnet command**
```
Subject: Hello

world
.
```

_result_
```
250 2.0.0 Ok: queued as 941731C4
```

**telnet command**
```
QUIT
```
_result_
```
221 2.0.0 Bye
Connection closed by foreign host.
```
