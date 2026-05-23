# Corewar

Corewar is a Hive Helsinki final project based on the School 42 Corewar
assignment. The project implements both sides of the game toolchain:

- an assembler that parses Corewar champion source files and emits `.cor`
  bytecode
- a virtual machine that loads compiled champions, validates their headers and
  executable code, and runs the arena simulation
- a sample champion, `JoeTheChamp.s`, used as a simple working player

The code follows the 42 Norm style constraints used at Hive, which shaped the
project structure and implementation style.

## Repository Layout

```text
.
├── srcs/asm/                assembler implementation
├── srcs/virtualmachine/     virtual machine implementation
├── includes/                shared project headers
├── libft/                   custom C utility library and ft_printf
├── asm_tests/               assembler fixtures and comparison scripts
├── test_files_vm_team/      VM-focused champion fixtures
├── JoeTheChamp.s            sample champion source
├── Makefile
└── README.md
```

The source tree intentionally stays close to the original Hive project layout.
The test directories contain historical fixtures from development rather than a
modern portable test suite.

## Project Context

Corewar is a programming game where small assembly-like programs compete inside a
fixed-size memory arena. Each champion executes instructions such as `live`,
`ld`, `st`, `add`, `fork`, and `zjmp`; the virtual machine advances the game by
cycles, schedules processes, tracks live calls, and eliminates processes that do
not report alive within the required cycle window.

This repository was built as a team project:

- Pavel Arkharov and Marius worked on the assembler
- Ian Gaplichnik and Miro Tissari worked on the virtual machine

## Build

```sh
make
```

This builds two executables in the repository root:

- `./asm`
- `./corewar`

Cleanup targets are also available:

```sh
make clean
make fclean
make re
```

## Assembler Usage

Compile a champion source file:

```sh
./asm JoeTheChamp.s
```

The assembler writes a `.cor` file next to the input source:

```text
Writing output program to JoeTheChamp.cor
File converted successfully
```

## Virtual Machine Usage

Run one to four compiled champions:

```sh
./corewar JoeTheChamp.cor
```

Useful flags:

```sh
./corewar -dump 100 JoeTheChamp.cor
./corewar -n 1 JoeTheChamp.cor
```

- `-dump <cycle>` prints the arena memory at the selected cycle and exits
- `-n <id>` assigns a specific player id to the following champion

## Implementation Notes

The assembler performs lexical and structural validation, stores labels and
instructions, resolves label references, generates argument coding bytes where
required, and writes the Corewar binary header and instruction bytes.

The virtual machine validates each champion before loading it into the arena.
Validation includes the magic header, champion name and comment fields, declared
code size, null separators, and executable bytecode size. The game loop manages
process scheduling, instruction timing, player liveness, cycle-to-die checks,
and arena memory updates.

## Tests And Fixtures

The repository includes historical assembler and VM fixtures used during the
Hive project:

- `asm_tests/` contains source files, invalid-input cases, comparison scripts,
  and expected compiled bytecode
- `test_files_vm_team/` contains VM-focused champion fixtures

Some helper scripts compare this assembler against the original 42 reference
assembler used during development.

## License

This project is licensed under the MIT License. See `LICENSE` for details.
