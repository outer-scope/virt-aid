# Switch Module

On host, this module performs KVM (Keyboard, Video, Mouse) switch between host-guest or guest-guest by utilizing `ddcutil` for switching monitor port and `virsh` for (de/a)ttaching keyboard and mouse.

## Usage

### Switch I/O

| Params        | Definition            |
| ------------- | -------------         |
| `target`      | Can be host or guest. |   

To perform both monitor port and input peripheral switch, do:

```
sudo $VA_DISPATCH switch target
```

To perform only monitor port switch, do:

```
sudo $VA_DISPATCH switch/output target
```

To perform only input peripheral switch, do:

```
sudo $VA_DISPATCH switch/input target
```

### Manual Setup

To setup both I/O switch.
```
sudo $VA_DISPATCH switch/setup-switch
```

To setup only input switch.
```
sudo $VA_DISPATCH switch/setup-switch-input
```

To setup only output switch.
```
sudo $VA_DISPATCH switch/setup-switch-output
```

## Config Files

<table>
    <tr>
        <td>
            <b>Config</b>
        </td>
        <td>
            <b>Contents</b>
        </td>
    </tr>
    <tr>
        <td>
            `[device_pci_id].conf`
        </td>
        <td>
            A list of GPU's connected `ddcutil` display numbers, input port codes, and bus numbers. <br />
            Format: `$monitor` `$default_port` `$bus_num` <br />
            <table>
                <tr>
                    <td><b>Variable</b></td>
                    <td><b>Definition</b></td>
                </tr>
                <tr>
                    <td>`$monitor`</td>
                    <td>Monitor name.</td>
                </tr>
                <tr>
                    <td>`$default_port`</td>
                    <td>Connected monitor input port.</td>
                </tr>
                <tr>
                    <td>`$bus_num`</td>
                    <td>I2C bus number.</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>
            `current`
        </td>
        <td>
            Current holder of monitor and input peripherals. Can be host or guest.
        </td>
    </tr>
    <tr>
        <td>
            `gpu-framebuffer.conf`
        </td>
        <td>
            A list of GPU and it's framebuffers.
        </td>
    </tr>
    <tr>
        <td>
            `input.conf`
        </td>
        <td>
            The product id of the keyboard and mouse. Will be used by `virsh` to redirect these devices into guests/host.
        </td>
    </tr>
    <tr>
        <td>
            `mon-[monitor_name].conf`
        </td>
        <td>
            A list of monitor's connected GPUs and their bus numbers and monitor input ports. <br />
            Format: `$gpu` `$port` `$bus_num` <br />
            <table>
                <tr>
                    <td><b>Variable</b></td>
                    <td><b>Definition</b></td>
                </tr>
                <tr>
                    <td>`$gpu`</td>
                    <td>GPU ID; E.g. 0000:02.00.0.</td>
                </tr>
                <tr>
                    <td>`$port`</td>
                    <td>Connected monitor input port to that GPU.</td>
                </tr>
                <tr>
                    <td>`$bus_num`</td>
                    <td>I2C bus number for that GPU.</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>
            `primary-gpu.conf`
        </td>
        <td>
            The PCI id of the primary GPU.
        </td>
    </tr>
    <tr>
        <td>
            `shared-displays.conf`
        </td>
        <td>
            A list of monitors, its' GPU, and their connecting ports.
        </td>
    </tr>
</table>

## Exposed Variables for `$VA_QUERY`

| Params            | Definition                                                            |
| -------------     | -------------                                                         |
| `device_pci_id`   | The pci id of a particular GPU. This will look like `0000:02.00.0`.   |   

```
. $VA_QUERY switch device_pci_id
```

| Variables          | Contents                                                                         |
| -------------      | -------------                                                                    |
| `current`          | The content of `current` config file.                                            |   
| `display_ports`    | The content of `[device_pci_id].conf`                                            |   
| `gpu_device_paths` | A list of paths of GPU config files. E.g. `$VA_PATH/utils/switch/0000:02.00.0`   |   
| `gpu_devices`      | A list of GPU devices.                                                           |   
| `gpu_fb`           | Content of `gpu-framebuffer.conf`.                                               |   
| `input_devices`    | Content of `input.conf`.                                                         |   
| `primary_gpu`      | Content of `primary-gpu.conf`.                                                   |   
| `shared_displays`  | Content of `shared-displays.conf`                                                |   

## Helpers

