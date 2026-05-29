# Illumos

illumos x86_64 syscall bindings for Rux.

This package keeps the same public `Linux::{...}` surface used by existing
projects, but its implementation is backed by illumos/OmniOS syscall numbers.

## Installation

On illumos hosts, the compiler can remap `Linux` dependencies to this package
automatically.

```sh
rux add illumos
```
Add `Illumos` to your `Rux.toml`:

```toml
[Dependencies]
Illumos = "*"
```

## Requirements 
    -Illumos x86_64

## License

[MIT](LICENSE)
