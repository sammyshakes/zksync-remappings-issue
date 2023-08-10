# Remappings Support for ZkSync

This repo demonstrates the differences in remappings support beteween the solc solidity compiler and the zksolc compiler.

## Prerequisite:

- Must have a version of `solc` installed
- Must have a version of `zksolc` installed
- make sure `solc` and `zksolc` executables are accessible

## Quickstart

- clone this repo and cd into `zksync-remappings-issue`
- `forge install`

```bash
git clone ...
cd zksync-remappings-issue
forge install
```

`Counter.sol` imports:

```bash
# actual path: <PROJECT-ROOT>/lib/solmate/src/tokens/ERC20.sol
import "solmate/tokens/ERC20.sol";
```

## Compile `Counter.sol` with `solc` and `zksolc` for comparison:

## `Solc`

### `solc` without providing remappings:

```bash
solc src/Counter.sol --output-dir ./remap-output/ --combined-json "bin"
```

Output:

```bash
Error: Source "solmate/tokens/ERC20.sol" not found: File not found. Searched the following locations: "".
 --> src/Counter.sol:4:1:
  |
4 | import "solmate/tokens/ERC20.sol";
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

### `solc` with remappings gives a positive result:

remapping provided: `solmate/=lib/solmate/src/`

```bash
solc solmate/=lib/solmate/src/ src/Counter.sol --output-dir ./remap-output/ --combined-json "bin" --overwrite
```

Output:

```bash
Compiler run successful. Artifact(s) can be found in directory "./remap-output/".
```

## `zkSolc`

### `zksolc` without providing remappings:

```bash
 zksolc-linux-amd64-musl-v1.3.11 src/Counter.sol --combined-json "bin" --output-dir ./remap-output/  --overwrite
```

Output:

```bash
Error: Source "solmate/tokens/ERC20.sol" not found: File not found. Searched the following locations: "".
 --> src/Counter.sol:4:1:
  |
4 | import "solmate/tokens/ERC20.sol";
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

### `zksolc` with remappings:

```bash
zksolc-linux-amd64-musl-v1.3.11 solmate/=lib/solmate/src/ src/Counter.sol --output-dir out/remap-output
```

```bash
No such file or directory (os error 2)
```

### Sanity check: `zksolc` with `Counter.sol` adjusted imports:

Change `Counter.sol` imports:

```bash
import "lib/solmate/src/tokens/ERC20.sol";
```

run zksolc compiler:

```bash
zksolc-linux-amd64-musl-v1.3.11 src/Counter.sol --combined-json "bin" --output-dir ./remap-output/  --overwrite
```

Result, Succesful Compilation.
