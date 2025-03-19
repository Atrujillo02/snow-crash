# Snow Crash - Level 08

## Identifying the Files

Listing the directory contents, we find an executable (`level08`) and a file (`token`) that we do not have permission to read but can execute.

## Bypassing the Restriction

Running `level08` prompts us to enter a filename to read. If we input `token`, it denies access. However, any other file name is successfully read.

Decompiling `level08`, we find the following logic:

```c
if (strstr(input_filename, "token")) {
    printf("You may not access '%s'\n");
    exit(1);
}
```

This check prevents direct access to `token`, but does not account for symbolic links. 

## Exploiting Symbolic Links

Since the program checks the filename but not the actual file, we create a symbolic link to `token` with a different name:

```bash
ln -s token token1
```

Now, executing `level08` with `token1` as the argument allows us to read the contents of `token`:

```bash
./level08 token1
```

This outputs the password for `flag08`, allowing us to switch users:

```bash
su flag08
getflag
```

Executing `getflag` grants us the required flag.

