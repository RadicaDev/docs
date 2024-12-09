---
sidebar_position: 2
---

# Radica Tag

The `RadicaTag.sol` contract handles the creation, the storage and the retrieval of the tag certificates.

## Create Tag

In order to create a Tag the followning function should be called:

```solidity
function createTag(
    address tagAddr,
    Metadata memory metadata,
    TracebilityMetadata memory tracebilityMetadata,
    bytes32 proofHash
) public onlyOwner {
    require(proofHash != 0, "Proof hash cannot be zero");
    require(tagAddrToCert[tagAddr].id == 0, "Tag already in use");

    uint256 certId = _deriveCertId(msg.sender, tagAddr);

    tagAddrToCert[tagAddr] = Certificate({
        id: certId,
        metadata: metadata,
        tracebilityMetadata: tracebilityMetadata
    });

    (bool success, ) = radicaPropertyAddr.call(
        abi.encodeWithSignature(
            "setProof(uint256,bytes32)",
            certId,
            proofHash
        )
    );
    require(success, "Call to setProof failed");
}
```

The `createTag` function takes the following parameters:

- `tagAddr`: the address of the tag
- `metadata`: the metadata of the tag
- `tracebilityMetadata`: the tracebility metadata of the tag
- `proofHash`: the hashed proof of the tag (needed to claim the property of the product)

The function can only be called by Radica. This is because it is a root of trust for the system, thus the certificates should be issued by Radica.

## Retrieve Certificate

The certificate of a product can be retrieved by the public mapping `tagAddrToCert` which maps the tag address to the certificate.

## Certificate Format

The certificate is stored in the following format:

```solidity
struct Metadata {
    string serialNumber;
    string name;
    string description;
    string image;
    string manufacturer;
    string externalUrl;
}

struct TracebilityMetadata {
    string batchId;
    string supplierChainHash;
}

struct Certificate {
    uint256 id;
    Metadata metadata;
    TracebilityMetadata tracebilityMetadata;
}
```

The **Certificate Id** is derived from the tag address and the issuer address. It has type `uint256`: the first 96 bit are formed by the **Issuer Fingerprint** while the last 160 bit are the **Tag Address**.

```solidity
function _deriveCertId(
    address issuerAddr,
    address tagAddr
) private pure returns (uint256) {
    uint256 issuerAddrUint = uint256(uint160(issuerAddr));
    uint256 tagAddrUint = uint256(uint160(tagAddr));

    uint256 issuerFP = issuerAddrUint >> 64;

    return (issuerFP << 160) | tagAddrUint;
}
```

This is needed to make it compatible with multiple Certificate Authorities in the future.
