---
title: GDB Debugging Guide
description: Learn how to use GDB to debug C programs and step through assembly code — setup, breakpoints, inspection, TUI mode, and practical workflows.
category: gdb-debugging
order: 7
---

## Overview

GDB (GNU Debugger) is the standard debugger for C, C++, and assembly on Linux. It lets you pause execution, inspect variables and memory, step through code line by line (or instruction by instruction), and examine the call stack. Whether you're tracking down a segfault or studying how your C code compiles to assembly, GDB is the tool.

## Installing GDB

```bash
# Install GDB
sudo apt install gdb

# Verify installation
gdb --version
```

> **Tip:** For a better TUI experience, also install `gdb-multiarch` if you're debugging cross-compiled binaries:
> `sudo apt install gdb-multiarch`

## Compiling for Debugging

GDB works best when your binary includes debug symbols. Always compile with `-g`:

```bash
# Compile C with debug symbols
gcc -g -o myprogram myprogram.c

# Compile with debug symbols and no optimization (recommended for debugging)
gcc -g -O0 -o myprogram myprogram.c

# Compile and keep assembly info
gcc -g -O0 -S -masm=intel myprogram.c -o myprogram.s
```

| Flag | Purpose |
|------|---------|
| `-g` | Include debug symbols (source-level debugging) |
| `-O0` | Disable optimization (code maps directly to source) |
| `-ggdb` | Include GDB-specific debug info (superset of `-g`) |
| `-S` | Output assembly instead of binary |
| `-masm=intel` | Use Intel assembly syntax (easier to read) |

> **Warning:** Optimized code (`-O1`, `-O2`, `-O3`) can reorder, inline, or remove code entirely. Variables may be "optimized out" and stepping can jump unexpectedly. Always use `-O0` when debugging.

## Starting GDB

```bash
# Debug a program
gdb ./myprogram

# Debug with arguments
gdb --args ./myprogram arg1 arg2

# Attach to a running process
gdb -p <PID>

# Debug a core dump
gdb ./myprogram core

# Start in TUI mode (split source + command view)
gdb -tui ./myprogram
```

Once inside GDB, you'll see the `(gdb)` prompt. From here you control everything.

## Essential Workflow

Here's the typical debug cycle:

```
1. Set breakpoints    →  break main
2. Run the program    →  run
3. Inspect state      →  print variable, info locals
4. Step through code  →  next, step, continue
5. Repeat or quit     →  quit
```

### A Quick Example

Given this C program (`example.c`):

```c
#include <stdio.h>

int add(int a, int b) {
    int result = a + b;
    return result;
}

int main() {
    int x = 5;
    int y = 10;
    int sum = add(x, y);
    printf("Sum: %d\n", sum);
    return 0;
}
```

Debug session:

```bash
gcc -g -O0 -o example example.c
gdb ./example
```

```
(gdb) break main
Breakpoint 1 at 0x1169: file example.c, line 9.
(gdb) run
Starting program: /home/user/example

Breakpoint 1, main () at example.c:9
9           int x = 5;
(gdb) next
10          int y = 10;
(gdb) next
11          int sum = add(x, y);
(gdb) step
add (a=5, b=10) at example.c:4
4           int result = a + b;
(gdb) print a
$1 = 5
(gdb) print b
$2 = 10
(gdb) next
5           return result;
(gdb) print result
$3 = 15
(gdb) continue
Continuing.
Sum: 15
[Inferior 1 (process 12345) exited normally]
(gdb) quit
```

## Breakpoints

Breakpoints pause execution at a specific location.

```
break main              # Break at function 'main'
break 42                # Break at line 42 of current file
break example.c:42      # Break at line 42 of example.c
break add               # Break at function 'add'
break *0x401234         # Break at a specific memory address

# Conditional breakpoints
break 42 if x > 10      # Only break when condition is true
break add if a == 0     # Break in 'add' only when a is 0

# Temporary breakpoints (auto-delete after first hit)
tbreak main             # Break once at main, then remove

# Managing breakpoints
info breakpoints        # List all breakpoints (shorthand: info b)
delete 1                # Delete breakpoint #1
delete                  # Delete ALL breakpoints
disable 2               # Disable breakpoint #2 (keep it, skip it)
enable 2                # Re-enable breakpoint #2
clear main              # Remove breakpoints at 'main'
```

