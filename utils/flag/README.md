# Flag Module

This is a generic flagging utility to mark an event. A module can flag that an event is happening. Other modules can use this flag to execute some commands. If called, `flag` creates a file using the params as the name. When queried, `flag` tells if the file exists or not. Flagged event should be removed after some period to mark that an event ends.

## Usage

| Params        | Definition                            |
| ------------- | -------------                         |
| `param1`      | Main flagged event. Non space string. |   

```
$VA_DISPATCH flag param1
```

## Exposed Variables for `$VA_QUERY`

| Params        | Definition                            |
| ------------- | -------------                         |
| `param1`      | Main flagged event. Non space string. |   

```
. $VA_QUERY flag param1
```

| Variables          | Contents                                                                         |
| -------------      | -------------                                                                    |
| `is_flagged`       | The path to the created file. If empty, the event doesn't happen.                |   
