# fingerprinting  

Fingerprintjs2 JavaScript library.  

On our server /var/www/html  

`sudo wget https://github.com/Valve/fingerprintjs2/archive/master.zip`  

`sudo unzip master.zip`  

`sudo mv fingerprintjs2-master/ fp`  

To js.php  

```
<!doctype html>
<html>
<head>
  <title>Blank Page</title>
</head>
<body>
  <h1>You have been given the finger!</h1>
  <script src="fingerprint2.js"></script>
  <script>
      var d1 = new Date();
      var options = {};
      Fingerprint2.get(options, function (components) {
        var values = components.map(function (component) { return component.value })
        var murmur = Fingerprint2.x64hash128(values.join(''), 31)
	      var clientfp = "Client browser fingerprint: " + murmur + "\n\n";
        var d2 = new Date();
        var timeString = "Time to calculate fingerprint: " + (d2 - d1) + "ms\n\n";
        var details = "Detailed information: \n";
        if(typeof window.console !== "undefined") {
          for (var index in components) {
            var obj = components[index];
            var value = obj.value;
	          if (value !== null) {
              var line = obj.key + " = " + value.toString().substr(0, 150);
              details += line + "\n";
	          }
          }
        }
        var xmlhttp = new XMLHttpRequest();
        xmlhttp.open("POST", "/fp/js.php");
        xmlhttp.setRequestHeader("Content-Type", "application/txt");
        xmlhttp.send(clientfp + timeString + details);
      });
  </script>
</body>
</html>
```  

php that processes the POST request.  

```
<?php
$data = "Client IP Address: " . $_SERVER['REMOTE_ADDR'] . "\n";
$data .= file_get_contents('php://input');
$data .= "---------------------------------\n\n";
file_put_contents('/var/www/html/fp/fingerprint.txt', print_r($data, true), FILE_APPEND | LOCK_EX);
?>
```  

Allow user www-data to write to fp directory.  

`sudo chown www-data:www-data fp`  

