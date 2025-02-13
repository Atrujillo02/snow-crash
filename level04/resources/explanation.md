# Snow Crash - Level 04

## Finding the Vulnerability

We find a `.pl` file, which is a Perl script. Upon inspecting it, we discover that it runs a web server listening on port `4747` for HTTP requests.

The script executes anything passed through the `X` parameter on the server with the permissions of the file owner, which in this case is `flag04`.

## Exploiting the Vulnerability

By injecting a command into the `X` parameter, we can execute arbitrary commands on the server.

### Command Used:

```bash
curl 'localhost:4747?x=$(getflag)'
```

This command sends a request to the server, injecting `getflag`, which executes with `flag04`'s permissions and retrieves the flag.

