# i2c Setup Module

This module loads the `i2c-dev` module into the kernel during init phase of `virt-aid` installation. This is needed by `ddcutil` to interface with the monitor. Uninstalling `virt-aid` will unload the `i2c-dev` module.

> **⚠️ Warning**  
> Do not execute `sudo $VA_DISPATCH i2c/uninstall/purge` manually. This will cause the `ddcutil` to not work properly.

## Hooks

`i2c` subscribes to (is hooked by) the following events:

| Script/Event        | Definition                   |
| -------------       | -------------                |
| `install/main`      | Installs `i2c` module.       |   
| `uninstall/purge`   | uninstalls `i2c` module.     |   
