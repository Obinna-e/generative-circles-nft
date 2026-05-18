generative-circles-nft
ERC-721 smart contract for a generative-art NFT collection on Polygon. Part of a personal generative-art exploration during my 2020 to 2022 smart contract dev work.

Not affiliated with CirclesUBI. The name predates familiarity with that project.

The project
Circles is a generative NFT collection where each piece is a single coloured circle on a coloured background. Foreground colour, background colour, and radius are drawn at random per token. Polygon was chosen to keep gas costs negligible for an experimental drop.
The project spans three repositories:

Circles-contract (this repo): the on-chain Solidity contract.
Nft-Generator: a Flutter app that generates the art (PNG) and ERC-721 metadata (JSON) locally, ready to be pinned to IPFS.
NFT-Contract-Minter: a Flutter app that calls the deployed contract to mint each token with its IPFS metadata URI.

What the contract does

ERC-721 with owner-only minting. mint(tokenURI) is callable only by the contract owner. The owner generates art off-chain, pins it to IPFS, and mints each token with its metadata URI.
On-chain tokenURI mapping per token (custom implementation; see "Known limitations").
OpenSea Wyvern proxy auto-approval. A hard-coded operator address is auto-approved in isApprovedForAll, sparing collectors a separate approval transaction when listing on OpenSea.

Tech stack

Solidity ^0.8.4
OpenZeppelin Contracts 4.7.3 (ERC721, Ownable)
Hardhat 2.10 with hardhat-toolbox
Tests via Mocha, Chai, and ethereum-waffle
Etherscan source verification via hardhat-etherscan
Deployment target: Polygon

Historical context
This contract is from the Wyvern era of OpenSea, before the Seaport upgrade in mid-2022. The hard-coded operator-address auto-approval pattern was the standard way to remove a redundant approval transaction when listing. After OpenSea moved to Seaport, this optimisation became obsolete, and modern contracts should not hard-code OS proxy addresses.
Known limitations
Things a current rewrite would do differently:

Reimplements ERC721URIStorage. OpenZeppelin's ERC721URIStorage extension already provides _setTokenURI and per-token tokenURI storage. The custom _tokenURIs mapping here duplicates that with no added behaviour.
Counters is imported but unused. A plain uint256 tokenCounter is what's actually used. Drop the import.
OpenSea proxy auto-approval is now a footgun. Hard-coding operator addresses is no longer recommended post-Seaport; the override should be removed in any modern rewrite.
onlyOwner minting is fine for an artist-curated drop but locks the contract to a single-source workflow. Public-mint with an allowlist would be the standard pattern today.
No supportsInterface override for any extension interfaces; not a bug for this minimal contract, but worth noting.

License
Unlicense (public domain).
