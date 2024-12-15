# Libvirt Hooks Module

This module sets up the necessary hook scripts to be used to perform KVM switching. The libvirt hook main script is inspired from https://raw.githubusercontent.com/PassthroughPOST/VFIO-Tools/master/libvirt_hooks/qemu and `virt-aid` prepares it under `/etc/libvirt/hooks`. 

This module also downloads `vfio-startup` and `vfio-teardown` scripts from https://gitlab.com/risingprismtv/single-gpu-passthrough/-/tree/master?ref_type=heads. These scripts at(de)taches desktop sessions from primary GPU.

When uninstalling `virt-aid`, the `backup` module will be dispatched, and all contents under `/etc/libvirt/hooks` will be backed up and removed.

> **⚠️ Warning**  
> Do not execute `sudo $VA_DISPATCH libvirt-hooks/uninstall/purge` manually. This will cause the libvirt hooks to fail, and all guest definitions to disappear, although backed up.

## Hooks

`libvirt-hooks` subscribes to (is hooked by) the following events:

| Script/Event        | Definition                                                                                                          |
| -------------       | -------------                                                                                                       |
| `install`           | Downloads libvirt hook script and single GPU passthrough scripts.                                                   |   
| `uninstall/purge`   | Calls `backup`. Removes content of `/etc/libvirt/hooks`.                                                            |   

## Development Notes

After countless observations and trial-and-errors, `virt-aid` prepares the following libvirt hook scripts:

### `qemu`

Placed under `/etc/libvirt/hooks`, this script listens to fired events and calls appropriate libvirt-hook scripts. This script also checks if I/O peripherals are intact to avoid partially connected I/O. If not, guest cannot start.

### `prepare-begin`

This script is first to be called, whenever a guest is started. This script:
- logs out all active X sessions if `startx` is running and if the guest passes through primary GPU,
- executes `vfio-startup` script that
    - detaches GPU from active desktop sessions in case of primary GPU passthrough, and
    - loads necessary modules, e.g. `vfio`
- switches I/O to the guest.

### `started-begin`

This script is called after `prepare-begin` and:
- executes `watch` for the guest that watches incoming `virt-aid` commands from it.

### `release-end`

Executed whenever a guest is shut down, it:
- re-attaches passed through devices onto the host,
- executes `vfio-teardown` that re-activates desktop sessions, and
- switches I/O to next available guest/to host.

## Sources and Credits

- https://gitlab.com/risingprismtv/single-gpu-passthrough/-/tree/master?ref_type=heads
- https://github.com/joeknock90/Single-GPU-Passthrough
- https://github.com/QaidVoid/Complete-Single-GPU-Passthrough
- https://www.cyberciti.biz/faq/linux-command-log-off-all-existing-sessions-accounts/
- https://wiki.archlinux.org/title/xinit
- https://www.baeldung.com/linux/kill-terminal
- https://passthroughpo.st/simple-per-vm-libvirt-hooks-with-the-vfio-tools-hook-helper/
- https://raw.githubusercontent.com/PassthroughPOST/VFIO-Tools/master/libvirt_hooks/qemu.
- https://ostechnix.com/solved-cannot-access-storage-file-permission-denied-error-in-kvm-libvirt/
