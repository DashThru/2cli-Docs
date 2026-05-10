---
name: eda-2cli
author: DashThru Technology<support@dashthru.com>
description: >
  Use DashThru 2cli to start an EDA tool and control its interactive shell.
  Use when: The agent needs to start an EDA tool with an interactive shell, such as synthesis, place & route or STA tool.
  NOT for: EDA tools without an interactive shell, such as analog and digital simulators.
---

## When to Run

- The user asks the agent to start an EDA tool with an interactive shell to perform a task.
- The agent needs to invoke an EDA tool with an interactive shell for iterative debugging.
- The agent is unfamiliar with an EDA tool and wants to use tab-completion, `man` command, `-help` switch in the interactive shell to explore the functionality and usage of the EDA tool
- 2cli is properly installed (test with `2cli -version`)

## Workflow

1. Use `2cli -start <EDA_TOOL_EXEC> [<TOOL_OPTIONS>]...` to start the EDA tool through the 2cli daemon. For example, `2cli -start dashrtl -no_gui` starts the DashRTL tool.
2. Use `2cli <TOOL_COMMAND> [COMMAND_OPTIONS]...` to send a command or partial input to the interactive shell of the EDA tool. For example, sending `2cli puts hello` to a Tcl-based EDA tool prints 'hello'. It's also acceptable to combine the command and its options into the first argument of `2cli`, such as `2cli 'puts hello'`.
3. Use tab-completion to explore commands that start with a specific prefix. For example, send `2cli 'get\t'` to list all commands that begin with `get`. Use the `-help` switch to view the argument definitions of non-built-in Tcl commands, such as `2cli get_cells -help`. Use `man` command to get detailed information about commands and application variables, for example: `2cli man get_cells`. Many EDA tools also support tab-completion within the `man` command, e.g., sending `2cli man hdl\t` lists all application variables that start with `hdl`.
4. By default, 2cli always tries to send commands to the most recently launched daemon. If multiple EDA tools have been started via the 2cli daemon by the current user, the agent may use the `-pid` switch to specify which daemon to send commands to. In this case, the agent should record the PID printed after running `2cli -start <EDA_TOOL_EXEC>`, and then use `2cli -pid <PID> <TOOL_COMMAND>` to send a command to that specific daemon. For example, if `2cli -start dashrtl -no_gui` prints a PID of 1234, commands can be sent to this daemon using `2cli -pid 1234 puts hello`. The agent can also use `2cli -query` to list all active daemons for the current user.
5. If the agent does not explicitly quit the tool using commands such as `2cli exit`, the daemon will terminate the tool after 86400 seconds of inactivity by default.
6. Refer to `2cli --help` if the agent needs to know all supported switches of `2cli`.
