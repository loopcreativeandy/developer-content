---
title: Terminology
description:
  "Learn the essential terminology used throughout the Solana blockchain and
  development models."
keywords:
  - terms
  - dictionary
  - definitions
  - define
  - programming models
---

The following terms are used throughout the Solana documentation and development
ecosystem.

## account

A record in the Solana ledger that either holds data or is an executable
program.

Like an account at a traditional bank, a Solana account may hold funds called
[lamports](#lamport). Like a file in Linux, it is addressable by a key, often
referred to as a [public key](#public-key-pubkey) or pubkey.

The key may be one of:

- an ed25519 public key
- a program-derived account address (32byte value forced off the ed25519 curve)
- a hash of an ed25519 public key with a 32 character string

## account owner

The address of the program that owns the account. Only the owning program is
capable of modifying the account.

## app

A front-end application that interacts with a Solana cluster.

## associated token account

An account that stores tokens belonging to a particular `mint`. When sending
tokens, the sender can create associated token accounts for the recipient on
demand, so the recipient can recieve the token in the receipent's associated
token account.

## bank state

The result of interpreting all programs on the ledger at a given
[tick height](#tick-height). It includes at least the set of all
[accounts](#account) holding nonzero [native tokens](#native-token).

## base58

A way of representing binary values as strings. Unlike similar systems like
base64, base58 omits similar-appearing characters to avoid confusion. Solana
commonly uses base58 to refer to [wallet addresses](#wallet-address)

## block

A contiguous set of [entries](#entry) on the ledger covered by a
[vote](#ledger-vote). A [leader](#leader) produces at most one block per
[slot](#slot).

## blockhash

A unique value ([hash](#hash)) that identifies a record (block). Solana computes
a blockhash from the last [entry id](#entry-id) of the block.

## block height

The number of [blocks](#block) beneath the current block. The first block after
the [genesis block](#genesis-block) has height one.

## bootstrap validator

The [validator](#validator) that produces the genesis (first) [block](#block) of
a block chain.

## BPF loader

The Solana program that owns and loads
[BPF](developing/on-chain-programs/faq#berkeley-packet-filter-bpf) on-chain
programs, allowing the program to interface with the runtime.

## client

A computer program that accesses the Solana server network [cluster](#cluster).

## commitment

A measure of the network confirmation for the [block](#block).

## cluster

A set of [validators](#validator) maintaining a single [ledger](#ledger).

## compute budget

The maximum number of [compute units](#compute-units) consumed per transaction.

## compute units

The smallest unit of measure for consumption of computational resources of the
blockchain.

## confirmation time

The wallclock duration between a [leader](#leader) creating a
[tick entry](#tick) and creating a [confirmed block](#confirmed-block).

## confirmed block

A [block](#block) that has received a [super majority](#supermajority) of
[ledger votes](#ledger-vote).

## control plane

A gossip network connecting all [nodes](#node) of a [cluster](#cluster).

## cooldown period

Some number of [epochs](#epoch) after [stake](#stake) has been deactivated while
it progressively becomes available for withdrawal. During this period, the stake
is considered to be "deactivating". More info about:
[warmup and cooldown](implemented-proposals/staking-rewards.md#stake-warmup-cooldown-withdrawal)

## credit

See [vote credit](#vote-credit).

## cross-program invocation (CPI)

A call from one on-chain [program](@program) to another. For more information,
see
[calling between programs](developing/programming-model/calling-between-programs.md).

## data plane

A multicast network used to efficiently validate [entries](#entry) and gain
consensus.

## drone

An off-chain service that acts as a custodian for a user's secret key. It
typically serves to validate and sign transactions.

## entry

An entry on the [ledger](#ledger) either a [tick](#tick) or a
[transaction's entry](#transactions-entry).

## entry id

A preimage resistant [hash](#hash) over the final contents of an entry, which
acts as the [entry's](#entry) globally unique identifier. The hash serves as
evidence of:

- The entry being generated after a duration of time
- The specified [transactions](#transaction) are those included in the entry
- The entry's position with respect to other entries in [ledger](#ledger)

See [proof of history](#proof-of-history-poh).

## epoch

The time, i.e. number of [slots](#slot), for which a
[leader schedule](#leader-schedule) is valid.

## fee account

The fee account in the transaction is the account that pays for the cost of
including the transaction in the ledger. This is the first account in the
transaction. This account must be declared as Read-Write (writable) in the
transaction since paying for the transaction reduces the account balance.

## finality

When nodes representing 2/3rd of the [stake](#stake) have a common
[root](#root).

## fork

A [ledger](#ledger) derived from common entries but then diverged.

## genesis block

The first [block](#block) in the chain.

## genesis config

The configuration file that prepares the [ledger](#ledger) for the
[genesis block](#genesis-block).

## hash

A digital fingerprint of a sequence of bytes.

## inflation

An increase in token supply over time used to fund rewards for validation and to
fund continued development of Solana.

## inner instruction

See [cross-program invocation](#cross-program-invocation-cpi).

## instruction

The smallest contiguous unit of execution logic in a [program](#program). An
instruction specifies which program it is calling, which accounts it wants to
read or modify, and additional data that serves as auxiliary input to the
program. A [client](#client) can include one or multiple instructions in a
[transaction](#transaction). An instruction may contain one or more
[cross-program invocations](#cross-program-invocation-cpi).

## ix

Short for [instruction](#instruction).

## keypair

A [public key](#public-key-pubkey) and corresponding [secret key](#private-key)
for accessing an account.

## lamport

A fractional [native token](#native-token) with the value of 0.000000001
[sol](#sol).

:::info Within the compute budget, a quantity of
_[micro-lamports](https://github.com/solana-labs/solana/blob/ced8f6a512c61e0dd5308095ae8457add4a39e94/program-runtime/src/prioritization_fee.rs#L1-L2)_
is used in the calculation of [prioritization fees](#prioritization-fee). :::

## leader

The role of a [validator](#validator) when it is appending [entries](#entry) to
the [ledger](#ledger).

## leader schedule

A sequence of [validator](#validator) [public keys](#public-key-pubkey) mapped
to [slots](#slot). The cluster uses the leader schedule to determine which
validator is the [leader](#leader) at any moment in time.

## ledger

A list of [entries](#entry) containing [transactions](#transaction) signed by
[clients](#client). Conceptually, this can be traced back to the
[genesis block](#genesis-block), but an actual [validator](#validator)'s ledger
may have only newer [blocks](#block) to reduce storage, as older ones are not
needed for validation of future blocks by design.

## ledger vote

A [hash](#hash) of the [validator's state](#bank-state) at a given
[tick height](#tick-height). It comprises a [validator's](#validator)
affirmation that a [block](#block) it has received has been verified, as well as
a promise not to vote for a conflicting [block](#block) \(i.e. [fork](#fork)\)
for a specific amount of time, the [lockout](#lockout) period.

## light client

A type of [client](#client) that can verify it's pointing to a valid
[cluster](#cluster). It performs more ledger verification than a
[thin client](#thin-client) and less than a [validator](#validator).

## loader

A [program](#program) with the ability to interpret the binary encoding of other
on-chain programs.

## lockout

The duration of time for which a [validator](#validator) is unable to
[vote](#ledger-vote) on another [fork](#fork).

## major unit

The 'main' unit of a currency. For example, among fiat currencies, the dollar
(for USD), pound (for GBP) and Euro (for EUR) are major units. Solana's SOL
token has a major unit called the [Sol](#sol). Each Sol is worth 1 billion of
[lamports](#lamport). Since computers aren't good at decimal maths, transactions
usually use [minor-units](#minor-unit), converting to major units only for user
display.

## message

The structured contents of a [transaction](#transaction). Generally containing a
header, array of account addresses, recent [blockhash](#blockhash), and an array
of [instructions](#instruction).

Learn more about the
[message formatting inside of transactions](./developing/programming-model/transactions.md#message-format)
here.

## memo

The [memo program](https://spl.solana.com/memo) provides a way to write text to
the blockchain. The Memo program is often added as an instruction to transfer
transactions as a text description by the sender of what the transfer is for.
For example, 'Thanks!❤️ '.

## minor unit

The lesser unit of a currency. Well known minor units include US cents (for
USD), pence (for GBP) and Eurocents (for EUR). Solana's Sol
[cryptocurrency](#cryptocurrency) has a minor unit called the
[Lamport](#lamport).

## mint

Verb. Minting tokens increases the supply of the tokens and transfers the new
tokens to a specific [token account](#token-account).

## mint account

An [account](#account) used to [mint](#mint) tokens to store in a separate
[token account](#token-account). Each token mint is unique for that network. For
example, [USDC on Solana mainnet](https://www.circle.com/en/usdc/developers) has
the mint address `EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v`.

## native token

The [token](#token) used to track work done by [nodes](#node) in a cluster.

## node

A computer participating in a [cluster](#cluster).

## node count

The number of [validators](#validator) participating in a [cluster](#cluster).

## on-chain program

A programthat runs on a blockchain that can read and modify accounts over which
the on-chain program has control.

## PoH

See [Proof of History](#proof-of-history-poh).

## point

A weighted [credit](#credit) in a rewards regime. In the [validator](#validator)
[rewards regime](cluster/stake-delegation-and-rewards.md), the number of points
owed to a [stake](#stake) during redemption is the product of the
[vote credits](#vote-credit) earned and the number of lamports staked.

## private key

Another word for the [#secret-key]. Solana tools and documentation generally use
the term 'secret key' instead.

## program (or on-chain program)

The executable code that interprets the [instructions](#instruction) sent inside
of each [transaction](#transaction) on the Solana.

These programs are often referred to as
"[_smart contracts_](./developing//intro/programs.md)" on other blockchains.

## program derived account (PDA)

An account whose signing authority is a program and thus is not controlled by a
secret key like other accounts.

## program id

The public key of the [account](#account) containing a [program](#program).

## proof of history (PoH)

A stack of proofs, each of which proves that some data existed before the proof
was created and that a precise duration of time passed before the previous
proof. Like a [VDF](#verifiable-delay-function-vdf), a Proof of History can be
verified in less time than it took to produce.

## prioritization fee

An additional fee user can specify in the compute budget
[instruction](#instruction) to prioritize their [transactions](#transaction).

The prioritization fee is calculated by multiplying the requested maximum
compute units by the compute-unit price (specified in increments of 0.000001
lamports per compute unit) rounded up to the nearest lamport.

Transactions should request the minimum amount of compute units required for
execution to minimize fees.

## public key (pubkey)

The public key of a [keypair](#keypair).

## rent

Fee paid by [Accounts](#account) and [Programs](#program) to store data on the
blockchain. When accounts do not have enough balance to pay rent, they may be
Garbage Collected.

See also [rent exempt](#rent-exempt) below. Learn more about rent here:
[What is rent?](../src/developing/intro/rent.md).

## rent exempt

Accounts that maintain more than 2 years with of rent payments in their account
are considered "_rent exempt_" and will not incur the
[collection of rent](../src/developing/intro/rent.md#collecting-rent).

## root

A [block](#block) or [slot](#slot) that has reached maximum [lockout](#lockout)
on a [validator](#validator). The root is the highest block that is an ancestor
of all active forks on a validator. All ancestor blocks of a root are also
transitively a root. Blocks that are not an ancestor and not a descendant of the
root are excluded from consideration for consensus and can be discarded.

## runtime

The component of a [validator](#validator) responsible for [program](#program)
execution.

## secret key

The secret key of a [keypair](#keypair).

## Sealevel

Solana's parallel runtime for [on-chain programs](#on-chain-program).

## shred

A fraction of a [block](#block); the smallest unit sent between
[validators](#validator).

## signature

A 64-byte ed25519 signature of R (32-bytes) and S (32-bytes). With the
requirement that R is a packed Edwards point not of small order and S is a
scalar in the range of 0 <= S < L. This requirement ensures no signature
malleability. Each transaction must have at least one signature for
[fee account](terminology#fee-account). Thus, the first signature in transaction
can be treated as [transaction id](#transaction-id)

## skip rate

The percentage of [skipped slots](#skipped-slot) out of the total leader slots
in the current epoch. This metric can be misleading as it has high variance
after the epoch boundary when the sample size is small, as well as for
validators with a low number of leader slots, however can also be useful in
identifying node misconfigurations at times.

## skipped slot

A past [slot](#slot) that did not produce a [block](#block), because the leader
was offline or the [fork](#fork) containing the slot was abandoned for a better
alternative by cluster consensus. A skipped slot will not appear as an ancestor
for blocks at subsequent slots, nor increment the
[block height](terminology#block-height), nor expire the oldest
`recent_blockhash`.

Whether a slot has been skipped can only be determined when it becomes older
than the latest [rooted](#root) (thus not-skipped) slot.

## slot

The period of time for which each [leader](#leader) ingests transactions and
produces a [block](#block).

Collectively, slots create a logical clock. Slots are ordered sequentially and
non-overlapping, comprising roughly equal real-world time as per
[PoH](#proof-of-history-poh).

## smart contract

Another term for [on chain programs](#programs). Solana programs should use the
term ["on-chain programs"](#program) or simply ["programs"](#program).

## sol

The [native token](#native-token) of a Solana [cluster](#cluster).

## Solana Program Library (SPL)

A [library of programs](https://spl.solana.com/) on Solana such as spl-token
that facilitates tasks such as creating and using tokens.

## stake

Tokens forfeit to the [cluster](#cluster) if malicious [validator](#validator)
behavior can be proven.

## supermajority

2/3 of a [cluster](#cluster).

## sx

Short for [signature](#signature).

## sysvar

A system [account](#account).
[Sysvars](developing/runtime-facilities/sysvars.md) provide cluster state
information such as current tick height, rewards [points](#point) values, etc.
Programs can access Sysvars via a Sysvar account (pubkey) or by querying via a
syscall.

## thin client

A type of [client](#client) that trusts it is communicating with a valid
[cluster](#cluster).

## tick

A ledger [entry](#entry) that estimates wallclock duration.

## tick height

The Nth [tick](#tick) in the [ledger](#ledger).

## token

A digitally transferable asset.

## token account

An [account](#account) that holds the balance for newly [mint](#mint)ed
[token](#token)s. Each token account is associated with a single
[mint account](#mint-account).

## tps

[Transactions](#transaction) per second.

## tpu

[Transaction processing unit](validator/tpu.md).

## transaction

One or more [instructions](#instruction) signed by a [client](#client) using one
or more [keypairs](#keypair) and executed atomically with only two possible
outcomes: success or failure.

## transaction id

The first [signature](#signature) in a [transaction](#transaction), which can be
used to uniquely identify the transaction across the complete [ledger](#ledger).

## transaction confirmations

The number of [confirmed blocks](#confirmed-block) since the transaction was
accepted onto the [ledger](#ledger). A transaction is finalized when its block
becomes a [root](#root).

## transactions entry

A set of [transactions](#transaction) that may be executed in parallel.

## tvu

[Transaction validation unit](validator/tvu.md).

## tx

Short for [transaction](#transaction).

## validator

A full participant in a Solana network [cluster](#cluster) that produces new
[blocks](#block). A validator validates the transactions added to the
[ledger](#ledger)

## VDF

See [verifiable delay function](#verifiable-delay-function-vdf).

## verifiable delay function (VDF)

A function that takes a fixed amount of time to execute that produces a proof
that it ran, which can then be verified in less time than it took to produce.

## vote

See [ledger vote](#ledger-vote).

## vote credit

A reward tally for [validators](#validator). A vote credit is awarded to a
validator in its vote account when the validator reaches a [root](#root).

## wallet

A collection of [keypairs](#keypair) that allows users to manage their funds.

## wallet address

The unique [public key](#public-key-pubkey) of a wallet. These are usually
displayed to programs or user interfaces as a [base58](#base58) string.

## wallet app

An end-user program with a user interface (UI) to interact with a blockchain. A
wallet typically includes a UI to send and recieve tokens and NFTs, check
balances of diffferent tokens, view NFTs, stake, and on/off ramp into fiat and
back. See the
[wallet apps on a Solana ecosytstem page](https://solana.com/ecosystem/explore?categories=wallet).

## wallet name

A unique memorable name used to refer to a [wallet address](#wallet-address).

## warmup period

Some number of [epochs](#epoch) after [stake](#stake) has been delegated while
it progressively becomes effective. During this period, the stake is considered
to be "activating". More info about:
[warmup and cooldown](cluster/stake-delegation-and-rewards.md#stake-warmup-cooldown-withdrawal)
