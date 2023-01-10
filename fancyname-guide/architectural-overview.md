---
description: A high-level overview of the FancyName design
---

# Architectural overview

As we mentioned earlier, FancyName is made up of two complementary components, a Themelio covenant and a set of Ethereum smart contracts. Both of these components work together to enable the transfer of assets on Themelio to Ethereum securely.

## The Themelio covenant

A Themelio covenant, somewhat analogous to a Bitcoin script, is a program attached to a coin (UTXO) that takes in a transaction and its execution context and decides whether or not that transaction can spend.

The FancyName covenant keeps track of Ethereum headers using Ethereum's native cryptographic primitives, trustlessly inheriting its security and allowing it to verify the contents of a particular storage mapping in the FancyName smart contracts, allowing it to act as a specialized Ethereum light-client.

A bridging event is initiated by the locking up of any coin to the Themelio bridge covenant. This covenant locks coins until they are proven to have been burned on the Ethereum network, at which point the coins are released. The covenant is written in Themelio's high-level language, [Melodeon](https://melodeonlang.org), and its source code can be viewed [here](https://github.com/themeliolabs/bridge-covenants).

### Covenant API

#### Locking coins

To lock coins up on the Themelio network, all that needs to be done is send a transaction to the covenant's address where:

* the first output of the transaction contains the coin that will be bridged
* the recipient's Ethereum address is encoded in the first outputs's `additional data` field.

#### Unlocking coins

To unlock coins, a transaction must be sent which attempts to spend the locked coin and has, in the `additional data` field of its first output, an encoded Merkle proof which proves the equivalent token amount was burned on Ethereum. This proof can be obtained by any Ethereum client.

## The Ethereum smart contracts

On the Ethereum network, the bridge is composed of a set of smart contracts which together enable light client verification of Themelio block headers, staker sets, and transactions. This is possible through the use Themelio's underlying cryptographic primitives which allow the smart contracts to trustlessly inherit Themelio's native security. The source code for the FancyName smart contracts can be viewed [here](https://github.com/themeliolabs/bridge-sol).

### Smart contracts minimal API

#### verifyStakes(uint256 blockHeight, bytes stakesDatablock, uint256 datablockIndex, bytes32\[] stakesProof) returns (bool)

This function is used for hashing a stakes tree datablock using Blake3 and storing its height in a hash mapping in contract storage for subsequent verification of Themelio headers.

* `blockHeight`: the block height of the header which will be used to verify the included stakes datablock
* `stakesDatablock`: a `bytes` consisting of a serialized Themelio stakes tree datablock
* `datablockIndex`: the index of the datablock within the stakes tree
* `stakesProof`: the Merkle proof used to prove inclusion of the datablock within `blockHeight` header's stakes hash

Returns `true` if `stakes` were successfully hashed and stored, reverts otherwise.

#### verifyHeader(bytes header, bytes32\[] signers, bytes32\[] signatures) returns (bool success)

Stores header information for a particular block height once the header has been verified through ed25519 signature verification of stakes worth at least 2/3 of total sym staked for that epoch.

The process of header verification can be completed in multiple transactions in the case of particularly computationally intensive verifications which exceed the block gas limit. In this case, progress is saved in an intermediary state until the header has enough votes for verification.

* `header`: the bincode serialized Themelio block header in `bytes`
* `signers`: array of 32-byte ed25519 public keys of stakers that have signed `header`, in `bytes32[]`
* `signatures`: array of 64-byte ed25519 staker signatures of `header` in the same order as `signers`, split into 32-byte `R` and 32-byte `s` (this means `signatures.length == signers.length * 2`)

Returns `true` if the header was successfully verified and stored, reverts otherwise.

#### verifyTx(bytes transaction, uint256 txIndex, uint256 blockHeight, bytes32\[] proof) returns (bool success)

Verifies that `transaction` was included in the header at `blockHeight` by running a proof of inclusion using `proof` and comparing the result with the transactions hash of the header. Once the transaction has been proven to have been included in the block, the value and denomination of the first output of the transaction will be minted and sent to the Ethereum address included in the addition data field of the output.

* `transaction`: the bincode serialized Themelio transaction, in `bytes`
* `txIndex`: the transaction's index within the block, as `uint256`
* `blockHeight`: the block height of the header `transaction` is included in, as `uint256`
* `proof` - an array of the sibling hashes comprising the Merkle proof, as `bytes32[]`

Returns `true` if the header was successfully verified and stored, reverts otherwise.

#### burn(address account, bytes32 txHash, bytes32 themelioRecipient)

Burns tokens owned by `account` with the value and denom specified by the locked Themelio coin with transaction hash `txHash`. If successful the burned coin's status will be updated to be `themelioRecipient`, indicating the coin has been burned and claimed, to allow unlocking on the Themelio network.

* `account`: the account owning the tokens to be burned
* `txHash`: the transaction hash of the Themelio coin to be released (the coin will always be considered the first output of that transaction)
* `themelioRecipient`: the address to release the burned coin to on the Themelio network

## Next steps

In this guide, we learned about both the function and architecture of FancyName and how it is powered by the native security of both Themelio and Ethereum. In the next section we will learn how we can help strengthen the security of the Themelio network ourselves by participating in staking.
