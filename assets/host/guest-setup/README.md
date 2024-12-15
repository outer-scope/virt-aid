# Guest Setup Module

This module initializes `libvirt` hooks for a guest. Subsequently, this module also calls `devices` module to setup devices for guest.

## Usage

```
sudo $VA_DISPATCH guest-setup
```

## Hooks

`guest-setup` fires the following scripts:

| Script/Event          | Definition                                                                      |
| -------------         | -------------                                                                   |
| `main`                | Fired when setting up a guest. This event will be listened by `devices`         |   
