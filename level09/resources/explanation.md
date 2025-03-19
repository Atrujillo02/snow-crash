# Snow Crash - Level 09

## Identifying the Files

Listing the directory contents, we find two files: `level09` (an executable) and `token` (an encrypted text file). 

## Understanding the Encryption

Decompiling `level09` reveals a loop that encrypts a string by modifying each character based on its position:

```c
while (true)
{
    var_120 += 1;
    int32_t i = 0xffffffff;
    int32_t edi_1 = argv[1];
    
    while (i)
    {
        bool cond:0_1 = 0 != *edi_1;
        edi_1 += 1;
        i -= 1;
        
        if (!cond:0_1)
            break;
    }
    
    if (var_120 >= ~i - 1)
        break;
    
    putchar(*(var_120 + argv[1]) + var_120);
}
```

This loop increments each character of the input string by its position index, creating an encrypted output.

## Decrypting the Token

To retrieve the original text, we reverse the encryption by subtracting the character's position from each byte in the encrypted `token` file.

### Python Script to Decrypt:

```python
with open("token", "r") as f:
    encrypted = f.read().strip()

decrypted = "".join(chr(ord(c) - i) for i, c in enumerate(encrypted))
print("Decrypted password:", decrypted)
```

Running this script reveals the password for `flag09`.

