# Illumos

Illumos x86_64 syscall bindings for [Rux](https://rux-lang.dev).

## Installation

You can install the package using Rux CLI

```sh
rux add Illumos
```

or add `Illumos` to your `Rux.toml`:

```toml
[Dependencies]
Illumos = "*"
```

## Overview

This package exposes a small Illumos syscall foundation. It does not depend on
libc; calls are backed by Rux compiler support for
`__rux_linux_syscall0` through `__rux_linux_syscall6`.

### Raw syscall wrappers

| Function | Description |
| --- | --- |
| `Syscall0` ... `Syscall6` | Invoke an Illumos x86_64 syscall with 0 to 6 arguments |
| `IsError(result)` | Returns `true` for Illumos positive errno results |
| `Errno(result)` | Converts a positive errno result to a positive errno value |

### Basic helpers

| Function | Description |
| --- | --- |
| `Read(fd, buffer, count)` | Call `read(2)` |
| `Write(fd, buffer, count)` | Call `write(2)` |
| `Close(fd)` | Call `close(2)` |
| `Exit(code)` | Call `exit(2)` |
| `GetPid()` | Call `getpid(2)` |
| `Mmap(addr, length, prot, flags, fd, offset)` | Call `mmap(2)` |
| `Munmap(addr, length)` | Call `munmap(2)` |
| `Brk(addr)` | Call `brk(2)` |
| `Nanosleep(req, rem)` | Call `nanosleep(2)` |
| `ClockGetTime(clockId, ts)` | Call `clock_gettime(2)` |

All syscall helpers return the raw Illumos result as `int64`: non-negative values
are successful results, while values from `1` through `4095` represent positive
errno (Illumos convention). This differs from Linux, which uses negative errno.

## Constants

- File descriptors: `Stdin`, `Stdout`, `Stderr`
- Syscall numbers: `SYS_READ`, `SYS_WRITE`, `SYS_CLOSE`, `SYS_MMAP`,
  `SYS_MUNMAP`, `SYS_BRK`, `SYS_GETPID`, `SYS_EXIT`, `SYS_NANOSLEEP`,
  `SYS_CLOCK_GETTIME`
- Memory flags: `PROT_READ`, `PROT_WRITE`, `PROT_EXEC`, `MAP_PRIVATE`,
  `MAP_ANONYMOUS` (= 256)
- Clock IDs: `CLOCK_MONOTONIC` (= 4), `CLOCK_REALTIME` (= 3)

## Example

```rux
import Illumos::{ Write, Stdout };

func Main() -> int {
    let msg = "hello from Illumos\n";
    let result = Write(Stdout, msg.data, msg.length);
    return result == msg.length as int64 ? 0 : 1;
}
```

## Requirements

- Illumos (OmniOS, OpenIndiana, SmartOS, etc.) x86_64
- A Rux compiler with Illumos syscall thunk support

## License

[MIT](LICENSE)
