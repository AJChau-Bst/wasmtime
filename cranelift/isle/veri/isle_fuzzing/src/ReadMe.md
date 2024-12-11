# Isle Rule Converter and Clif Fuzzer

## Authors
Becky Chen and Annette Chau

## Overview
This Rust project is a tool for converting Isle (Instruction Selection Language) rules into Cranelift (Clif) instructions through a fuzzing approach. The primary goal is to randomly generate Clif instruction sequences from Isle rules and test their lowering capabilities.

## Motivation
Instruction lowering from Clif to low-level languages (ARM or x86) guided by ISLE can occasionally contain errors that might lead to vulnerabilities. As evidenced by CVE-2023-26489, there's a need for systematic fuzzing to identify potential issues in instruction lowering rules.

## Code Structure
- `expr.rs`: Main file for parsing ISLE rules and generating random Clif modules
- `instructions.txt`: List of valid instructions
- `amod_unextended.isle`: Target ISLE rule for fuzzing
- `automate_script.sh`: Shell script to automate Clif generation and compilation

## Key Components

### Data Structures
- `Expr`: Enumeration representing different expression types
  - `Var`: Variables
  - `Int`: Integer literals
  - `Inst`: Instructions with arguments
  - `NotAnInst`: Non-instruction expressions
- `Ident`: Represents identifiers with position information
- `Inst`: Represents instructions with name and arguments
- `NotAnInst`: Represents non-instruction terms

### Core Functions
- `convert_pattern()`: Converts ISLE patterns into expressions
- `match_inst_or_non_inst()`: Classifies terms as instructions or non-instructions
- `to_clif_list()`: Transforms expressions into Clif instructions
- `format_output()`: Formats generated instructions into a Clif module

## Installation and Setup
1. Install Rust: [Rust Installation Guide](https://www.rust-lang.org/tools/install)
2. Clone the repository:
   ```
   git clone git@github.com:AJChau-Bst/wasmtime.git
   ```
3. Install Wasmtime (follow repository README)

## Manual Usage

### Clif Module Generation
Navigate to `wasmtime/cranelift/isle/veri/isle_fuzzing/src` and run:
```
cargo run --bin expr
```

### Testing Generated Clif
1. Copy generated code to `test.clif`
2. Run compilation:
   ```
   cargo run -- compile --target aarch64 -D {custompath}/wasmtime/cranelift/isle/veri/isle_fuzzing/src/test.clif
   ```

## Automated Fuzzing
1. Update paths in `automate_script.sh`
2. Make script executable:
   ```
   chmod +x automate_script.sh
   ```
3. Run the script:
   ```
   ./automate_script.sh
   ```

## Limitations
- Currently supports generating one Clif module at a time
- Randomization can produce invalid programs
- Works primarily on Macs with ARM chips (M1-M4)
- Occasional incompatibility with type extensions (`.i16`, `.i32`, etc.)

## Tradeoffs and Challenges
- Random generation may produce invalid Clif programs
- Limited to single ISLE rule processing
- Requires manual verification of generated code

## Future Work
- Adding more places to increase 'random' generation for testing
