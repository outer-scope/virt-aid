# i2c Setup Module

This module creates a backup of all guest XML definitions. In case the guest name is supplied as a parameter, only that guest's xml will be backed up. These config files will be stored under `$VA_PATH/boxes`. This module will be executed during uninstallation of `virt-aid`, although can be called individually.

## Usage

| Param             | Definition                                                                                                        |
| -------------     | -------------                                                                                                     |
| `guest`           | If supplied, `virt-aid` will backup guest's xml definition. If not spplied, all guest xmls will be backed up.     |

```
sudo $VA_DISPATCH backup guest
```

## Hooks

`backup` subscribes to (is hooked by) the following events:

| Script/Event        | Definition                   |
| -------------       | -------------                |
| `uninstall/purge`   | Backs up all guests XML.     |   