### Watchpoints

Watchpoints pause execution when a variable's value changes:

```
watch x                 # Break when x changes (write watchpoint)
rwatch x                # Break when x is read
awatch x                # Break when x is read OR written
info watchpoints        # List all watchpoints
```

## Stepping Through Code

| Command | Shorthand | What it does |
|---------|-----------|--------------|
| `next` | `n` | Execute next line (step OVER function calls) |
| `step` | `s` | Execute next line (step INTO function calls) |
| `finish` | `fin` | Run until current function returns |
| `continue` | `c` | Resume execution until next breakpoint |
| `until 50` | `u 50` | Run until line 50 |
| `advance main` | | Run until a specific location |
| `nexti` | `ni` | Step one assembly instruction (over calls) |
| `stepi` | `si` | Step one assembly instruction (into calls) |

> **Key difference:** `next` steps over function calls (executes them fully), while `step` enters the function. Use `next` for library calls like `printf` and `step` for your own functions.

### Reverse Debugging

GDB supports running backward through your program (requires record mode):

```
(gdb) record                    # Start recording execution
(gdb) continue                  # Run forward
(gdb) reverse-next              # Step backward (over calls)
(gdb) reverse-step              # Step backward (into calls)
(gdb) reverse-continue          # Run backward to previous breakpoint
(gdb) record stop               # Stop recording
```

## Inspecting State

### Variables and Expressions

```
print x                 # Print variable x
print/x x               # Print x in hexadecimal
print/t x               # Print x in binary
print/o x               # Print x in octal
print/d x               # Print x as signed decimal
print/u x               # Print x as unsigned decimal
print/c x               # Print x as character
print/f x               # Print x as float
print *ptr              # Dereference a pointer
print arr[3]            # Print array element
print sizeof(x)         # Print size of variable
print (int)3.14         # Cast and print

# Display — auto-print on every step
display x               # Show x after every step
display/x $rax          # Show rax register in hex after every step
undisplay 1             # Remove display #1
info display            # List active displays

# Local variables
info locals             # Print all local variables
info args               # Print function arguments
```

### Format Specifiers

| Specifier | Format |
|-----------|--------|
| `/x` | Hexadecimal |
| `/d` | Signed decimal |
| `/u` | Unsigned decimal |
| `/t` | Binary |
| `/o` | Octal |
| `/c` | Character |
| `/f` | Float |
| `/s` | String |
| `/a` | Address |

### The Call Stack

```
backtrace               # Show call stack (shorthand: bt)
backtrace full          # Show call stack with local variables
frame 2                 # Switch to frame #2
up                      # Move up one frame (toward caller)
down                    # Move down one frame (toward callee)
info frame              # Detailed info about current frame
```

## Examining Memory

The `x` (examine) command inspects raw memory:

```
x/FMT ADDRESS
```

Where FMT is `count` + `format` + `size`:

```
x/10xb &x              # 10 bytes in hex starting at &x
x/4xw &arr             # 4 words (32-bit) in hex at &arr
x/8xg $rsp             # 8 giant words (64-bit) in hex at stack pointer
x/s 0x404000           # Print string at address
x/10i $pc              # Print 10 assembly instructions at program counter
x/20xb $rsp            # Examine 20 bytes of stack
```

### Size Specifiers

| Letter | Size |
|--------|------|
| `b` | byte (8-bit) |
| `h` | halfword (16-bit) |
| `w` | word (32-bit) |
| `g` | giant word (64-bit) |

### Practical Memory Examples

```bash
# Examine a string
(gdb) x/s buffer
0x7fffffffe010: "Hello, World!"

# Examine the stack (20 bytes from stack pointer)
(gdb) x/20xb $rsp
0x7fffffffe000: 0x05 0x00 0x00 0x00 0x0a 0x00 0x00 0x00
0x7fffffffe008: 0x0f 0x00 0x00 0x00 0x00 0x00 0x00 0x00

# Examine an array of ints
(gdb) x/5dw arr
0x7fffffffe020: 1    2    3    4    5

# Examine instructions at the program counter
(gdb) x/5i $pc
0x401149 <main+4>:    sub    $0x10,%rsp
0x40114d <main+8>:    movl   $0x5,-0xc(%rbp)
0x401154 <main+15>:   movl   $0xa,-0x8(%rbp)
```

