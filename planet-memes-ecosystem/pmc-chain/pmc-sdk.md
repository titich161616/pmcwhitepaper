# PMC SDK

PMC SDK is designed to make it super easy to get started with Game development by enabling connection and interaction with the PMC blockchain.

Contains a huge range of classes, plugins, and example scripts for a variety of use cases.

### Use cases

This help content focuses on the following use cases.

1. Connecting to a Web3 Wallet (Memes Wallet) and Authenticating a user
2. Performing Updates to NFTs by interacting with the blockchain and calling smart functions.

#### 01 Connect Web3 Wallet

Connecting to an Ethereum i.e. Memes wallet provides a link between a wallet address and a user's game account. You have to connect your wallet via WalletConnect. There is a number of ways in which you can get a session key from your wallet:

1. Connect with QRCode using QRCodeImage component.
2. &#x20;Connect using WalletConnect.Instance.OpenMobileWallet.

#### 02 Perform Updates to NFTs

Making updates to the NFT e.g. adding a red hat to a character requires signing and sending a transaction.

1. Signing Transactions \
   All updates are transactions that must be signed via a prompt from Memes wallet.

2\. Sign message\
&#x20;      Login via WalletConnect is required to sign the message.

#### Verify the user signed message

#### Sending Transactions&#x20;

There are two ways to make update transactions. Using the GetData method and the CallMethod

1. GetData Method Use the GetData method to retrieve information from the blockchain. (<mark style="color:orange;">READ functions do NOT require mining</mark>.).
2. &#x20;Other non-standard Get functions are also available

These methods require

1. Contract Address
2. ABI

#### CallMethod&#x20;

Use the CallMethod to write new information to the blockchain. These methods utilize gas.





