# Anchor Vault

This repository contains a Solana vault program built with the Anchor framework. The program allows users to create a personal vault, deposit SOL, withdraw funds, and close the vault to reclaim rent. It is intended as a clear example of how to use Anchor for secure, PDA-based account management on Solana.

## Contents
- `vault/programs/vault/src/lib.rs` — The main Rust program implementing the vault logic.
- `vault/tests/vault.ts` — TypeScript tests using Anchor's Mocha runner to verify program functionality.

## Usage

### Build the Program
To compile the Rust code and generate the program binary, run:
```bash
anchor build
```

### Run the Tests
To deploy the program to a local validator and execute the test suite:
```bash
anchor test
```
This will run through the main instructions: initialize, deposit, withdraw, and close.

### Deploy to Devnet (Optional)
To deploy the program to Solana devnet:
```bash
anchor deploy --provider.cluster devnet
```

## Program Overview
- **Initialize:** Creates a vault for the user (using a PDA for security and uniqueness).
- **Deposit:** Allows the user to deposit SOL into their vault.
- **Withdraw:** Enables the user to withdraw SOL, ensuring the vault remains rent-exempt.
- **Close:** Returns all remaining SOL to the user and closes the vault and state accounts.

## Technical Details
- **PDAs (Program Derived Addresses):**
  - Each user is assigned a unique `vault_state` PDA: `[b"state", user pubkey]`.
  - The vault account is also a PDA: `[b"vault", vault_state pubkey]`.
- **Accounts:**
  - `vault_state` stores bump seeds and is used to derive the vault PDA.
  - `vault` is a system account that holds the user's SOL.
- **Instructions:**
  - `initialize`: Creates both the state and vault accounts, and sets up rent exemption.
  - `deposit`: Transfers SOL from the user to their vault.
  - `withdraw`: Transfers SOL from the vault back to the user, using the correct PDA signer.
  - `close`: Transfers all remaining SOL back to the user and closes the accounts.
- **Rent Exemption:**
  - The program ensures the vault always stays rent-exempt until closed.
- **Testing:**
  - The TypeScript tests derive the same PDAs as the program and call each instruction in sequence.

## Vault Flow Diagram

```
+----------------+         Initialize         +-------------------+
|   User Wallet  |-------------------------> |  vault_state PDA  |
+----------------+                          +-------------------+
        |                                         |
        |             Initialize                  |
        +-------------------------------------->  |
        |                                         |
        |             Deposit                     |
        +--------------------------+              |
        |                          |              |
        v                          v              v
+----------------+         +-------------------+  |
|   User Wallet  |<------->|    vault PDA      |<--+
+----------------+ Withdraw/Deposit/Close      +-------------------+
```

## Project Structure

```
Q3_25_Anchor_Vault/
  vault/
    programs/
      vault/
        src/
          lib.rs         # Main Anchor program
    tests/
      vault.ts           # Anchor Mocha/TypeScript tests
    Anchor.toml
    Cargo.toml
    package.json
    ...
```

## License

This project is licensed under the MIT License.

---

MIT License

Copyright (c) 2024 Zohara
