# LayerEdge Light Node Setup Guide

This guide will walk you through the steps to set up and run your LayerEdge Light Node efficiently on both Linux and Windows (WSL).

## Prerequisites

Ensure you have the following:
- A computer with at least 4GB of RAM.
- A stable internet connection.
- Basic knowledge of the command line interface.

## Step 1: Install WSL (Windows Only)

If you're using Windows, you need to install Windows Subsystem for Linux (WSL).

1. Open PowerShell as Administrator and run the following command:
    ```sh
    wsl --install
    ```

2. Restart your computer if prompted.

3. Set up your Linux distribution (e.g., Ubuntu) from the Microsoft Store.

## Step 2: Install Required Dependencies

### Linux
1. Open your terminal.
2. Update the package list and install updates:
    ```sh
    sudo apt update && sudo apt upgrade -y
    ```

3. Install required dependencies:
    ```sh
    sudo apt install -y build-essential curl wget git
    ```

### Windows (WSL)
1. Open your WSL terminal.
2. Update the package list and install updates:
    ```sh
    sudo apt update && sudo apt upgrade -y
    ```

3. Install required dependencies:
    ```sh
    sudo apt install -y build-essential curl wget git
    ```

## Step 3: Install Go

1. Download the Go binary:
    ```sh
    wget https://golang.org/dl/go1.23.1.linux-amd64.tar.gz
    ```

2. Extract the archive and install Go:
    ```sh
    sudo tar -C /usr/local -xzf go1.23.1.linux-amd64.tar.gz
    ```

3. Add Go to your PATH:
    ```sh
    echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.profile
    source ~/.profile
    ```

4. Verify the installation:
    ```sh
    go version
    ```

## Step 4: Install Rust

1. Install Rust using rustup:
    ```sh
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    ```

2. Follow the on-screen instructions to complete the installation.

3. Verify the installation:
    ```sh
    rustc --version
    ```

## Step 5: Install Risc0 Toolchain

1. Install the Risc0 Toolchain:
    ```sh
    curl -L https://risczero.com/install | bash && rzup install
    ```

## Step 6: Clone the Light Node Repository

1. Navigate to your preferred directory:
    ```sh
    cd ~
    ```

2. Clone the repository and navigate to the directory:
    ```sh
    git clone https://github.com/Layer-Edge/light-node.git
    cd light-node
    ```

## Step 7: Configure Environment Variables

Set up the required environment variables in your terminal session or add them to a `.env` file:

```sh
GRPC_URL=grpc.testnet.layeredge.io:9090
CONTRACT_ADDR=cosmos1ufs3tlq4umljk0qfe8k5ya0x6hpavn897u2cnf9k0en9jr7qarqqt56709
ZK_PROVER_URL=https://layeredge.mintair.xyz
API_REQUEST_TIMEOUT=100
POINTS_API=https://light-node.layeredge.io
PRIVATE_KEY='cli-node-private-key'
```

> **Note**: Replace `'cli-node-private-key'` with the actual private key of the wallet address you choose to run the CLI from. Ensure that you store your private key securely and do not expose it in public repositories or logs. Ensure that the `ZK_PROVER_URL` matches the server where the Merkle service is running.

## Step 8: Start the Merkle Service

Before running the Light Node, start the Merkle service:

1. Navigate to the Merkle service directory and build the project:
    ```sh
    cd risc0-merkle-service
    cargo build && cargo run
    ```

2. Wait until the Merkle service is fully initialized before proceeding.
> **Note:** If you are using the mintair.xyz PROVER URL in the environment variables then you don't have to go through this step.

## Step 9: Build and Run the LayerEdge Light Node

In a separate terminal window, navigate to the root directory of the Light Node and execute:

1. Build the Light Node:
    ```sh
    go build
    ```

2. Run the Light Node:
    ```sh
    ./light-node
    ```

Ensure that the Light Node is running independently and correctly connected to the Merkle service.

## Connecting CLI Node with LayerEdge Dashboard

To link your CLI node with the dashboard for analytics:

1. **Fetch Points via CLI**:
    ```sh
    https://light-node.layeredge.io/api/cli-node/points/{walletAddress}
    ```
    Replace `{walletAddress}` with your actual CLI wallet address.

2. **Connect to Dashboard**:
    - Navigate to [dashboard.layeredge.io](https://dashboard.layeredge.io)
    - Connect your wallet
    - Link your CLI node’s Public Key

> **Important Notes**:
> - One CLI wallet can only link to one dashboard wallet.
> - Linking is mandatory, even if the CLI and dashboard wallets are identical.

## Dashboard Monitoring Features

- Node status (Active/Inactive)
- Points tracking & detailed analytics

## Logging & Monitoring

The node provides comprehensive logs for:

- Merkle tree discovery
- ZK proof generation and verification
- Submission status
- Performance optimizations (tree sleep state)

## Security Best Practices

- Always store keys, mnemonics, and AUTHKEY offline.
- Utilize firewall protections and secure SSH configurations.
- Regularly update nodes from official LayerEdge sources.

## Troubleshooting Common Issues

1. **Issue**: gRPC connection is inactive.
   - **Solution**: Verify gRPC connection settings.

2. **Issue**: Risc0 prover service is not running.
   - **Solution**: Restart the prover service and check logs.

3. **Issue**: Incorrect wallet or signature configurations.
   - **Solution**: Double-check wallet addresses and environment variables.

Consult detailed logs for more specific errors.

## Need Help?

Encounter issues or have questions? Join our Discord community for immediate assistance: [discord.gg/layeredge](https://discord.gg/layeredge)

## License

Licensed under the MIT License. See the LICENSE file for details.
