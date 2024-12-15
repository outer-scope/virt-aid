# Watch Module

This module listens messages from the host via a tty device and executes them. The `tty` device is acquired by extracting it from `dmesg`.

The incoming command is wrapped with an XML format.

```
<virt-aid-cmd>virt-aid-module param1 param2</virt-aid-cmd>
```

> **⚠️ Warning**  
> Do not execute manually, as this would duplicate `watch process`.

## Hooks

`watch` subscribes to (is hooked by) the following events:

| Script/Event         | Definition                                                                              |
| -------------        | -------------                                                                           |
| `install/main`       | `watch` registers a `@reboot` entry on crontab to execute `watch/main` on boot.         |   
| `uninstall/purge`    | `watch` deletes the `@reboot` entry from crontab.                                       |   
