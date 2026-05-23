# Corewar 👾

**A Hive Helsinki Final Project (Algorithm Branch)**

Corewar is a competitive programming game based on the [**School 42 Corewar project**](docs/corewar.en.pdf). Small "champions" (programs written in a custom assembly language) battle for control of a virtual memory arena. This repository contains a full implementation of the toolchain, including a bytecode assembler and a multi-process virtual machine.

This project was built following the strict [**School 42 Norm**](docs/en.norm.pdf) coding standards, emphasizing memory efficiency, robust error handling, and modular C architecture.

---

## 🏗️ Project Architecture

The system is divided into three primary components:

### 1. The Assembler (`asm`) 🛠️
Translates human-readable `.s` assembly files into binary `.cor` bytecode.
- **Lexical Analysis:** Robust parsing of instructions, registers, and labels.
- **Bytecode Generation:** Handles argument coding bytes (ACB), direct/indirect values, and big-endian conversion.
- **Validation:** Ensures champions adhere to memory limits and structural constraints.

### 2. The Virtual Machine (`corewar`) 🖥️
The execution environment where champions compete in a circular 4096-byte arena.
- **Multi-Process Scheduling:** Manages thousands of concurrent processes with independent program counters.
- **Instruction Cycle Timing:** Each of the 16 opcodes has a specific cycle cost, requiring precise timing management.
- **Memory Isolation:** Strict validation of memory access to prevent simulation crashes.
- **Liveness Tracking:** A "Cycle to Die" mechanism that periodically eliminates processes and players who fail to report liveness.

### 3. The Champion (`Honky Joe`) 🥊
A sample player provided to demonstrate the system's capabilities.
- While not a world-class fighter, Joe serves as a reference for a valid, working champion.

---

## 🚀 Getting Started

### Prerequisites
- A C compiler (GCC/Clang)
- `make`

### Installation
```bash
git clone https://github.com/Sickology101/Corewar.git
cd Corewar
make
```
This will produce two executables: `asm` and `corewar`.

### Usage

**Step 1: Compile your champion**
```bash
./asm JoeTheChamp.s
```

**Step 2: Start the battle**
```bash
./corewar -dump 1500 champion1.cor champion2.cor
```

**Available Flags:**
- `-dump <nbr>`: Dumps memory to stdout after `<nbr>` cycles and exits.
- `-n <id>`: Assigns a specific ID to the following player.
- `-v <level>`: (If implemented) Verbosity levels for simulation tracing.

---

## 🥊 The Legend of Honky Joe

<img align="right" width="200" alt="JoeTheChamp" src="https://user-images.githubusercontent.com/90178358/219705436-d2724a41-c64e-4725-a2aa-780cd7087f5d.jpeg#right">

The Champion didn't need to be a grandmaster for this project—just a fellow who works and stays alive for a while. After building the Assembler and VM, we realized the moves were already familiar, so we created **Honky Joe**.

Honky Joe was born in Louisiana and Hungary, and the doctors knew from the beginning he was going to be "honky." Lacking traditional hand-eye coordination, Joe often slaps other champions by accident. Because Joe is such a nice fellow, he frequently lets others beat his ass.

He might not be the greatest fighter, but he is undeniably the **honkiest**.

---

## 🛠️ Implementation Details

### Validation Engine
The VM performs rigorous validation on every `.cor` file:
- **Magic Header:** Verification of the Corewar magic number.
- **Metadata:** Parsing and size-checking of champion names and comments.
- **Null Separators:** Ensuring proper binary alignment.
- **Bytecode Integrity:** Validating executable code size against the header declaration.

### The Game Loop
The simulation follows a strict cycle-based clock:
1. **Fetch:** Read next opcode for each process.
2. **Wait:** Pause execution based on the opcode's cycle cost.
3. **Execute:** Perform the operation (e.g., `fork`, `live`, `zjmp`, `st`) if the argument types are valid.
4. **Prune:** Every `CYCLE_TO_DIE` cycles, the VM checks for liveness and decreases the check interval if necessary.

---

## 👥 The Team

This project was a collaborative effort by:
- **Assembler Team:** [Pavel Arkharov](https://github.com/pavel-arkharov) & [Marius](https://github.com/Sickology101)
- **VM Team:** [Ian Gaplichnik](https://github.com/IanGaplichnik) & [Miro Tissari](https://github.com/MiroTissari)

---

## 📜 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

*So long, and thanks for all the fish!* 🐬📚
