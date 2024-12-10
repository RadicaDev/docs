---
sidebar_position: 2
---

# App Functionalities

The Radica Mobile App has three main sections:

- **Verify**: Allows users to verify the authenticity of a product by scanning the NFC Tag inside the product.
- **Online**: Allows users to verify the authenticity of an online product by entering the product's address or scanning the QR code.
- **Wallet**: Allows users to connect their wallet and visualize the product they own.

## Verify

The verify screen is the main screen of the app and it is what the user sees afeter the app is opened.

<img src="/img/verify-screen.png" alt="verify screen" height="480" />

It allows users to verify the authenticity of a product by scanning the NFC Tag inside the product. Pressing the Verify button will start the verification procedure.

### Signature Verification

When the NFC Tag is scanned, the app verify the authenticity of the tag. Note that this proves that the tag is authentic, but not the product itself. In order to prove the authenticity of the product the app should later check the certificate on the Blockchain.

There are two possible outcomes: **signature valid** and **signature invalid**.

<img src="/img/signature-verification-screen.png" alt="signature verification" height="480" />

### Product Verification

After the signature is verified, the app will check the certificate on the Blockchain. This will prove the authenticity of the product. Again, there are two possible outcomes: **product authentic** and **product not authentic**.

<img src="/img/product-authentication-screen.png" alt="product authentication" height="480" />

### Product Details

If the product is authentic, it is possible to visualize the detail of the product present in the certificate.

<img src="/img/product-details-screen.png" alt="product details" height="480" />

## Online

The Online screen allows users to verify the authenticity of an online product by entering the product's address or scanning the QR code.

<img src="/img/online-screen.png" alt="online screen" height="480" />

> **Note**: This only shows the certificate associated to that product, but those are publically available informations and can be accessed by anyone. It is not proven that the product tag is valid, or that it even has a tag. The product has to be considered authentic only when the NFC Tag is scanned.

## Wallet

The Wallet screen allows users to connect their wallet and visualize the product they own.

<img src="/img/wallet-screen.png" alt="wallet screen" height="480" />
