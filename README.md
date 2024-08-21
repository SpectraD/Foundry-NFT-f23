# Project Explanation
The project is a collection of Solidity smart contracts for the creation and management of NFTs (Non-Fungible Tokens) on the Ethereum blockchain. Here's a quick description of what each file does:

- `script/DeployBasicNft.s.sol`: Script for deploying the `BasicNft` contract.
- `script/DeployMoodNft.s.sol`: Script for deploying the `MoodNft` contract with base64 encoded SVG images.
- `script/Interactions.s.sol`: Script for interacting with the `BasicNft` contract, in particular for mining NFTs.
- `src/BasicNft.sol`: Contract implementing a basic NFT with functions for mining and obtaining token URIs.
- `src/MoodNft.sol`: Contract implementing a mood-based NFT, with functions for mining NFTs and changing their mood.
- `test/integrations/BasicNftTest.t.sol`: Integration tests for the `BasicNft` contract.
- `test/integrations/MoodNftIntegrationTest.t.sol`: Integration tests for the `MoodNft` contract.
- `test/unit/DeployMoodNftTest.t.sol`: Unit tests for the `DeployMoodNft` script.
- `test/unit/MoodNftTest.t.sol`: Unit tests for the `MoodNft` contract.


# Project Files Explanation

## Solidity Files

### `script/DeployBasicNft.s.sol`

This script deploys the `BasicNft` contract:
- **Imports**: `Script` from `forge-std` and `BasicNft` from `../src/BasicNft.sol`.
- **Function `run`**: Starts a broadcast, deploys a new `BasicNft` contract, stops the broadcast, and returns the deployed contract instance.

### `script/DeployMoodNft.s.sol`

This script deploys the `MoodNft` contract:
- **Imports**: `Script` and `console` from `forge-std`, `MoodNft` from `../src/MoodNft.sol`, and `Base64` from `@openzeppelin/contracts/utils/Base64.sol`.
- **Function `run`**: Reads SVG files, starts a broadcast, deploys a new `MoodNft` contract with the SVG URIs, stops the broadcast, and returns the deployed contract instance.
- **Function `svgToImageURI`**: Converts an SVG string to a base64-encoded data URI.

### `script/Interactions.s.sol`

This script interacts with the `BasicNft` contract:
- **Imports**: `Script` from `forge-std`, `DevOpsTools` from `lib/foundry-devops/src/DevOpsTools.sol`, and `BasicNft` from `../src/BasicNft.sol`.
- **Function `run`**: Retrieves the most recently deployed `BasicNft` contract and mints an NFT on it.
- **Function `mintNftOnContract`**: Mints an NFT on the specified contract address.

### `src/BasicNft.sol`

This contract implements a basic NFT:
- **Imports**: `ERC721` from `@openzeppelin/contracts/token/ERC721/ERC721.sol`.
- **State Variables**: `s_tokenCounter` for tracking token IDs and `s_tokenIdToUri` for mapping token IDs to URIs.
- **Constructor**: Initializes the contract with the name "Dogie" and symbol "DOG".
- **Function `mintNft`**: Mints a new NFT with a specified URI.
- **Function `tokenURI`**: Returns the URI of a specified token ID.

### `src/MoodNft.sol`

This contract implements a mood-based NFT:
- **Imports**: `ERC721` from `@openzeppelin/contracts/token/ERC721/ERC721.sol` and `Base64` from `@openzeppelin/contracts/utils/Base64.sol`.
- **State Variables**: `s_tokenCounter`, `s_sadSvgImageUri`, `s_happySvgImageUri`, and `s_tokenIdToMood`.
- **Constructor**: Initializes the contract with the name "Mood NFT" and symbol "MN", and sets the SVG URIs.
- **Function `flipMood`**: Flips the mood of a specified token ID.
- **Function `_baseURI`**: Returns the base URI for the token metadata.
- **Function `mintNft`**: Mints a new NFT with a default mood of HAPPY.
- **Function `tokenURI`**: Returns the metadata URI of a specified token ID based on its mood.

## Test Files

### `test/integrations/BasicNftTest.t.sol`

Integration tests for the `BasicNft` contract:
- **Imports**: `Test` from `forge-std/Test.sol`, `DeployBasicNft` from `../../script/DeployBasicNft.s.sol`, and `BasicNft` from `../../src/BasicNft.sol`.
- **State Variables**: `deployer`, `basicNft`, `USER`, and `PUG`.
- **Function `setUp`**: Deploys a new `BasicNft` contract.
- **Function `testNameIsCorrect`**: Asserts that the contract name is "Dogie".
- **Function `testCanMintAndHaveABalance`**: Mints an NFT and asserts the balance and token URI.

### `test/integrations/MoodNftIntegrationTest.t.sol`

Integration tests for the `MoodNft` contract:
- **Imports**: `Test` and `console` from `forge-std/Test.sol`, `MoodNft` from `../../src/MoodNft.sol`, and `DeployMoodNft` from `../../script/DeployMoodNft.s.sol`.
- **State Variables**: `moodNft`, `HAPPY_SVG_IMAGE_URI`, `SAD_SVG_IMAGE_URI`, `SAD_SVG_URI`, `deployer`, and `USER`.
- **Function `setUp`**: Deploys a new `MoodNft` contract.
- **Function `testViewTokenURIIntegration`**: Mints an NFT and logs its token URI.
- **Function `testFlipTokenToSad`**: Mints an NFT, flips its mood to SAD, and asserts the token URI.

### `test/unit/DeployMoodNftTest.t.sol`

Unit tests for the `DeployMoodNft` contract:
- **Imports**: `Test` and `console` from `forge-std/Test.sol`, and `DeployMoodNft` from `../../script/DeployMoodNft.s.sol`.
- **State Variables**: `deployer`.
- **Function `setUp`**: Initializes the deployer.
- **Function `testConvertSvgToUri`**: Asserts that the SVG to URI conversion is correct.

### `test/unit/MoodNftTest.t.sol`

Unit tests for the `MoodNft` contract:
- **Imports**: `Test` and `console` from `forge-std/Test.sol`, and `MoodNft` from `../../src/MoodNft.sol`.
- **State Variables**: `moodNft`, `HAPPY_SVG_URI`, `SAD_SVG_URI`, and `USER`.
- **Function `setUp`**: Initializes a new `MoodNft` contract.
- **Function `testViewTokenURI`**: Mints an NFT and logs its token URI.