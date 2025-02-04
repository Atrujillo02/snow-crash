# Commands

<code>
cat /etc/passwd | grep flag01
</code>

We can see that flag01 password is hashed directly at /etc/passwd file.
<code>
  flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
</code>


## John the Ripper
Using a program such as John the Ripper we can try to crack the hash.
<code>
john /etc/passwd
</code>
Result:
abcdefg


