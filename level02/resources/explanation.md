# Snow Crash - Level 02

## Transferring the file

We find a `.pcap` file, which contains network traces. To retrieve it, use the following `scp` command:

```bash
scp -P4242 level02@localhost:level02.pcap level02.pcap
```

## Analyzing the file

Using a tool like Wireshark or `tcpdump`, we can read and extract information from the `.pcap` file.

## Telnet Communication

Analyzing the capture, we see TCP packets. Using Wireshark's **Follow TCP Stream** feature, we can reconstruct the text that was sent.

The capture reveals a plaintext communication via Telnet. We can clearly see the prompts for username and password.

### Extracted password:

```
Password: 
ft_wandr...NDRel.L0L
```

In Telnet, dots (`.`) indicate backspaces, meaning the actual password is:

```
ft_waNDReL0L
```

The password for the `flag02` user is:

```
ft_waNDReL0L
```

