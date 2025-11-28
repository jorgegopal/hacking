# wfuzz

```
wfuzz -c -t 20 -w  <wordlist> -hc <codigo estado> -H "Host Fuzz.x.com" <url> 
```

-c colorines -w wordlist -hc hide code -sc show code Fuzz donde queramos meter la peticion -range1-2000: para indicar el rango del fuzzeo hace una comprobacion con esos numeros -X para realizar un metodo http personalizado
