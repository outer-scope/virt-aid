# Stream Module

This module streams `virt-aid` commands to a given guest, utilizing the pseudo-terminal device assigned to the guest.

> **ⓘ Info**
>
> `stream` requires a console device to communicate with a guest VM. Please refer to the installation manual.

## Usage

| Params        | Definition                                          |
| ------------- | -------------                                       |
| `guest_name`  | The guest to send the `virt-aid` command.           |   
| `cmd`         | `virt-aid` command to be executed by `guest_name`.  |   

```
# The stream is used by virt-aid in the background.
# To use manually, do:
sudo $VA_DISPATCH stream zorg switch/output 0x03 
```

## Exposed Variables for `$VA_QUERY`

```
. $VA_QUERY stream guest_name
```

| Variables     | Contents                                  |
| ------------- | -------------                             |
| `chardev`     | The console device attached to the guest. |   

## Sources and Credits

- https://stackoverflow.com/questions/30958584/delete-all-lines-before-last-case-of-a-string