## Registers and Assembly

### Viewing Registers

```
info registers          # Show all general-purpose registers (shorthand: i r)
info all-registers      # Show ALL registers (including FP, SSE, etc.)
print $rax              # Print a specific register
print/x $rsp            # Print stack pointer in hex
print/x $rip            # Print instruction pointer in hex
```

### Common x86-64 Registers

| Register | Purpose |
|----------|---------|
| `rax` | Return value / accumulator |
| `rbx` | Callee-saved general purpose |
| `rcx` | 4th argument / counter |
| `rdx` | 3rd argument |
| `rsi` | 2nd argument |
| `rdi` | 1st argument |
| `rbp` | Base pointer (frame pointer) |
| `rsp` | Stack pointer |
| `rip` | Instruction pointer (program counter) |
| `r8-r15` | Additional general purpose |
| `eflags` | Status flags (zero, carry, sign, overflow) |

### Disassembly

```
disassemble             # Disassemble current function
disassemble main        # Disassemble function 'main'
disassemble /m main     # Disassemble with source lines interleaved
disassemble /r main     # Disassemble with raw bytes
disassemble /s main     # Disassemble with source (better than /m)
set disassembly-flavor intel    # Use Intel syntax (default is AT&T)
set disassembly-flavor att      # Switch back to AT&T syntax
```

> **Tip:** Add `set disassembly-flavor intel` to your `~/.gdbinit` file to always use Intel syntax.

### Stepping Through Assembly

Use `stepi` (`si`) and `nexti` (`ni`) to step one instruction at a time:

```
(gdb) set disassembly-flavor intel
(gdb) break main
(gdb) run
(gdb) disassemble
Dump of assembler code for function main:
   0x0000555555555149 <+0>:     push   rbp
   0x000055555555514a <+1>:     mov    rbp,rsp
=> 0x000055555555514d <+4>:     sub    rsp,0x10
   0x0000555555555151 <+8>:     mov    DWORD PTR [rbp-0xc],0x5
   ...
(gdb) stepi
(gdb) info registers rax rbp rsp
(gdb) stepi
(gdb) x/4xw $rbp-0x10          # Examine local variables on the stack
```

### Debugging a Pure Assembly Program

For NASM-style assembly:

```bash
# Assemble and link
nasm -f elf64 -g -F dwarf program.asm -o program.o
ld -o program program.o

# Or with GAS (GNU Assembler)
as -g --gstabs -o program.o program.s
ld -o program program.o

# Debug
gdb ./program
```

```
(gdb) break _start
(gdb) run
(gdb) layout asm               # Show assembly in TUI
(gdb) layout regs              # Show registers in TUI
(gdb) stepi                    # Step one instruction
(gdb) info registers           # Check register state
(gdb) x/8xb &message           # Examine data section
```

## TUI Mode (Text User Interface)

TUI mode splits your terminal into panes showing source, assembly, registers, and the command line simultaneously.

### Entering TUI Mode

```bash
# Start GDB in TUI mode
gdb -tui ./myprogram

# Or toggle TUI inside GDB
(gdb) tui enable               # Enter TUI mode
(gdb) tui disable              # Exit TUI mode
# Or press Ctrl+X, A to toggle TUI on/off
```

### TUI Layouts

```
layout src              # Source code only
layout asm              # Assembly only
layout split            # Source + Assembly (top/bottom)
layout regs             # Add register window to current layout
layout next             # Cycle through available layouts
layout prev             # Cycle backward through layouts
```

### TUI Key Bindings

| Key | Action |
|-----|--------|
| `Ctrl+X, A` | Toggle TUI mode on/off |
| `Ctrl+X, 1` | Single-window TUI layout |
| `Ctrl+X, 2` | Two-window TUI layout |
| `Ctrl+X, O` | Switch focus between TUI windows |
| `Ctrl+L` | Refresh the screen |
| `Up/Down` | Scroll source window (when focused) |
| `PgUp/PgDn` | Page through source |

> **Tip:** If the TUI display gets garbled (common with program output), press `Ctrl+L` to refresh.

### TUI Focus

```
focus cmd               # Focus the command window (type commands)
focus src               # Focus the source window (scroll with arrows)
focus asm               # Focus the assembly window
focus regs              # Focus the register window
focus next              # Cycle focus to next window
```

