# Packages Module

This module installs (during the init phase of the installation) the necessary packages needed by host `virt-aid`.

> **⚠️ Warning**  
> Do not execute `sudo $VA_DISPATCH packages/uninstall/purge` manually. This will uninstall all installed packages and `virt-aid` will fail to function.

## Hooks

`packages` subscribes to (is hooked by) the following events:

| Script/Event        | Definition                   |
| -------------       | -------------                |
| `init`              | Installs required packages.  |   
| `uninstall/purge`   | Uninstall required packages. |   

## Helpers

| Function/Variabl               | Param             | Usage                            | Definition                                                                            |
| -------------                  | --------          | -------------                    | -------------                                                                         |
| `remove_package`               | `packages`        | `remove_package ddcutil samba`   | A foolish way to perform cross-distro package uninstall.                              |
| `unpack_package`               | `packages`        | `unpack_package ddcutil samba`   | A foolish way to perform cross-distro package install.                                |

## Sources and Credits

- https://www.dedoimedo.com/computers/kvm-permission-denied.html
