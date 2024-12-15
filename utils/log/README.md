# Log Module

This module logs events.

## Usage

| Param             | Definition                    |
| -------------     | -------------                 |
| `message`         | The message to be logged.     |

```
sudo $VA_DISPATCH backup guest
```

## Exposed Variables for `$VA_QUERY`

```
. $VA_QUERY log
```

| Variables                       | Contents                                                                                              |
| -------------                   | -------------                                                                                         |
| `logfile`                       | The location of the log file. Modules may use this file to directly write to/read from it.            |   

## Helpers

| Functions          | Contents                                                                         |
| -------------      | -------------                                                                    |
| `log`              | Logs incoming message. Will be used by `main`.                                   |   
