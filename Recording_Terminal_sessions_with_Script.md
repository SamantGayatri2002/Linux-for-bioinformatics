# Recording a Terminal Session in Ubuntu with `script`

## Overview

When working in Ubuntu/Linux, it is often useful to keep a complete record of what happened in the terminal.

Ubuntu provides a built-in command called `script` that can record an interactive terminal session, including the commands entered and the output displayed on the screen.

This can be useful for:

- Documenting command-line workflows
- Keeping a personal work log
- Troubleshooting errors
- Recording software installation or configuration steps
- Reviewing commands and their outputs later
- Supporting reproducibility of computational analyses


## What is `script`?

`script` is a Linux command that records a terminal session.

Instead of manually copying commands and outputs from the terminal, you can start `script` and continue working normally.

For example:

```bash
script terminal_log.txt
````

After running this command, you may see:

```text
Script started, output log file is 'terminal_log.txt'.
```

From this point onward, the terminal session is recorded in `terminal_log.txt`.

You can then run commands normally:

```bash
pwd
```

```bash
ls
```

```bash
python analysis.py
```

The commands and the text displayed by them are captured in the log.

---

## Basic Workflow

The general workflow is:

```text
Start recording
      ↓
Run commands normally
      ↓
Commands and terminal output are recorded
      ↓
Finish the work
      ↓
Exit the recording
      ↓
Terminal log is saved
```

### 1. Start recording

Run:

```bash
script terminal_log.txt
```

### 2. Work normally

For example:

```bash
pwd
ls
echo "Starting analysis"
python analysis.py
```

There is no special syntax required for the commands being recorded. You simply continue using the terminal normally.

### 3. Stop recording

When finished, type:

```bash
exit
```

You should see something similar to:

```text
Script done.
```

The recorded session will now be available in:

```text
terminal_log.txt
```

---

## Example

Suppose the following commands are executed after starting `script`:

```bash
script terminal_log.txt

pwd
echo "Hello"
ls
```

The terminal might display:

```text
/home/user/project
Hello
analysis.py
data
results
README.md
```

The terminal activity is recorded in:

```text
terminal_log.txt
```

The exact contents can depend on the terminal and commands used, but the log provides a record of the session rather than requiring the user to manually copy each command and its output.

---

## Using a Meaningful Filename

Instead of using a generic filename such as:

```bash
script terminal_log.txt
```

you can use a descriptive name:

```bash
script analysis_terminal_log.txt
```

or:

```bash
script project_worklog.txt
```

This is especially useful when maintaining multiple logs.

For example:

```text
project/
├── analysis_terminal_log.txt
├── analysis.py
├── data/
└── results/
```

---

## Why Not Just Use `>`?

A common way to save command output is output redirection:

```bash
python analysis.py > output.txt
```

This saves the standard output produced by that particular command.

However, it does not provide the same type of record as an interactive terminal session.

For example:

```bash
python analysis.py > output.txt
```

primarily captures the command's output.

By contrast:

```bash
script terminal_log.txt
```

is intended to record the terminal session while you continue working interactively.

### Comparison

| Method                    | Saves command output | Records interactive session |
| ------------------------- | -------------------: | --------------------------: |
| `command > output.txt`    |                  Yes |                          No |
| `command &> output.txt`   |                  Yes |                          No |
| `script terminal_log.txt` |                  Yes |                         Yes |

Therefore, `script` is particularly useful when you want a record of the overall terminal session.

---

## Recording a New Bash Session

You can also start a new Bash session specifically for recording:

```bash
script -c "bash" terminal_log.txt
```

You can then work normally:

```bash
pwd
ls
python analysis.py
```

When finished:

```bash
exit
```

This exits the recorded Bash session.

For most simple use cases, however, the following is sufficient:

```bash
script terminal_log.txt
```

---

## Viewing the Log

After finishing the recording, you can inspect the file using:

```bash
cat terminal_log.txt
```

For longer logs, it is usually more convenient to use:

```bash
less terminal_log.txt
```

You can also check the file size with:

```bash
ls -lh terminal_log.txt
```

---

## Important Considerations

### 1. Start recording before the work you want to document

Commands executed before starting `script` will not be included in that recording.

Therefore, use:

```bash
script terminal_log.txt
```

before starting the workflow you want to document.

---

### 2. The log is a terminal-session record

The resulting file should not necessarily be considered a clean list of commands.

It may contain terminal output, prompts, messages, and other terminal-related information.

If the goal is to create a polished tutorial or README, the recorded log may need to be cleaned and reformatted afterward.

---

### 3. Be careful with sensitive information

A terminal recording can contain information displayed during the session.

Do **not** knowingly record or publish sensitive information such as:

* Passwords
* API keys
* Access tokens
* Private keys
* Personal information
* Confidential data
* Internal credentials

This is particularly important when committing terminal logs to a public GitHub repository.

---

## A Practical Example

A generic computational workflow might look like this:

```bash
cd ~/my_project

script analysis_terminal_log.txt

pwd
ls -lh

python preprocess.py

python analysis.py

ls -lh results/

exit
```

After the session ends, the project may contain:

```text
my_project/
├── analysis_terminal_log.txt
├── preprocess.py
├── analysis.py
├── data/
└── results/
```

The `analysis_terminal_log.txt` file provides a record of the terminal session used during the analysis.

---

## Summary

The `script` command is a convenient built-in Linux utility for recording terminal sessions.

The basic usage is:

```bash
script terminal_log.txt
```

Perform the required work normally:

```bash
command_1
command_2
command_3
```

Then stop the recording:

```bash
exit
```

The resulting file can be retained as a personal work log, troubleshooting record, or supporting documentation for a computational workflow.

### Quick Reference

| Action                        | Command                             |
| ----------------------------- | ----------------------------------- |
| Start recording               | `script terminal_log.txt`           |
| Work normally                 | Run commands as usual               |
| Stop recording                | `exit`                              |
| View the log                  | `less terminal_log.txt`             |
| Display the log               | `cat terminal_log.txt`              |
| Check file size               | `ls -lh terminal_log.txt`           |
| Start a recorded Bash session | `script -c "bash" terminal_log.txt` |

> **Key idea:** `script` lets you start a terminal recording once and then work normally, while preserving a record of the session for later reference.


