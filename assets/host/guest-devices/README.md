# Devices Module

This module sets up guest's PCI devices into config file to automate PCI passthrough.

## Usage

| Params            | Definition            |
| -------------     | -------------         |
| `guest_name`      | Guest name to setup.  |   

```
sudo $VA_DISPATCH guest-devices/setup-devices guest_name
```

## Config Files

| Config                                 | Contents                                                              |
| -------------                          | -------------                                                         |
| `[guest_name].conf`                    | The PCI ids of guest. This will look like `0000:02.00.0`.             |   
| `single-gpu-passthrough-guests.conf`   | The guests having the primary GPU.                                    |   
| `gpu-passthrough-guests.conf`          | The guests having the non-primary GPUs.                               |   


## Exposed Variables for `$VA_QUERY`

| Params            | Definition                                                            |
| -------------     | -------------                                                         |
| `device_pci_id`   | The pci id of a particular GPU. This will look like `0000:02.00.0`.   |   

```
. $VA_QUERY switch device_pci_id
```

| Variables                             | Contents                                                                                                                          |
| -------------                         | -------------                                                                                                                     |
| `guest-devices`                       | Contains the PCI devices assigned to this guest. If `guest_name` is not supplied, this variable contains nothing.                 |   
| `devices_with_virsh_format`           | Contains information from `guest-devices` but with `virsh` format, to be used by `virsh`. For example, the PCI id `0000:02.00.0` will be translated into `pci_0000_02_00_0`                                                                                                                                                          |   
| `primary_gpu_passthrough_guests`      | Contents of `single-gpu-passthrough-guests.conf`                                                                                  |   
| `gpu_passthrough_guests`              | Contents of `gpu-passthrough-guests.conf`.                                                                                        |   
| `guest_gpu_device`                    | Returns guest's GPU device. Is empty if the guest has none.                                                                       |   
| `is_gpu_passthrough_guest`            | Returns `guest_name` if the guest performs non-primary GPU passthrough. This is a convenience variable to reduce boiler codes.    |   
| `is_primary_gpu_passthrough_guest`    | Returns `guest_name` if the guest performs primary GPU passthrough. This is a convenience variable to reduce boiler codes.        |   

> **ⓘ Dependency**  
>
> `guest-devices` query depends on `switch` query.

## Helpers

| Function                       | Param    | Usage                                 | Definition                                                                    |
| -------------                  | -------- | -------------                         | -------------                                                                 |
| `get_pci_id_with_virsh_format` | `pci_id` | `get_pci_id_with_virsh_format pci_id` | Formats incoming PCI id into `virsh` readable format. E.g. `pci_0000_02_00_0` |
| `get_guest_name_by_pci_id`     | `pci_id` | `get_guest_name_by_pci_id pci_id`     | Given a PCI id, returns the guest name of that PCI id.                        |

## Hooks

`guest-devices` subscribes to (is hooked by) the following events:

| Script/Event         | Definition                                                                     |
| -------------        | -------------                                                                  |
| `guest-setup/main`   | `guest-devices` will be called directly after `guest-setup`                    |   

## Sources and Credits

- https://curtisshoward.com/post/fixing-amd-gpu-passthrough-reset-issues-in-windows/
- https://www.alexforencich.com/wiki/en/pcie/hot-reset-linux
- https://www.alexforencich.com/wiki/_export/code/en/pcie/hot-reset-linux?codeblock=0
- https://www.libvirt.org/pci-hotplug.html
- https://gitlab.freedesktop.org/drm/amd/-/issues/2709
- https://adaptivesupport.amd.com/s/article/1148199?language=en_US
- https://unix.stackexchange.com/questions/73908/how-to-reset-cycle-power-to-a-pcie-device/474378#474378
- https://superuser.com/questions/1761149/bind-unbind-one-pcie-device-disables-other
- https://askubuntu.com/questions/1393562/unbind-and-bind-driver-during-startup-20-04-3-lts
