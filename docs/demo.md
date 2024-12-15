---
sidebar_position: 5
---

# Demo

This section walks you through a demo of the Radica Project on the Hedera network. Follow the steps below to get started.

## Download the Radica App

There are two ways to get the Radica App: **download from TestFlight** or **build from source**.

### Download from TestFlight

Downloading the Radica App from TestFlight is the easiest way to get started.

In order to get access to the Radica App on TestFlight, you need to provide your Apple ID to the Radica team. Once you have been added to the TestFlight list, you will receive an email from Apple with instructions on how to download the app.

Please send an email to [this email address](mailto:francescolaterza+radicademo00@gmail.com) specifying that you want to access Radica on TestFlight and your Name, Surname, and email address.

### Build from Source

You can build Radica from source directly from the Radica GitHub repository. Though you will need to have an active **Apple Developer account** to build the app since the app needs NFC capabilities, which are only available with an Apple Developer account.

To build Radica from source, follow these steps:

1. Clone the Radica repository from GitHub and checkout to the `demo` branch.:

```bash
git clone https://github.com/RadicaDev/radica-mobile-app.git && cd radica-mobile-app && git checkout demo
```

2. Install the dependencies:

```bash
npm install
```

3. Build and Install the app on your device:

   - If you are using macOS:

     1. Install Xcode

     2. Install Xcode command line tools

     3. Install Watchmain:

        ```bash
        brew install watchman
        ```

     4. Plug in your iPhone and enable developer mode:

        1. Connect your iOS device to your Mac using a USB to Lightning cable. Unlock the device and tap Trust if prompted.

        2. Open Xcode. From the menu bar, select Window > Devices and Simulators. You will see a warning in Xcode to enable developer mode.

        3. On your iOS device, open Settings > Privacy & Security, scroll down to the Developer Mode list item and navigate into it.

        4. Tap the switch to enable Developer Mode. After you do so, Settings presents an alert to warn you that Developer Mode reduces your device's security. To continue enabling Developer Mode, tap the alert's Restart button.

        5. After the device restarts and you unlock it, the device shows an alert confirming that you want to enable Developer Mode. Tap Turn On, and enter your device passcode when prompted.

     5. Run the following commands:

        ```bash
        npx expo run:ios --device --configuration Release
        ```

        If you experience any issues, try removing the `ios` folder and running the command again.

## Start using the Radica App

Once you have the Radica App installed on your device, you can start using it to interact with the Radica Project on the Hedera network.

### Create a new product tag

If you are in possess of a **NFC Reader**, and some **NFC Tags**, you can create a new product tag. The Smart Contract of the demo has been modified to allow anyone to create a tag in order to better test the application.

The **ACR122U** is fully compatible, other readers may work but are not guaranteed.

Be sure to have **ISO 14443, Type A, NFC Forum Type 2** tags. Every tag that implements this standard will work. We used the **NTAG215** by NXP which are very easy to find and cheap online.

To create a new product tag, follow these steps:

1. Clone the Radica repository from GitHub and checkout to the `demo` branch.:

```bash
git clone https://github.com/RadicaDev/radica-contracts.git && cd radica-contracts && git checkout demo
```

2. Install the dependencies:

:::warning Warining

Node version 23 is not supported. Please use Node version 22.

:::

```bash
npm install
```

3. Import your private key in the `.env` file:

```bash
cp .env.example .env
```

then copy your private key after `HEDERA_PRIVATE_KEY=`.

4. Initialize the tag:

```bash
npx hardhat run scripts/init-tags.ts
```

press `CTRL+C` to stop the script when you have initialized all the tags.

5. Create a new tag:

```bash
npx hardhat run scripts/create-tag.ts --network hederaTestnet
```

or use the `create-tags.ts` script to create multiple tags at once.

follow the instructions on the terminal to create the tag.

### Online authentication

If you don't have an NFC Reader or NFT Tags, you can still authenticate a product online. We have already created a tag so that you can test the application.

Go to the **online** screen and write the following address:

```
0xf474A4A7B05C3d7b7CC735f711A269fE52746d00
```

or scan the QR code below:

<img src="/img/qr-code.png" alt="QR Code" width="200"/>

:::note

Unfortunately, with the online authentication, you won't be able to experience all the features of the app. You won't be able to claim the ownership of the product, you can only do it if you are in possess of the physical tag.

:::

## Technical Details

This section provides more technical details about the Radica Demo project.

### NTC Tags

The NFC Tags used cannot perform digital signatures on-chip, thus the authentication method is slightly different. The tag UID is signed by Radica's private key and the signature is stored on the chip. The signature algorithm used is the ECDSA with the secp256k1 elliptic curve, the same as evm blockchains use, and the same as the ST25TA-E tags use.

The keys use to sign the tag are publically available for the demo. So that everibody can simulate the authentication process.

The tag has the following memory layout:

- **UID**: 8 bytes
- **reserved**: 8 bytes
- **Signature**: 68 bytes (65 + 3 of padding)
- **Proof**: 32 bytes (added at certificate creation)

The signature can be verified with the UID, the signature and the Radica address.

## Tag Address

The Tag address is derived from the signature. The private key is obtained as the keccak256 of the signature and the address is derived as in every EVM blockchain, thus compute the public key with the secp256k1 curve and then the address is computed as the last 20 bytes of the keccak256 of the public key.

:::note

A consideration is that the starting point to derive the address is the UID of the tag, which is only 8 bytes long, thus only 2^8 = 256 possible addresses can be derived from a single tag. This is not a limitation since the ECDSA signature includes a random element in the signature computation, which make two signatures of the same plaintext different. Even though two tags might have the same UID, their signature will be different, so will be their addresses.

:::
