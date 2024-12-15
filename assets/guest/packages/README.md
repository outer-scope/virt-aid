# Packages Module

`ddcutil` will be installed during the install phase. When uninstalling `virt-aid`, these packages will be uninstalled as well.

> **⚠️ Warning**  
> Do not execute `sudo $VA_DISPATCH packages/uninstall/purge` manually. This will uninstall all installed packages and `virt-aid` will fail to function.

## Hooks

`packages` subscribes to (is hooked by) the following events:

| Script/Event        | Definition                   |
| -------------       | -------------                |
| `init`              | Installs required packages.  |   
| `uninstall/purge`   | Uninstall required packages. |   
