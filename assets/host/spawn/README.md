# Spawn

This module spawns a given `virt-aid` command into a new terminal session.

## Usage

| Variable              | Definition                         |
| -------------         | -------------                      |
| `cmd`                 | The `virt-aid` command to spawn.   |

```
spawn switch guest
```

## Sources and Credits

- https://stackoverflow.com/questions/32910661/pretend-to-be-a-tty-in-bash-for-any-command
- https://stackoverflow.com/questions/1401002/how-to-trick-an-application-into-thinking-its-stdout-is-a-terminal-not-a-pipe
- https://unix.stackexchange.com/questions/170063/start-a-process-on-a-different-tty
- https://stackoverflow.com/questions/20338162/how-can-i-launch-a-new-process-that-is-not-a-child-of-the-original-process
- https://unix.stackexchange.com/questions/591860/bash-script-to-start-orphan-background-process
- https://stackoverflow.com/questions/1628204/how-to-run-a-command-in-background-using-ssh-and-detach-the-session
- https://unix.stackexchange.com/questions/269805/how-can-i-detach-a-process-from-a-bash-script
