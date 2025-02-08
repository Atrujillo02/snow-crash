# Snow Crash - Level 01

## Finding user information

Execute the following command to find information about `flag01`:

```bash
cat /etc/passwd | grep flag01
```

### Output:

```
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
```

The password for `flag01` is stored as a hash directly in the `/etc/passwd` file.

## Cracking the hash

Using a tool like John the Ripper, we can attempt to crack the hash:

```bash
john /etc/passwd
```

### Output:

```
abcdefg
```

The password for the `flag01` user is:

```
abcdefg
```

