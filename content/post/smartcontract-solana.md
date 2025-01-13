---
title: "Solana Smart Contract Tutorial"
date: 2024-02-23
description: "introduces the basics of Solana smart contracts, focusing on account creation, and understanding the roles of payers and signers."
categories: ["Programming"]
author: "adriel artiza"
---

## What is Solana?

Solana is a high-performance blockchain supporting fast transactions and low fees. Smart contracts in Solana are written in **Rust**, **C**, or **C++**, and are deployed as **programs**. Programs interact with **accounts**, which hold data and **SOL** tokens.

---

## Prerequisites

- Familiarity with Rust programming.
- Basic understanding of blockchain and smart contracts.
- Installed [Solana CLI](https://docs.solana.com/cli/install-solana-cli).

---

## Key Concepts

### 1. Accounts

- **Accounts** are the data storage units on Solana. Every account:
  - Has a unique public key.
  - Holds data that programs can read or modify.
  - Is owned by a specific program.
- Unlike Ethereum, Solana separates the program logic (smart contracts) from accounts.

### 2. Signers and Payers

- **Signers**: The entities that cryptographically sign transactions to prove authorization.
- **Payers**: The accounts that pay the transaction fee. Often, the signer is also the payer, but they can be separate.

---

## Setting Up a New Solana Account

### Step 1: Create a New Keypair

A keypair consists of a **public key** (your account address) and a **private key** (used to sign transactions).

Run the following command to create a keypair:
```bash
solana-keygen new --outfile my-keypair.json
```

This generates a keypair file (my-keypair.json) that holds your private key.

### Step 2: Fund the Account

You can fund your account using Solana’s devnet faucet:
```bash
solana airdrop 2 <your-public-key>
```
Replace <your-public-key> with the public key from your keypair.

**Writing a Simple Smart Contract**

**Step 1: Setup**

1.	Install Rust:
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

2.	Install Solana tools:

```bash
cargo install --git https://github.com/solana-labs/solana-program-library
```


**Step 2: Add Dependencies**

Update your Cargo.toml file with the following dependencies:
```rust
[dependencies]
solana-program = "1.16.6"
borsh = "0.10.3"
```
**Step 3: Write Your Program**

Replace the contents of src/lib.rs with the following:
```rust
use solana_program::{
    account_info::{next_account_info, AccountInfo},
    entrypoint,
    entrypoint::ProgramResult,
    pubkey::Pubkey,
    msg,
};

// The program's entrypoint
entrypoint!(process_instruction);

// The function that runs when the program is invoked
fn process_instruction(
    program_id: &Pubkey,      // Public key of the program
    accounts: &[AccountInfo], // Accounts involved in the transaction
    _instruction_data: &[u8], // Additional input data
) -> ProgramResult {
    msg!("Hello, Solana!");
    Ok(())
}
```

This is a minimal smart contract that logs “Hello, Solana!” whenever invoked.

**Step 4: Build the Program**

Build the program as a Solana-compatible binary:
```bash
cargo build-bpf --release
```

This generates a .so file in the target/deploy/ directory. This file is your deployable smart contract.

### Deploying the Smart Contract

**Step 1: Deploy the Program**

Use the Solana CLI to deploy the .so file:
```bash
solana program deploy target/deploy/solana_hello_world.so
```
This command returns the Program ID of your smart contract.

### Interacting with the Contract

**Step 1: Prepare a Transaction**

Write a script or use the CLI to invoke your program. For example:
```bash
solana program invoke --program-id <program-id> --signer ./my-keypair.json
```
Step 2: Observe Logs

Check the Solana blockchain logs for your program’s output:
```bash
solana logs
```
You should see the message “Hello, Solana!”.

### Understanding Payer and Signer in Practice

When invoking a smart contract:
    
    1.	The payer covers the transaction fee for execution.
	2.	The signer provides authorization to modify or interact with accounts.

For example:

	•	Signer: If your program writes to an account, that account must be a signer.
	•	Payer: The payer is the account specified when submitting the transaction.

Next Steps

	1.	Explore Solana’s documentation for advanced features.
	2.	Learn how to work with Anchor Framework, a higher-level abstraction for Solana development.

By following this guide, you’ve learned the basics of account creation, transaction signing, and deploying a simple Solana smart contract! 🎉

