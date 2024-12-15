# Stream Module

This module sends message to the host via a tty device. The `tty` device is acquired by extracting it from `dmesg`.

## Usage

| Params        | Definition                                  |
| ------------- | -------------                               |
| `cmd`         | `virt-aid` command to be executed by host.  |   

```
sudo $VA_DISPATCH stream switch zorg
```

## Exposed Variables for `$VA_QUERY`

```
. $VA_QUERY stream
```

| Variables     | Contents                                |
| ------------- | -------------                           |
| `tty`         | The `tty` device acquired from `dmesg`. |   

## Sources and Credits

- https://ravada.readthedocs.io/en/latest/docs/config_console.html
- https://ostechnix.com/how-to-enable-virsh-console-access-for-kvm-guests/
- https://stackoverflow.com/questions/40309746/sed-remove-all-of-a-line-except-matching-pattern
