# Watch Module

This module dispatches incoming `virt-aid` commands from guests by listening to the pseudo terminal devices connected to the guests. Each guest VM, which has `libvirt` hooks, spawns it's own watch process. Each watch process terminates itself when the guest VM shuts down.

The incoming command is wrapped with an XML format.

```
<virt-aid-cmd>virt-aid-module param1 param2</virt-aid-cmd>
```

> **⚠️ Warning**  
> Do not execute manually, as this would duplicate `watch process`.

## Hooks

`watch` subscribes to (is hooked by) the following events:

| Event                             | Definition                                          |
| -------------                     | -------------                                       |
| `libvirt-hooks/started-begin`     | Starts `watch`ing incoming messages from the guest. |   

## Sources and Credits

- https://stackoverflow.com/questions/44957822/using-sed-to-extract-element-content-of-an-xml-file
- https://www.google.com/search?client=firefox-b-d&q=bash+%5EM
- https://stackoverflow.com/questions/19345872/how-to-remove-a-newline-from-a-string-in-bash
- https://ravada.readthedocs.io/en/latest/docs/config_console.html
- https://ostechnix.com/how-to-enable-virsh-console-access-for-kvm-guests/
- https://serverfault.com/questions/1066243/what-is-the-use-case-of-qemu-chardev-pty
- https://libvirt.org/formatdomain.html#named-pipe
- https://wiki.qemu.org/Features/ChardevFlowControl
- https://fadeevab.com/how-to-setup-qemu-output-to-console-and-automate-using-shell-script/
- https://unix.stackexchange.com/questions/140876/retrieve-serial-port-information-of-a-libvirt-domain
