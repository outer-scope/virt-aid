# GRUB Module

This module modifies the GRUB config during `virt-aid` installation to add virtualization-related parameters, e.g. `intel_iommu=on` for intel CPUs or `amd_iommu=on` for AMD CPUs. This module saves the original GRUB file as a backup. The added config will be removed when uninstalling `virt-aid`.

> **⚠️ Warning**  
> Do not execute `sudo $VA_DISPATCH grub/uninstall/purge` manually. This will cause the virtualization system to fail.

## Usage

One may want to modify the `grub` file manually. To re-initialize GRUB, execute:

```
sudo $VA_DISPATCH grub/update-grub
```

## Hooks

`grub` subscribes to (is hooked by) the following events:

| Script/Event        | Definition                                                      |
| -------------       | -------------                                                   |
| `install`           | Sets up grub with modified values. Backs up original grub file. |   
| `uninstall/purge`   | Restore original grub file.                                     |   

## Sources & Credits

- https://unix.stackexchange.com/questions/152222/what-is-the-equivalent-of-update-grub-for-rhel-fedora-and-centos-systems