## GDB Configuration (`~/.gdbinit`)

Create a `~/.gdbinit` file to set preferences that load on every GDB session:

```bash
# ~/.gdbinit

# Use Intel assembly syntax
set disassembly-flavor intel

# Print prettier structures
set print pretty on

# Show array indices
set print array-index on

# Don't ask for confirmation on quit
set confirm off

# Keep command history
set history save on
set history size 10000
set history filename ~/.gdb_history

# Don't paginate output
set pagination off

# Show source on breakpoint hit
set listsize 20
```

### GDB Dashboard and Extensions

For a much richer experience, consider these GDB extensions:

```bash
# GEF (GDB Enhanced Features) — great for binary/assembly work
bash -c "$(curl -fsSL https://gef.blah.cat/sh)"

# pwndbg — another popular extension for reverse engineering
git clone https://github.com/pwndbg/pwndbg
cd pwndbg && ./setup.sh

# GDB Dashboard — clean Python-based dashboard
wget -P ~ https://git.io/.gdbinit
```

> **Note:** These extensions override `~/.gdbinit`. Use only one at a time.

## Practical Debugging Workflows

### Finding a Segfault

```bash
gcc -g -O0 -o buggy buggy.c
gdb ./buggy
```

```
(gdb) run
Program received signal SIGSEGV, Segmentation fault.
0x0000555555555172 in main () at buggy.c:8
8           *ptr = 42;
(gdb) backtrace
#0  0x0000555555555172 in main () at buggy.c:8
(gdb) print ptr
$1 = (int *) 0x0
(gdb) # ptr is NULL — found the bug!
```

### Debugging with Core Dumps

```bash
# Enable core dumps
ulimit -c unlimited

# Run the crashing program
./buggy
# Segmentation fault (core dumped)

# Debug the core dump
gdb ./buggy core
```

```
(gdb) backtrace              # See where it crashed
(gdb) frame 0                # Go to the crash frame
(gdb) info locals            # Check local variables
(gdb) print *ptr             # Examine the problem
```

### Debugging a Multithreaded Program

```
info threads                 # List all threads
thread 2                     # Switch to thread 2
thread apply all bt          # Backtrace for all threads
set scheduler-mode on        # Only run current thread when stepping
```

### Using Catchpoints

```
catch throw                  # Break on C++ throw
catch catch                  # Break on C++ catch
catch syscall write          # Break on write() system call
catch signal SIGSEGV         # Break on segfault signal
```

### Setting Breakpoints in Shared Libraries

```
set breakpoint pending on    # Allow breakpoints in not-yet-loaded libraries
break malloc                 # Break in libc's malloc
break dlopen                 # Break when loading shared libraries
```

## GDB with Make and Build Systems

```bash
# Compile and debug in one flow
make clean && make DEBUG=1
gdb ./myprogram

# Pass run arguments in GDB
(gdb) set args input.txt --verbose
(gdb) run

# Or use shell commands within GDB
(gdb) shell make
(gdb) run
```

## Quick Reference: Command Shorthand

Most GDB commands have short forms. Press **Enter** to repeat the last command.

| Full Command | Short | Notes |
|-------------|-------|-------|
| `break` | `b` | Set breakpoint |
| `run` | `r` | Start program |
| `continue` | `c` | Resume execution |
| `next` | `n` | Step over |
| `step` | `s` | Step into |
| `finish` | `fin` | Run to return |
| `print` | `p` | Print value |
| `backtrace` | `bt` | Show call stack |
| `info` | `i` | Info subcommands |
| `delete` | `d` | Delete breakpoint |
| `quit` | `q` | Exit GDB |
| `list` | `l` | Show source |
| `nexti` | `ni` | Step one instruction (over) |
| `stepi` | `si` | Step one instruction (into) |
| `examine` | `x` | Examine memory |
| `display` | `disp` | Auto-print on step |
| `undisplay` | `undisp` | Remove auto-print |
| `frame` | `f` | Select stack frame |
| `disassemble` | `disas` | Show disassembly |

> **Tip:** Pressing **Enter** with no command repeats the last command. This is especially useful with `next`, `step`, `stepi`, and `nexti` — just keep hitting Enter to step through code.
