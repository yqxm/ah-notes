# gdb

> How to start GDB?

```bash
gdb

gdb a.out

```

> How to quit GDB?

`quit`(abbr `q`) and `Ctrl-d` will quit gdb.

`Ctrl-c` does not exit gdb,but rather terminates the action of any gdb commands.

> How to use Shell Commands?

```bash
shell command
!command
```

`make` command doesn't need "shell". It's equavalent to `shell make make-args`.

> How to log output?

```bash
# enable logging
set logging enabled

# change logfile name
set logging file filename

# By default, GDB will append logfile, use this to set overwrite
set logging overwrite [on|off]

# show the current values of the logging settings
show logging

```
