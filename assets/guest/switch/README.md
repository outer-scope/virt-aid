# Switch Module

This module sends switch command to the host via `stream`. If receiving `switch/output` command from the host, the guest will perform monitor input source switch using `ddcutil` locally.

## Usage

```
sudo $VA_DISPATCH switch target
```
