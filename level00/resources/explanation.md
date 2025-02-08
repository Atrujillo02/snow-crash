# Snow Crash - Level 00

## Finding files

Execute the following command to locate files owned by the `flag00` user:

```bash
find / -user flag00 2>/dev/null
```

### Output:

```
/usr/sbin/john
/rofs/usr/sbin/john
```

One of the found files is `/usr/sbin/john`, which may contain useful information. Inspect it using:

```bash
cat /usr/sbin/john
```

### Output:

```
cdiiddwpgswtgt
```

## Deciphering the string

The obtained string appears to be encoded with a Caesar cipher. Applying a shift of 11 characters (ROT11):

```
cdiiddwpgswtgt -> nottoohardhere
```

The password for the `flag00` user is:

```
nottoohardhere
```