<table>
    <tr>
        <td><b>Functions</b></td>
        <td><b>Definition</b></td>
    </tr>
    <tr>
        <td>`extract_pci_id`</td>
        <td>Extracts PCI ID from path or string. This return something like `0000:02.00.0`.</td>
    </tr>
    <tr>
        <td>`get_active_guests`</td>
        <td>
            Returns active/running guests. <br />
            <table>
                <tr>
                    <td><b>Variable</b></td>
                    <td><b>Definition</b></td>
                </tr>
                <tr>
                    <td>`$guests`</td>
                    <td>Active guests.</td>
                </tr>
                <tr>
                    <td>`$active_guest_with_host_gpu`</td>
                    <td>Active guest having primary GPU.</td>
                </tr>
                <tr>
                    <td>`$active_guests_with_gpu`</td>
                    <td>Active guests having secondary GPUs.</td>
                </tr>
                <tr>
                    <td>`$active_guests_with_passthrough`</td>
                    <td>Combination of active guests with GPUs.</td>
                </tr>
                <tr>
                    <td>`$active_guests_no_passthrough`</td>
                    <td>Active guests without PCI passthrough.</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>`switch`</td>
        <td>
            Performs the monitor input port switch by host. <br />
            Usage: `switch $monitor $port $bus` <br />
            <br />
            <table>
                <tr>
                    <td><b>Params</b></td>
                    <td><b>Definition</b></td>
                </tr>
                <tr>
                    <td>`$monitor`</td>
                    <td>Saved monitor name/code</td>
                </tr>
                <tr>
                    <td>`$port`</td>
                    <td>monitor input port to switch to.</td>
                </tr>
                <tr>
                    <td>`$bus`</td>
                    <td>I2C bus number used for switching by `ddcutil`.</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>`switch_by_guest`</td>
        <td>
            Performs the monitor input port switch by guest. <br />
            Usage: `switch_by_guest $guest_name $monitor_name $port` <br />
            <br />
            <table>
                <tr>
                    <td><b>Params</b></td>
                    <td><b>Definition</b></td>
                </tr>
                <tr>
                    <td>`$guest_name`</td>
                    <td>The guest performing the `ddcutil`.</td>
                </tr>
                <tr>
                    <td>`$monitor_name`</td>
                    <td>The monitor the guest needs to switch.</td>
                </tr>
                <tr>
                    <td>`$port`</td>
                    <td>The monitor input port to switch to.</td>
                </tr>
            </table>
        </td>
    </tr>
</table>

## Hooks

`switch` fires the following scripts:

| Script/Event           | Definition                                                                      |
| -------------          | -------------                                                                   |
| `input`                | Fired when switching input peripherals.                                         |   
| `main`                 | Fired when called main script. E.g. `$VA_DISPATCH switch target`.               |   
| `next`                 | Fired when shutting down a guest to give I/O to next available guest/to host.   |   
| `output`               | Fired when switching monitor input port.                                        |   
| `shutdown`             | Fired when host shuts down to switch monitor back to host's input port.         |   

> **ⓘ Info**  
> All subscribers (other modules that listen to this event) will be hooked as well.

`switch` subscribes to (is hooked by) the following events:

