---
sidebar_position: 3
---

# Radica Property

The `RadicaProperty.sol` contract handles the product ownership NFTs. The token ID of the NFT is the certificate ID of the product.

## Set Proof

When a product certificate is created, the `proofHash` relative to that product gets stored in the `RadicaProperty` contract calling the following function:

```solidity
function setProof(uint256 tokenId, bytes32 proofHash) public onlyOwner {
    require(_tokenIdToProofHash[tokenId] == 0, "Proof already set");
    _tokenIdToProofHash[tokenId] = proofHash;
}
```

## Claim Property

The owner of a product can claim the ownership of the product by calling the following function when in posses of the `proof`:

```solidity
function claimProperty(uint256 tokenId, bytes32 proof) public {
    require(
        _tokenIdToProofHash[tokenId] == keccak256(abi.encode(proof)),
        "Invalid proof"
    );

    string memory uri = string(
        abi.encodePacked(
            "Property of Tag with CertId: ",
            tokenId.toHexString()
        )
    );

    _safeMint(msg.sender, tokenId);
    _setTokenURI(tokenId, uri);
}
```

## Inheritance

The contract inherits from the `ERC721` contract and uses the **OpenZeppelin** library with the `ERC721Enumerable` and `ERC721URIStorage` extension. Refer to this [link](https://docs.openzeppelin.com/contracts/5.x/) for more details.
