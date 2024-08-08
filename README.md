# Launch Solana Program Using Candy Machine V3 and sugar

## Installation and Setup

### 1. Install Sugar
To install Sugar, run the following command:
```bash
bash <(curl -sSf https://sugar.metaplex.com/install.sh)
```

### 2. Check Sugar Version
Verify that Sugar is installed correctly by checking its version:
```bash
sugar --version
```

### 3. Create a New Wallet
Generate a new wallet using Solana Keygen:
```bash
solana-keygen new --no-bip39-passphrase --outfile ./wallet.json
```
*Note: If this command does not work, you must install the Solana CLI.*

### 4. Install Solana CLI
Install the Solana CLI by running:
```bash
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
```
Save your wallet's public key and seed phrase for later use. Private Key is stored in wallet.json Do not share file or you risk losing control of program and Dapp as this will be your deployer wallet.

*Note: You can now import the wallet to phantom you will need it to initialize the Dapp*

### 5. Configure Solana CLI to Use This Wallet
Set the Solana CLI to use the wallet you just created:
```bash
solana config set --keypair ./wallet.json
```

### 6. Configure RPC
Set the Solana RPC URL (this determines if you launch on Mainnet or Devnet):
```bash
solana config set --url YOUR_QUICKNODE_URL
```
*Note: A unique QuickNode RPC is recommended, but you can use a public RPC (it will be slower).*

### 7. Fund Your Wallet
- **Devnet**: 
    ```bash
    solana airdrop 1
    ```
- **Mainnet**: 
    Send SOL to your wallet's public key. 

*Note: Send 1 Sol you will not use it all but is recommended if by chance changes are needed quickly*

## Preparing Assets

### 1. Prepare the Asset Files
NFT files should be placed in the `./assets` folder. The project is configured to upload assets to Arweave, which will incur a cost in SOL.

*Note: Ensure that Metadata JSON files are correctly formatted, referencing the examples provided.*

### 2. Configure Project Details
Your project details are configured in `collection.json`:
- Project Name
- Description
- Ticker
- etc.

Ensure `collection.png` is the correct logo for your collection.

### 3. Edit `config.json`
Update the following fields:
- `number`: Amount of NFTs in the collection
- `symbol`: Ticker for the tokens (must match metadata JSON files)
- `sellerBasisPoints`: Royalty percentage (500 equals 5%)
- `creator`: Public key of the wallet you generated
- `startDate`: Start dates for guards (UTC time)
- `allowlist`: Update Merkle proof for each allowlist phase (OG and WL)

Use the [Merkle Root Tool](https://tools.key-strokes.com/merkle-root) to generate Merkle proofs. Use One Wallet per line for correct format.

*Note: If you update the whitelist with new wallets or change order of wallets, you must update the Merkle proof.*

### 4. Check for Errors
Validate your configuration with:
```bash
sugar validate
```
This built-in Sugar command checks everything before deployment including metadata format if you have errors correct them before moving to next steps.

## Deployment

### 1. Upload Assets
Upload your assets to Arweave:
```bash
sugar upload
```
*Note: This process may take some time depending on the number of assets. This will cost some Sol for Rent on solana*

This will correct URI paths to images no need to upload images first Sugar handles all of this.

### 2. Deploy Program
Deploy your program with:
```bash
sugar deploy
```

*Note: Save the Candymachine ID you will need it for the Dapp*

### 3. Verify Program
Verify the deployed program:
```bash
sugar verify
```

### 4. Test Mint
Test the minting process:
```bash
sugar mint
```
*Note: This will Mint 1 NFT to deployer Wallet. Do this to test everything is working as expected*

## Adding and Managing Guards

### 1. Add Guards (Mint Phases)
Add mint phases:
```bash
sugar guard add
```

### 2. Check Guards
Check the configured guards:
```bash
sugar guard show
```

### 3. Optional: Update Guards
If you need to update guards (e.g., add wallets to the whitelist or correct mint limits), follow these steps:

1. Update the config file.
2. Generate a new Merkle proof if new wallets are added to the whitelist.
3. Update the start and end dates if necessary.
4. Update mint limit if needed.

Then run:
```bash
sugar guard update
```

By following these steps, you can successfully launch and manage your Solana program using Candy Machine V3 and Sugar.

