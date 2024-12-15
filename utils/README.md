# Plugin Development

`virt-aid` is designed in a modular fashion. There should be a module that is responsible to automate device passthrough, a module that installs/uninstalls `virt-aid`, a module that switches I/O between guests and host, etc. Each module should represent a single, independent functionality. [`angl`](https://github.com/outer-scope/angl) is used as the backbone to cater to this need.

## **Default Hooks**

Using `angl`, `virt-aid` offers the following default hooks for external plugins to use:

### `init/main`

This hook is called when installing `virt-aid` for the first time, by executing `sudo /path-to/virt-aid/init`. This hook executes before `install/main` hook. The purpose of this hook is to initialize all resources needed to perform the setup. For example: installing I2C module, installing packages, modifying GRUB, etc.

### `install/main`

The hook is called when `$VA_DISPATCH install` is executed. Or, when installing `virt-aid` for the first time, this hook is called after the `init/main`, which is why it is also called the 'setup' hook. This hook is meant to perform modules' configuration setup. For example, setting up `switch` configs, etc.

### `uninstall/purge`

This hook is called when `$VA_DISPATCH uninstall` is executed. The purpose is to remove all `virt-aid` related packages, configurations, etc.

Following the `angl` tutorial, if a module/plugin is to have an install/uninstall feature, it must have the following folder structure:

```
──[some-plugin]
   ├──[install]
   |  ┗──main
   ┗──[uninstall]               
      ┗──purge 
```

### `query`

This hook is used to gather necesary variables exposed by some modules to be consumed by other modules. This hook requires the module name as a parameter as well as other additional params.

```
. $VA_QUERY module_name param
```

### `helpers`

This hook is used to gather necessary functions exposed by some modules to be consumed by other modules. Unlike `query`, `helpers` does not take any parameters since it gathers all helpers by default for convenience.

```
. $VA_HELPERS
```

## **Logging and Debugging**

`virt-aid` provides logging capabilities, which is helpful when debugging certain problems.

```
$VA_DISPATCH log "some message"
```

The log file is located at `$VA_PATH/utils/log/logs`. Additionally, one can inspect the libvirt log for the guests located at `/var/log/libvirt/qemu/guest.log`

## **Installation Assets**

`virt-aid` can be installed on the host, as well as on the guests. It is important for the guests to install `virt-aid`, to help with the KVM switch features. If this is not the use case one is after, `virt-aid` guest can be ignored. Due to the fact, plugins/modules can be installed either on host, guest, or both. For this purpose, `virt-aid` provides a helper, which determines, in which environment `virt-aid` is being installed.

```
. $VA_HELPERS

echo "$is_virtual"
```

If `$is_virtual` is `TRUE`, `virt-aid` is on guest mode.

By default, `virt-aid` packages all host modules under `virt-aid/assets/host`, whilst guest modules under `virt-aid/assets/guest`. The contents of both folders will be moved under `virt-aid/utils` during installation and will be deleted when installation completes. 

If a custom module is to be added and installed on both environment, that module would have the following structure:

```
──[some-plugin]
   ├──[install]
   |  ├──[assets]
   |  |  ├──[guest]
   |  |  |  ┗... 
   |  |  ┗──[host]
   |  |     ┗... 
   |  ┗──main
   ┗──[uninstall]               
      ┗──purge 
```

Upon installation completion, the `install/` and `init/` folders and all files underneath will be deleted. 
