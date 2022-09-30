# General Security Principles

## User Master Password

Used to decrypt local data containing the user data.

- Never stored in any server, nor any form of its derivatives.
- Never stored locally until the user enables “Remember”.
- Never transmitted over the internet.
- Master Password verification is delayed by PBKDF2-SHA2 to mitigate brute force crack.

## User Data

Contains sensitive information including mnemonics and private keys.

- Utilize AES 256 for encryption and decryption.
- A salt for encryption is generated using a web crypto random function for each setup.
- The user data is stored in the web browser's local storage. This local storage is in turn stored on the user file system with the browser profile data.

## Decrypted Data Usage

- Private keys are saved in memory without having to ask the user for Master Password each time.
- Private keys are sent from different instances through local storage or web sockets that utilize ECDH and AES.
- Memory protection depends on the browser’s security.

## Communication

- All communication between Pawket wallet and Pawket servers is secured with HTTPS.
- Only insensitive information like spend-bundle, puzzle_hash, and coin_name can be sent to the server.
- Users are allowed to host the server on-premise for better security.

## Air-gapped Signing

Only save your mnemonic on an offline device and sign using a QR code exchange.

## Shadow Wallet

Fully compatible with BIP39, with shadow wallet support.

## Super Light Wallet

Utilize server performance superiority, and mitigate the burden of Light Wallet to handle dusted accounts smoothly.

## Single Mnemonic Multiple Accounts

With a single mnemonic, the user can create multiple accounts bootstrap with a sequence or shadow password.

## Cross Platform

Support multiple platforms and cross-platform experiences.

## dApp Integration

External API that allows other dApp to be easily integrated into the Chia Blockchain.