| Event                             | Definition                                                                                                        |
| -------------                     | -------------                                                                                                     |
| `init`                            | Sets up installation files based on host/guest mode. Also downloads [external dependencies](https://gitlab.com/risingprismtv/single-gpu-passthrough). <br><br> **ⓘ Info** <br> The `modprobe -r vfio*` section of the `vfio-teardown` script is commented out to keep PCI passthrough for the next guest when shutting down a guest. System would crash otherwise.                                                                                                                                              |   
| `install`                         | Sets up config files.                                                                                             |   
| `libvirt-hooks/started-begin`     | Perform KVM switch to the guest by executing `main` when the guest starts.                                        |   
| `libvirt-hooks/release-end`       | Perform KVM switch to the next available guest or to the host when the guest shuts down.                          |   
| `watch`                           | Listens to `SIGTERM` (shutdown) signal from host shutdown to then execute `shutdown`.                             |   

## Flags

Flags registered by `switch`:

| Flag                               | Event                                                                                                  |
| -------------                      | -------------                                                                                          |
| `switch`                           | Marking a switch event.                                                                                |   
| `switch-output-by-guest success`   | Switching monitor input port by guest succeeded. Continue with the next monitor.                       |   
| `switch-output success`            | Switching monitor input port by host succeeded.                                                        |   
| `watchdog`                         | Hooked by `watch` to listen to `SIGTERM`.                                                              |   

Flags listened by `switch`:

| Flag              | Reaction                                                     |
| -------------     | -------------                                                |
| `switch`          | Exit switch. This event marks an existing switching process. |   

## Development Notes

### About KVM Switch and GPU Passthrough.

A GPU can connect to multiple monitors. Conversely, a monitor can be attached by multiple GPUs.

The following picture depicts a monitor being shared by multiple GPU.

```
        ╔━━━━━━━━━━━━━━━━━━━━━━━━━╗
        ║                         ║
        ║                         ║
        ║         Monitor         ║          ┏━━━━━━━━━━━━┓
        ║                         ║        ┏━┫  virt-aid  ┣━━━━━━━━━━━━━━━━┓
        ║                         ║        ┃ ┗━━━━━━━━━━━━┛                ┃
        ║   ┏━━━┓ ┏━━━━┓          ║        ┃              ┏━━━━━━━━━┓    ┏━┻━━━┓
        ╚━━━┫DVI┣━┫HDMI┣━━━━━━━━━━╝        ┃       ┏┄┄┄┄┄┄┃  Host   ┃┄┄┄┄┃LAN 2┃
            ┗━┳━┛ ┗━┳━━┛                   ┃       ┊      ┗━━━━━━━━━┛    ┗━┳━━━┛
              ┃     ┃                      ┃       ┊      ┏━━━━━━━━━┓      ┃
              ┃     ┃                      ┃       ┊ ┏┄┄┄┄┃ Guest 1 ┃┄┓    ┃
              ┃     ┃                      ┃       ┊ ┊    ┃ zorg    ┃ ┊    ┃
              ┃     ┃                      ┃       ┊ ┊    ┗━━━━━━━━━┛ ┊  ┏━┻━━━┓
              ┃     ┃                  ┏━━━┻━━━━━┓ ┊ ┊    ┏━━━━━━━━━┓ ┊┄┄┃LAN 1┃
              ┃     ┗━━━━━━━━━━━━━━━━━━┫ AMD GPU ┣┄┻┄┛┄┄┄┄┃ Guest 2 ┃ ┊  ┗━┳━━━┛
              ┃                        ┃     ┏━━━┛        ┃ flux    ┃┄┛    ┃
              ┃                        ┗━━━┳━┛            ┗━━━━━━━━━┛      ┃
              ┃                        ┏━━━┻━━━━━┓        ┏━━━━━━━━━┓    ┏━┻━━━┓
              ┗━━━━━━━━━━━━━━━━━━━━━━━━┫  iGPU   ┃┄┄┄┄┄┄┄┄┃ Guest 3 ┃┄┄┄┄┃WIFI ┃
                                       ┃     ┏━━━┛        ┃ debby   ┃    ┗━┳━━━┛
                                       ┗━━━┳━┛            ┗━━━━━━━━━┛      ┃
                                       ┏━━━┻━━━━━┓                         ┃
             ┏━━━━━━━━━━━━━━━━┳━━━━━━━━┃ USB Hub ┃━━━━━━━━━━━━━━━━━━━━━━━━━┛
    ┏━━━━━━━━┻━━━━━━━━━┓      ┃        ┗━━━━━━━━━┛
    ┃┏┓┏┓┏┓┏┓┏┓    ┏┓┏┓┃    ┏━╋━┓
    ┃┏┓┏┓┏┓┏┓┏┓ ┏┓ ┏┓┏┓┃    ┃   ┃
    ┃┏┓┏━━━━┓┏┓┏┓┏┓┏┓┏┓┃    ┗━━━┛
    ┗━━━━━━━━━━━━━━━━━━┛
```
The following picture depicts multiple monitors being shared by multiple GPUs.

```
  ╔━━━━━━━━━━━━━━━━━━━━━━━━━╗     ╔━━━━━━━━━━━━━━━━━━━━━━━━━╗
  ║                         ║     ║                         ║
  ║                         ║     ║                         ║
  ║         Monitor         ║     ║         Monitor         ║          ┏━━━━━━━━━━━━┓
  ║                         ║     ║                         ║        ┏━┫  virt-aid  ┣━━━━━━━━━━━━━━━━┓
  ║                         ║     ║                         ║        ┃ ┗━━━━━━━━━━━━┛                ┃
  ║   ┏━━━┓ ┏━━━━┓          ║     ║   ┏━━━┓ ┏━━━━┓          ║        ┃              ┏━━━━━━━━━┓    ┏━┻━━━┓
  ╚━━━┫DVI┣━┫HDMI┣━━━━━━━━━━╝     ╚━━━┫DVI┣━┫HDMI┣━━━━━━━━━━╝        ┃       ┏┄┄┄┄┄┄┃  Host   ┃┄┄┄┄┃LAN 2┃
      ┗━┳━┛ ┗━┳━━┛                    ┗━┳━┛ ┗━┳━━┛                   ┃       ┊      ┗━━━━━━━━━┛    ┗━┳━━━┛
        ┃     ┃                         ┃     ┃                      ┃       ┊      ┏━━━━━━━━━┓      ┃
        ┃     ┃                         ┃     ┃                      ┃       ┊ ┏┄┄┄┄┃ Guest 1 ┃┄┓    ┃
        ┃     ┃                         ┃     ┃                      ┃       ┊ ┊    ┃ zorg    ┃ ┊    ┃
        ┃     ┃                         ┃     ┃                      ┃       ┊ ┊    ┗━━━━━━━━━┛ ┊  ┏━┻━━━┓
        ┃     ┃                         ┃     ┃                  ┏━━━┻━━━━━┓ ┊ ┊    ┏━━━━━━━━━┓ ┊┄┄┃LAN 1┃
        ┃     ┃                         ┃     ┗━━━━━━━━━━━━━━━━━━┫ AMD GPU ┣┄┻┄┛┄┄┄┄┃ Guest 2 ┃ ┊  ┗━┳━━━┛
        ┃     ┗━━━━━━━━━━━━━━━━━━━━━━━━━┃━━━━━━━━━━━━━━━━━━━━━━━━┃     ┏━━━┛        ┃ flux    ┃┄┛    ┃
        ┃                               ┃                        ┗━━━┳━┛            ┗━━━━━━━━━┛      ┃
        ┃                               ┃                        ┏━━━┻━━━━━┓        ┏━━━━━━━━━┓    ┏━┻━━━┓
        ┃                               ┗━━━━━━━━━━━━━━━━━━━━━━━━┫  iGPU   ┃┄┄┄┄┄┄┄┄┃ Guest 3 ┃┄┄┄┄┃WIFI ┃
        ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃     ┏━━━┛        ┃ debby   ┃    ┗━┳━━━┛
                                                                 ┗━━━┳━┛            ┗━━━━━━━━━┛      ┃
                                                                     ┃                               ┃
                                                                     ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

> **ⓘ Info**  
> Consider the following use cases:
> - Switching to host can be done as long as the primary GPU isn't acquired by any guest.
> - Switching to `zorg` from `flux` will shutdown `flux` and turn on `zorg`. There can't exist more than one guest for one GPU.
> - Switching to `debby` from `zorg` will execute the KVM switch capability normally without turning off/on `debby`/`zorg`.
> - KVM switching to `debby` from `zorg` or `flux` (the host no longer acquires the primary GPU) will be done by either `zorg` or `flux`. `virt-aid` will broadcast `switch debby` command via `stream`.
> - If `flux` is turned off, `virt-aid` will switch to either `zorg` or `debby`. If none of them are active, all resources are sent back to the host.

If the GPU of the target monitor switch is not available (acquired by the guest,) `virt-aid` host will try to switch using other available I2C bus(es), owned by other GPU(s), also connected to that monitor. If this also fails, pass the switching to available guests, acquiring the GPUs, connected to that particular monitor. See below illustration:


```
        ╔━━━━━━━━━━━━━━━━━━━━━━━━━╗
        ║                         ║
        ║                         ║
        ║         Monitor         ║          ┏━━━━━━━━━━━━┓
        ║                         ║        ┏━┫  virt-aid  ┣━━━━━━━━━━━━━━━━┓
        ║                         ║        ┃ ┗━━━━━━━━━━━━┛                ┃
        ║   ┏━━━┓ ┏━━━━┓          ║        ┃              ┏━━━━━━━━━┓    ┏━┻━━━┓
        ╚━━━┫DVI┣━┫HDMI┣━━━━━━━━━━╝        ┃       ┏┄┄┄┄┄┄┃  Host   ┃┄┄┄┄┃LAN 2┃
            ┗━┳━┛ ┗━┳━━┛                   ┃       ┊      ┗━━━━━━━━━┛    ┗━┳━━━┛
              ┃     ┃                      ┃       ┊      ┏━━━━━━━━━┓      ┃
              ┃     ┃                      ┃       ┊ ┏┄┄┄┄┃ Guest 1 ┃┄┓    ┃
              ┃     ┃                      ┃       ┊ ┊    ┃ zorg    ┃ ┊    ┃
              ┃     ┃                      ┃       ┊ ┊    ┗━━━━━━━━━┛ ┊  ┏━┻━━━┓
              ┃     ┃                  ┏━━━┻━━━━━┓ ┊ ┊    ┏━━━━━━━━━┓ ┊┄┄┃LAN 1┃
              ┃     ┗━━━━━━━━━━━━━━━━━━┫ AMD GPU ┣┄┻┄┛┄┄┄┄┃ Guest 2 ┃ ┊  ┗━┳━━━┛
              ┃                        ┃     ┏━━━┛        ┃ flux    ┃┄┛    ┃
              ┃                        ┗━━━┳━┛            ┗━━━━━━━━━┛      ┃
              ┃                        ┏━━━┻━━━━━┓        ┏━━━━━━━━━┓    ┏━┻━━━┓
              ┗━━━━━━━━━━━━━━━━━━━━━━━━┫  iGPU   ┃┄┄┄┄┄┄┄┄┃ Guest 3 ┃┄┄┄┄┃WIFI ┃
                                       ┃     ┏━━━┛        ┃ debby   ┃    ┗━┳━━━┛
                                       ┗━━━┳━┛            ┗━━━━━━━━━┛      ┃
                                       ┏━━━┻━━━━━┓                         ┃
             ┏━━━━━━━━━━━━━━━━┳━━━━━━━━┃ USB Hub ┃━━━━━━━━━━━━━━━━━━━━━━━━━┛
    ┏━━━━━━━━┻━━━━━━━━━┓      ┃        ┗━━━━━━━━━┛
    ┃┏┓┏┓┏┓┏┓┏┓    ┏┓┏┓┃    ┏━╋━┓
    ┃┏┓┏┓┏┓┏┓┏┓ ┏┓ ┏┓┏┓┃    ┃   ┃
    ┃┏┓┏━━━━┓┏┓┏┓┏┓┏┓┏┓┃    ┗━━━┛
    ┗━━━━━━━━━━━━━━━━━━┛
```

Given that:
- `debby` is running, switching to DVI port will be done by host `ddcutil` with AMD GPU I2C bus.
- `zorg` is running, switching to DVI port will be done by host or `zorg`'s `ddcutil` with their respective I2C buses.
- both `zorg`/`flux` and `debby` are running, switching to DVI port will be done by either `zorg`'s or `debby`'s `ddcutil` with their respective I2C buses.

> **ⓘ Info**  
> Note that each guest will assign a different I2C bus to it's GPU but the monitor input port will stay the same.

In order to pass switch command to the guests, `switch` utilizes `stream` to send `virt-aid` commands. `stream` module utilizes console device to communicate with guests.

> **ⓘ Why not using `qemu-guest-agent`?**  
> On some distros, executing `ddcutil` on guest using `qemu-guest-agent` fails due to privilege restriction.

### On Host-boot

During host boot, especially when there are multiple autostart guests with GPU passthrough, a race condition occurs due to these guests trying to perform KVM switch in a relatively short moment. To avoid this, `switch` applies an early-bird-gets-the-worm strategy; the first guest that autostarts gets the privilege to lock the rest of the guests from `switch`.

### On Host-shutdown

During host shutdown, a race condition also occurs due to multiple guests with GPU passthrough trying to give back I/O resources to the host. To avoid this, `switch` locks all guests from performing switch, and does only the monitor input port switching manually. The input peripherals switch will be automatically done by virsh. If the host has no I2C devices left to perform the monitor input switch, `switch` assigns the running guest having the primary GPU to do the switch instead.

## Sources and Credits

- https://www.linuxquestions.org/questions/programming-9/bash-combine-arrays-and-delete-duplicates-882286/
- https://unix.stackexchange.com/questions/658655/why-doesnt-uniq-unique-remove-all-duplicate-lines
- https://www.baeldung.com/linux/systemd-run-script-before-shutdown
- https://stackoverflow.com/questions/12744031/how-to-change-values-of-bash-array-elements-without-loop
- https://unix.stackexchange.com/questions/20458/how-to-use-dev-fb0-as-a-console-from-userspace-or-output-text-to-it
- https://seenaburns.com/2018/04/04/writing-to-the-framebuffer/
- https://unix.stackexchange.com/questions/192206/is-it-possible-to-access-to-the-framebuffer-in-order-to-put-a-pixel-on-the-scree
- https://raspberrypi.stackexchange.com/questions/28398/recover-console-text-output-after-using-fbi-app-to-display-image-on-dev-fb0
- https://collectingwisdom.com/grep-find-line-number-of-match/#:~:text=Often%20you%20may%20want%20to,%2Dn%20%22Forward%22%20athlete_data.
- https://www.unix.com/shell-programming-and-scripting/129007-display-string-line-number.html
