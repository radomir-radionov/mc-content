# Blockchain — Interview

Answers cover the published question list.

[← Back to hub](./7_special_algorithms.md)

### Related docs

- [Blockchain — Interview](./7_algo_blockchain_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Blockchain](https://devinterview.io/questions/data-structures-and-algorithms/blockchain-interview-questions/).

### Blockchain Fundamentals

<details>
<summary>Can you explain what a blockchain is and how it works?</summary>

### Can you explain what a blockchain is and how it works?

**Definition:**

A blockchain is a replicated log of blocks. Each block commits to the previous block’s hash, so the log has one checkable history.

**How it works:**

- A block holds a batch of transactions, a timestamp or height, and the hash of the parent block.
- Nodes collect pending transactions, check signatures and balances, and propose a block under the consensus rule.
- Because block `n` stores `hash(block n−1)`, editing an old transaction changes every hash after it.
- Peers store copies. A peer rejects a chain whose links do not match or whose transactions break the protocol.
- “The” chain is the one the fork-choice rule prefers when two valid tips exist. The hash links make a quiet edit visible. They do not by themselves decide which of two valid tips wins.

**Examples:**

Block 2 stores `prev = H(block 1)`. Change a transfer inside block 1 and `H(block 1)` no longer equals that `prev`. Every later block has to be rebuilt to look consistent.
</details>

<details>
<summary>What is the difference between a public blockchain and a private blockchain?</summary>

### What is the difference between a public blockchain and a private blockchain?

**Definition:**

A public chain lets anyone read, submit, and help decide the next block. A private chain restricts those roles to a known operator or consortium.

**How it works:**

- Public membership is open. Safety comes from the cost of outvoting the rest of the hash power or the stake, not from an access list.
- Private membership is an administrative decision. The operator can add or remove validators, which buys access control and usually higher throughput.
- Readers of a public chain can check history themselves. Readers of a private chain trust the operator who controls who may append.
- “Permissioned” names the access rule. A ledger can be permissioned and still be run by several organizations. A single company’s database with a hash chain is private and is not decentralized in the public-chain sense.
- The data structure, a hash-linked log, is the same. What changes is who is allowed to write and who is trusted to choose the tip.

**Examples:**

Bitcoin is public: anyone can relay a transaction and mine. A bank’s internal ledger that only member banks may append is private, even if each block hashes the previous one.
</details>

<details>
<summary>What are the key features of a blockchain that make it secure?</summary>

### What are the key features of a blockchain that make it secure?

**Definition:**

Security comes from hash links, signatures, replication, and a consensus rule that makes a conflicting history expensive or organizationally hard to publish.

**How it works:**

- Each block stores the parent hash, so a quiet edit is caught by recomputing the chain.
- A signature shows that the private-key holder authorized that transaction. Other nodes check it with the public key.
- Many independent copies mean there is no single database to edit in secret.
- Consensus defines which valid tip wins. Under proof of work, rewriting history means redoing the work of every later block. Under a vote protocol, a minority cannot gather the quorum.
- A run of later blocks raises the cost of replacing an old one, because the replacement has to become the tip the fork-choice rule still prefers.
- None of this hides transaction contents on a public chain. Integrity and authentication are not confidentiality.

**Examples:**

To replace a payment ten blocks back on a proof-of-work chain, an attacker must remine that block and the ten after it faster than the honest chain grows.
</details>

<details>
<summary>How does a blockchain ensure data integrity and prevent tampering?</summary>

### How does a blockchain ensure data integrity and prevent tampering?

**Definition:**

Integrity is tamper evidence: any change to an old record changes its hash, which breaks the link stored in the next block. Honest nodes recompute those links and drop a chain that fails.

**How it works:**

- The hash in block `n` depends on block `n−1` and, through that link, on everything earlier.
- Transactions inside a block are usually summarized by a Merkle root, so one changed transaction changes the block hash as well.
- Nodes also check signatures and the protocol rules for balances or unspent outputs. A hash-valid block with an illegal transfer is still rejected.
- This detects an altered copy. It does not stop a majority of the validating power from adopting a different valid history under the consensus rules.
- “Immutable” in this setting means “altering the past is detectable and costly”, not “the bits cannot be rewritten on one disk”.

**Examples:**

Flip one bit of an old transaction. The Merkle root changes, the block hash changes, and the child block’s `prev` no longer matches. A node that recomputes the header rejects that file.
</details>

<details>
<summary>Can you explain the concept of decentralization in blockchain?</summary>

### Can you explain the concept of decentralization in blockchain?

**Definition:**

Decentralization means no single party owns the ledger or can unilaterally decide which transactions are valid. Independent nodes apply the same public rules to their own copies.

**How it works:**

- Each full node downloads blocks, checks them, and relays what it accepts. A user can verify instead of trusting one database administrator.
- The degree varies. A handful of mining pools or a small validator set is more centralized than a diagram of equal peers.
- Open membership, as in proof of work, pushes the decision toward whoever pays for the work. Stake pushes it toward whoever locks the coins.
- A private chain run by one operator is a replicated database with an administrator. The hash links do not make it decentralized.
- Decentralization is a trade against throughput and against the ability to reverse a mistake. There is no help desk that can edit an old public block.

**Examples:**

If one Bitcoin node is shut off, the network still accepts blocks. If the single operator of a private ledger is shut off, appending stops, which is the centralized case.
</details>

<details>
<summary>What is a consensus mechanism, and why is it important in blockchain technology?</summary>

### What is a consensus mechanism, and why is it important in blockchain technology?

**Definition:**

A consensus mechanism is the rule by which nodes pick the next block and, when two valid blocks compete, which chain is authoritative.

**How it works:**

- The network delays and drops messages, and some nodes may be faulty or dishonest. Local validity checks are not enough, because two valid blocks can exist at the same height.
- The mechanism defines safety: honest nodes should not finalize conflicting histories. It defines liveness: the chain should still grow.
- Proof of work uses accumulated work as the fork choice. Proof of stake uses locked coins and votes. A permissioned protocol uses a quorum of known validators.
- Without a shared rule, peers can accept different histories and a double-spend can survive on one of them.
- The mechanism is not the hash link. The hash link checks integrity. Consensus chooses which intact chain the network treats as current.

**Examples:**

Two miners publish different valid blocks at height 800,000. The next proof-of-work blocks decide the winner: honest nodes switch to the heavier chain and drop the other tip.
</details>

<details>
<summary>Describe the most common types of consensus algorithms and how they differ.</summary>

### Describe the most common types of consensus algorithms and how they differ.

**Definition:**

The common families differ in who may propose a block, what they spend, and whether a block is final when it appears or only probably final.

**How it works:**

- Proof of work: miners search for a block hash under a target. The heaviest valid chain wins. Membership is open. Rewriting history takes a majority of the hash power, and the search spends energy.
- Proof of stake: validators lock coins and are selected to propose or vote. Misbehavior can destroy that stake. Influence follows the locked coins, and the energy cost is far lower than a hash search.
- Practical Byzantine Fault Tolerance and its relatives use a known validator set. A block is final once a supermajority of votes agrees. They suit a permissioned network and give explicit finality. They do not grow an open mining population.
- Safety assumptions differ. Proof of work assumes an honest majority of hash power over time. Voting protocols assume fewer than a threshold of validators are Byzantine, often one third.
- Throughput and openness move in opposite directions. An open race is slower to finalize than a small voting committee.

**Examples:**

Bitcoin finalizes by depth: six further blocks make a reversal expensive, not impossible. A three-node PBFT committee finalizes when two of the three (or the protocol’s supermajority) have voted, and that vote is the finality.
</details>

<details>
<summary>How does a blockchain achieve consensus in a distributed environment?</summary>

### How does a blockchain achieve consensus in a distributed environment?

**Definition:**

Each node checks blocks locally, then applies one fork-choice rule to whatever valid tips it has seen. Messages arrive late, so two honest nodes can disagree for a while and still converge.

**How it works:**

- Transactions and blocks are gossiped. A node does not wait for a global lock.
- Validity is local: signatures, structure, and the ledger rules. Invalid data is not voted on; it is dropped.
- Fork choice is the shared rule. Proof of work prefers the heaviest chain. Proof of stake prefers the checkpoint justified by the most stake, or the head the votes support. A quorum protocol prefers the block a supermajority committed.
- Two heads can exist until more blocks or more votes arrive. Honest nodes that see the same evidence then pick the same tip.
- Finality is probabilistic under proof of work: a deeper block is harder to replace, not formally impossible. Under a voting protocol it is explicit once the quorum has committed.

**Examples:**

Node A sees block `X` first. Node B sees block `Y` first. Both are valid. When both nodes later see a child of `X` and none of `Y`, the heaviest-chain rule moves B onto `X`.
</details>

<details>
<summary>What is a node in the blockchain context, and what role does it play?</summary>

### What is a node in the blockchain context, and what role does it play?

**Definition:**

A node is a process that stores chain data and speaks the peer-to-peer protocol. Not every node checks the same amount of history.

**How it works:**

- A full node downloads blocks, checks every transaction and consensus rule, and relays what it accepts. It does not have to mine or stake.
- A light client stores headers and asks a full node for the pieces it needs. It trusts the header chain and a Merkle proof rather than replaying every transaction.
- Miners and validators are nodes that also propose blocks. Proposal is extra. Verification is the baseline.
- Users can check the chain themselves only while running a full node stays practical. If blocks are too heavy, people delegate verification and the network becomes easier to fool.
- Shutting off one full node does not stop a public network. Shutting off the only node of a private deployment does.

**Examples:**

A wallet on a phone that checks a Merkle proof against a header is a light client. The process that downloaded every Bitcoin block and rejected an invalid signature is a full node, whether or not it mines.
</details>

<details>
<summary>Can you explain what a blockchain fork is and why it might occur?</summary>

### Can you explain what a blockchain fork is and why it might occur?

**Definition:**

A fork is a split in the chain or in the rules nodes enforce. Short forks are ordinary. Rule forks split the software.

**How it works:**

- A transient fork happens when two valid blocks are produced at the same height before either has spread. The fork-choice rule later drops one branch. Its transactions return to the mempool if they are still valid.
- A hard fork changes the rules so old software rejects the new blocks. Nodes that do not upgrade stay on a separate chain. A disputed hard fork is how one network becomes two assets.
- A soft fork tightens the rules in a way old nodes still accept as valid, while upgraded nodes reject the newly forbidden blocks.
- Accidental forks come from latency. Intentional forks come from an upgrade or from an attack that privately mines a competing tip.
- Applications that treat the latest block as final will observe a payment and then see it disappear if that block is the one the fork choice drops.

**Examples:**

Two blocks at height 100, then a child only on the first. Nodes switch to that heavier tip. The transactions unique to the dropped block become pending again, unless they conflict with the winning branch.
</details>

### Cryptography in Blockchain

<details>
<summary>How does cryptography secure transactions and blocks in a blockchain?</summary>

### How does cryptography secure transactions and blocks in a blockchain?

**Definition:**

Cryptography supplies integrity and authentication. The sender signs the transaction, and hashes commit to the exact bytes of transactions and blocks. Which of two valid chains wins is still a consensus decision.

**How it works:**

- The sender signs the transaction bytes with a private key. Every other node checks that signature with the public key and never learns the private key.
- A block hash, and the Merkle root of the transactions, commits to those bytes. Any edit changes the digest.
- Proof of work is itself a search for a hash in a target range. The puzzle binds the block to a measurable amount of work.
- An address is commonly a hash of a public key, so the key need not appear on the chain until it spends.
- This does not encrypt a typical public-chain transfer. The amount and the recipient are readable. Confidentiality needs a separate scheme.

**Examples:**

A node rejects a transfer whose signature does not match the public key that owns the coins, even if the block hash is well formed. It also rejects a block whose stored parent hash is not the hash it just computed.
</details>

<details>
<summary>What is a hash function, and why is it crucial to blockchain technology?</summary>

### What is a hash function, and why is it crucial to blockchain technology?

**Definition:**

A cryptographic hash maps an input of any length to a fixed-size digest. Blockchains use it as the glue between blocks and as the summary of a block’s transactions.

**How it works:**

- The same input always produces the same digest, and computing it is fast.
- A one-bit change spreads across the output, so similar transactions do not have similar hashes.
- Preimage resistance makes the digest hard to invert. Collision resistance makes it hard to find two inputs with the same digest. Those are the properties a chain relies on when it treats the digest as a name for the bytes.
- Uses: the parent pointer, the Merkle root, addresses derived from keys, and the proof-of-work puzzle.
- A hash is not encryption. It is not built to be reversed and read back. Knowing `H(block)` does not reveal the block.

**Examples:**

`H("pay Alice 1")` and `H("pay Alice 2")` are unrelated digests. The second is not a small edit of the first, which is why a changed amount cannot hide inside an old block hash.
</details>

<details>
<summary>Can you explain asymmetric encryption and how it’s used in blockchains?</summary>

### Can you explain asymmetric encryption and how it’s used in blockchains?

**Definition:**

Asymmetric cryptography uses a key pair: an operation done with one key is checked or undone only with the other. On a blockchain the common use is a signature, not secrecy of the transfer.

**How it works:**

- The private key stays with its owner. The public key, or an address derived from it, is what the chain stores.
- The owner signs with the private key. Anyone verifies with the public key. A valid check shows authorization of those exact bytes.
- The same mathematics can encrypt a payload for the holder of a private key. A typical public-chain transaction does not do that. Every node must read the amount and the recipient to check the ledger rules.
- Publishing the public key does not reveal the private key, because the derivation is one-way, usually multiplication on an elliptic curve.
- Losing the private key loses the ability to authorize spends. There is no recovery from the public data on the chain.

**Examples:**

Alice signs “pay Bob 3” with her private key. Bob and every full node verify with Alice’s public key. They learn that Alice authorized it. They do not learn a secret they can use to spend her other coins.
</details>

<details>
<summary>What is a digital signature, and how does it provide authenticity in transactions?</summary>

### What is a digital signature, and how does it provide authenticity in transactions?

**Definition:**

A digital signature is a short value computed from the transaction bytes and the signer’s private key. A verifier accepts it only when those same bytes match the public key.

**How it works:**

- The signature covers a hash of the transaction, not a separate sticky note. Changing the amount or the recipient makes verification fail.
- It shows authenticity: the private-key holder produced it. It shows integrity of the signed bytes. It does not hide them.
- Schemes such as ECDSA and EdDSA are used because the signatures are short and every node can verify them cheaply.
- A signature does not prove a person, only a key. Whoever holds the key can produce a signature the network will accept.
- Replay is a protocol concern. A signature of “pay Bob 3” must be bound to a nonce, a chain id, or a specific unspent output, or the same bytes could be submitted twice.

**Examples:**

```text
sig = sign(private_key, hash(tx))
ok = verify(public_key, hash(tx), sig)
```

Edit `tx.amount` after signing and `ok` becomes false, without anyone needing the private key to notice.
</details>

<details>
<summary>How do public and private keys work within blockchain?</summary>

### How do public and private keys work within blockchain?

**Definition:**

The private key is a secret random number. The public key is derived from it by a one-way function. Spending requires a signature that verifies under the public key the coins were sent to.

**How it works:**

- Generation starts with randomness. A weak random source produces a guessable private key.
- On the curves blockchains actually use, the public key is scalar multiplication of a fixed base point. Going backward, from the point to the scalar, is the hard problem.
- You distribute the public key or, more often, an address that is a hash of it. You do not distribute the private key.
- The chain checks `verify(public_key, tx, signature)`. It never sees the private key, and it cannot recover one.
- Losing the private key loses control of the funds. There is no administrator reset. Exposing it lets anyone produce a signature the network will accept.
- Wallets often derive many private keys from one seed so a single backup covers the accounts. The seed is then the secret that must not leak.

**Examples:**

Coins sent to an address derived from public key `P` move only when a later transaction carries a signature that `P` verifies. A signature made by a different key fails, even if the attacker copies the rest of a previous transaction.
</details>

### Smart Contracts

<details>
<summary>Define smart contracts and how they operate within a blockchain.</summary>

### Define smart contracts and how they operate within a blockchain.

**Definition:**

A smart contract is a program published into Ethereum state. Users do not send new code for every request. They send a transaction that asks the network to run that program with arguments.

**How it works:**

- The intro describes it as a vending machine: given parameters, it performs the action if the conditions hold. Developers pay a fee to publish it. Callers pay a fee to execute it.
- Every node runs the same execution. The EVM is the single computer whose state those nodes agree on. A successful call changes that state and the change is stored on the chain.
- Contract creation is one kind of transaction. A message call is the other. The EVM page says creation stores bytecode on a new contract account.
- The fee is not optional. The intro says the ETH bounty is what stops a caller from requesting an infinite loop for free.

**Sources:**

https://ethereum.org/developers/docs/intro-to-ethereum/
https://ethereum.org/developers/docs/evm/
</details>

<details>
<summary>What platforms can you use to deploy smart contracts, and what are their differences?</summary>

### What platforms can you use to deploy smart contracts, and what are their differences?

**Definition:**

Ethereum is the platform these notes can describe from its own docs: a chain with the EVM embedded, proof-of-stake validators, and programs stored in state. Bitcoin, on the same page, is the ledger comparison: rules about what a coin payment may do, without that general-purpose computer.

**How it works:**

- On Ethereum a developer publishes bytecode once. Later transactions call it. The intro lists marketplaces, financial instruments, and games as the kind of app that bytecode can be.
- Bitcoin in that comparison maintains a record that must follow payment rules, such as not spending more than was received. It is not described there as a machine that runs arbitrary uploaded programs.
- A contract call still needs a transaction: from, to, signature, nonce, value, input data, and gas fields. The transactions page says contract accounts do not send transactions. An externally owned account does.
- Other chains are outside these pages. Do not treat a name that is not in the Ethereum docs as if those docs specified its fee market or virtual machine.

**Sources:**

https://ethereum.org/developers/docs/intro-to-ethereum/
https://ethereum.org/developers/docs/transactions/
</details>

<details>
<summary>How can smart contracts access off-chain data?</summary>

### How can smart contracts access off-chain data?

**Definition:**

The EVM does not read a website. A contract sees the transaction that called it, the state already on the chain, and the block context the protocol exposes. Off-chain facts arrive only if some account puts them into a transaction.

**How it works:**

- The transactions page lists input data as an optional field of arbitrary data. The first four bytes of a contract call select the function. The rest are ABI-encoded arguments. That is how a number or a hash from outside becomes an argument.
- view and pure functions do not change state. Calling them from an externally owned account via eth_call costs no gas. They still cannot fetch a URL. They only read chain state.
- Scaling separates off-chain execution from on-chain data. A rollup executes elsewhere and posts the data to layer 1, where consensus is reached. The layer 1 contract sees the posted data, not the outside server's private disk.
- Someone must pay gas to submit that data. The gas page says every computation, including a call that later reverts, has a cost so the network cannot be stuck in an infinite loop.

**Sources:**

https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/evm/
https://ethereum.org/developers/docs/scaling/
</details>

<details>
<summary>Explain how smart contracts handle exceptions or errors.</summary>

### Explain how smart contracts handle exceptions or errors.

**Definition:**

If execution runs out of gas halfway, the EVM reverts the state changes and still consumes the gas that was provided. A transaction that fails validation before inclusion, such as a simple transfer with a gas limit below 21,000, is rejected and consumes no gas.

**How it works:**

- The gas page separates those two failures. Unused gas below the max fee is refunded. Gas spent before an out-of-gas revert is not.
- The fee is paid whether the transaction succeeds or fails, once it is executed. The formula on that page is gas used times (base fee + priority fee). The base fee is burned. The priority fee is the validator's tip.
- view and pure calls through eth_call do not change state and cost no gas from an externally owned account. An internal call to the same function from another contract does cost gas.
- A revert means the state transition Y(S, T) does not commit that attempt. The EVM page defines the successful transition as old state plus valid transactions producing a new state.

**Sources:**

https://ethereum.org/developers/docs/gas/
https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/evm/
</details>

<details>
<summary>Can you describe a potential security vulnerability in a smart contract?</summary>

### Can you describe a potential security vulnerability in a smart contract?

**Definition:**

The ERC-20 page documents a concrete loss: tokens sent with transfer to a contract that has no way to notice the receipt can sit there forever. The standard does not callback the recipient.

**How it works:**

- transfer and transferFrom move balances even when the recipient is a contract that was not written to hold that token. There is no mandatory receive function.
- The page's practical guard is require(_to != address(this)) so a token cannot be transferred to its own contract address. It also says the deposit pattern for contracts is approve plus transferFrom, not a blind transfer.
- A recipient should still assume tokens can arrive and should provide a way to extract them. The page says you cannot fully prevent the loss on the recipient side.
- ERC-223 and ERC-1363 are named on that page as alternatives that grew out of this hole. ERC-721's safeTransferFrom is a different standard's attempt to check the recipient. It is not part of ERC-20.

**Sources:**

https://ethereum.org/developers/docs/standards/tokens/erc-20/
https://ethereum.org/developers/docs/standards/tokens/erc-721/
</details>

### Ethereum Basics

<details>
<summary>What is Ethereum, and how does it differ from Bitcoin?</summary>

### What is Ethereum, and how does it differ from Bitcoin?

**Definition:**

Ethereum is a blockchain with a computer embedded in it. The intro calls Bitcoin-style chains a distributed ledger of payments. Ethereum is a distributed state machine: accounts, balances, and machine state, updated by transactions everyone re-executes.

**How it works:**

- Ether pays for computation. Part of the fee is burned. The rest goes to the validator who executes and includes the transaction. Validators stake ETH as collateral under proof of stake.
- Bitcoin, in that comparison, enforces payment rules such as not spending more than an address received. Ethereum adds arbitrary programs in state, the smart contracts.
- Consensus is agreement on blocks. Each block references its parent, so changing old data means changing every later block and convincing the network.
- A simple ETH transfer is one transaction type. A contract call is another. Both are signed by an externally owned account.

**Sources:**

https://ethereum.org/developers/docs/intro-to-ethereum/
</details>

<details>
<summary>Can you describe what Gas is in the context of Ethereum?</summary>

### Can you describe what Gas is in the context of Ethereum?

**Definition:**

Gas measures computational effort. The fee is gas used times the price per unit, paid in ETH, usually quoted in gwei. One gwei is 10^-9 ETH.

**How it works:**

- A simple ETH transfer costs 21,000 gas. The user sets a gas limit. If the limit is below what validation needs, the transaction is rejected and no gas is consumed. If execution runs out mid-way, changes revert and the provided gas is consumed.
- The price splits into a protocol base fee and a priority fee, the tip. The base fee is burned. The tip goes to the validator. maxFeePerGas must cover base fee plus tip. The sender is refunded the difference.
- The base fee moves by at most 12.5% per block, comparing the previous block's gas to a target of half the gas limit. A full block raises the next base fee. An empty one lowers it.
- Unused gas under the cap is returned. The fee is charged whether execution succeeds or fails.

**Sources:**

https://ethereum.org/developers/docs/gas/
https://ethereum.org/developers/docs/transactions/
</details>

<details>
<summary>What is the Ethereum Virtual Machine (EVM), and what is its role?</summary>

### What is the Ethereum Virtual Machine (EVM), and what is its role?

**Definition:**

The EVM is the virtual machine every Ethereum node runs so contract code executes the same way everywhere. It is a stack machine. Each stack item is a 256-bit word. The stack is 1024 items deep.

**How it works:**

- The state transition is Y(S, T) = S'. A valid old state plus valid transactions produce a new state. Transactions are either message calls or contract creations.
- Execution uses transient memory, a byte array that does not persist between transactions. Transient storage, TSTORE and TLOAD, lasts for the whole transaction, including internal calls, and is cleared at the end. It is not written into the account's storage trie.
- Persistent contract data lives in a Merkle Patricia storage trie for that account. Opcodes include ordinary stack math and chain-specific ones such as ADDRESS, BALANCE, and BLOCKHASH.
- Gas meters each opcode so a loop cannot run without bound. Implementations must follow the Yellow Paper. Clients include an EVM. The docs also name standalone ones such as evmone and revm.

**Sources:**

https://ethereum.org/developers/docs/evm/
https://ethereum.org/developers/docs/gas/
</details>

<details>
<summary>How is the state of the Ethereum blockchain maintained?</summary>

### How is the state of the Ethereum blockchain maintained?

**Definition:**

Ethereum state is one structure every node stores: accounts, balances, and contract storage. The EVM page says it is a modified Merkle Patricia Trie, folded into a single root hash kept on the chain.

**How it works:**

- A transaction is a signed request to change that state. The transition is Y(S, T) = S'. Nodes re-execute the transactions in a block and must arrive at the same new state.
- Contract storage is its own Merkle Patricia trie on the account. Transient storage from TSTORE lasts one transaction and is not part of that trie. Memory does not survive the transaction.
- Proof of stake selects a validator who has staked ETH to propose the block. Other validators check it. The intro says rewards and penalties push validators to stay honest and online.
- The parent hash chains blocks. Changing an old state means changing every later block and getting the network to agree.

**Sources:**

https://ethereum.org/developers/docs/evm/
https://ethereum.org/developers/docs/intro-to-ethereum/
</details>

<details>
<summary>Explain what ERC-20 and ERC-721 tokens are.</summary>

### Explain what ERC-20 and ERC-721 tokens are.

**Definition:**

ERC-20 is the fungible token standard: each unit is interchangeable. ERC-721 is the non-fungible standard: each token has a tokenId, and the pair of contract address plus tokenId is unique.

**How it works:**

- ERC-20 methods on the docs include totalSupply, balanceOf, transfer, transferFrom, approve, and allowance, plus Transfer and Approval events. name, symbol, and decimals are part of the same listing.
- ERC-721 methods include balanceOf, ownerOf, transferFrom, safeTransferFrom, approve, setApprovalForAll, getApproved, and isApprovedForAll. Events are Transfer, Approval, and ApprovalForAll.
- An ERC-20 balance is a count. An ERC-721 balance is how many distinct token ids an address holds. ownerOf returns the address for one id.
- The ERC-20 page warns that transfer into a contract that does not handle receipts can strand tokens. ERC-721's safeTransferFrom is the other standard's checked transfer, not an ERC-20 method.

**Sources:**

https://ethereum.org/developers/docs/standards/tokens/erc-20/
https://ethereum.org/developers/docs/standards/tokens/erc-721/
</details>

### Blockchain Data Structures & Design

<details>
<summary>Explain the structure of a blockchain block and its contents.</summary>

### Explain the structure of a blockchain block and its contents.

**Definition:**

A block is a batch of transactions plus a link to the parent block. The intro says Ethereum blocks generally hold dozens to hundreds of transactions, and each block cryptographically references its parent.

**How it works:**

- Transactions inside are signed instructions: from, to, signature, nonce, value, input data, gas limit, and the EIP-1559 fee fields on a type 2 transaction.
- A validator who has staked ETH proposes the block. Other validators check it. The transactions page then moves the block from included, to justified, to finalized.
- The state root is the Merkle Patricia commitment to the state after those transactions. The EVM page says the whole state reduces to that root stored on the chain.
- Type 3 transactions can also carry blob data for rollups. That is part of the transaction payload the block includes, not a second chain.

**Sources:**

https://ethereum.org/developers/docs/intro-to-ethereum/
https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/evm/
</details>

<details>
<summary>What is a Merkle tree, and why is it important in blockchain?</summary>

### What is a Merkle tree, and why is it important in blockchain?

**Definition:**

Ethereum's state is a modified Merkle Patricia Trie. Every account is linked by hashes, and the whole structure reduces to one root hash stored on the chain. Change one account and the root changes.

**How it works:**

- The EVM page says that root is how nodes agree on one state without comparing every account byte for byte in the block header.
- Contract storage is its own Merkle Patricia trie on the account. It is part of the global state, unlike transient memory and unlike TSTORE data that dies at the end of the transaction.
- A block also references its parent. The intro says data in a block cannot change without changing every later block, because each block commits to the previous one.
- A proof of one account is a path of hashes up to the root. These pages name the trie and the root. They do not spell a Bitcoin-style list of transaction hashes.

**Sources:**

https://ethereum.org/developers/docs/evm/
https://ethereum.org/developers/docs/intro-to-ethereum/
</details>

<details>
<summary>How can blockchain systems ensure scalability and performance?</summary>

### How can blockchain systems ensure scalability and performance?

**Definition:**

Ethereum's scaling page says the goal is more transactions and faster finality without dropping decentralization or security. The path it describes is rollups, not a larger single block for everyone to re-execute.

**How it works:**

- On-chain, sharding the chain into pieces for subsets of validators was the old plan. The page says rollups took over, helped by blob data on layer 1 that validators can check cheaply.
- A rollup executes transactions off layer 1 and posts the data to layer 1. Optimistic rollups assume validity and run a fraud proof on challenge. Zero-knowledge rollups submit a validity proof.
- State channels let parties transact off-chain and settle on mainnet. Sidechains are separate chains with their own consensus, linked by a bridge. They do not inherit mainnet security the way a rollup does.
- Gas is the pressure valve. When demand is high, tips rise. Layer 2 exists, the gas page says, to bring those costs down.

**Sources:**

https://ethereum.org/developers/docs/scaling/
https://ethereum.org/developers/docs/gas/
</details>

<details>
<summary>Describe the process of adding a new block to a blockchain.</summary>

### Describe the process of adding a new block to a blockchain.

**Definition:**

On Ethereum a validator who has staked ETH is selected to propose a block of pending transactions. Other validators check it. Honest participation is paid. Dishonest stake can be destroyed.

**How it works:**

- Users first broadcast signed transactions. The transactions page says each one lands in a pool of pending transactions until a validator includes it.
- Inclusion is not the last word. The block later becomes justified and then finalized. The page says a finalized block would take a network-level attack costing many billions to rewrite.
- The block references its parent, so it extends one history. The intro says every node must agree on the chain.
- The producer is not a separate company in the docs. It is a validator running the client, with ETH as collateral.

**Sources:**

https://ethereum.org/developers/docs/intro-to-ethereum/
https://ethereum.org/developers/docs/transactions/
</details>

<details>
<summary>What considerations are involved in designing a blockchain architecture?</summary>

### What considerations are involved in designing a blockchain architecture?

**Definition:**

The scaling page states the design constraint directly: more throughput and faster finality must not be bought by making it hard to run a node. Centralizing execution is the failure mode those pages argue against.

**How it works:**

- Decide what is consensus-critical. On Ethereum the EVM state transition is. Off-chain execution is allowed when the data or a proof comes back to layer 1, which is what a rollup does.
- Decide the fee market. EIP-1559 splits a burned base fee from a tip so demand moves the base fee by at most 12.5% a block instead of a pure auction.
- Decide account model and finality. Ethereum uses accounts, balances, and a nonce, and it has justified then finalized blocks. A ledger that only checks that a coin was received is the Bitcoin comparison on the intro page.
- Decide who may propose a block. Proof of stake requires staked ETH. The intro says that collateral is what makes a free flood of fake identities expensive.

**Sources:**

https://ethereum.org/developers/docs/scaling/
https://ethereum.org/developers/docs/intro-to-ethereum/
https://ethereum.org/developers/docs/gas/
</details>

### Transactions and UTXOs

<details>
<summary>Explain the concept of transactions within a blockchain.</summary>

### Explain the concept of transactions within a blockchain.

**Definition:**

An Ethereum transaction is a signed instruction from an externally owned account. It is the only way that account asks the network to change state.

**How it works:**

- The transactions page lists the fields: from, to, signature, nonce, value in wei, input data, gasLimit, maxPriorityFeePerGas, and maxFeePerGas. Contract accounts do not send transactions.
- Three shapes: a plain transfer, a deployment with no to address and bytecode in the data, and a call whose to is a contract. The first four bytes of calldata select the function. The rest are ABI arguments.
- A type 2 transaction is the EIP-1559 form. Type 3 carries blobs. Type 0 is the legacy form. The type byte comes from EIP-2718.
- view and pure functions read state. From an externally owned account, eth_call does that without gas. A transaction that changes state always pays gas.

**Sources:**

https://ethereum.org/developers/docs/transactions/
</details>

<details>
<summary>What is a UTXO, and how is it different from an account/balance model?</summary>

### What is a UTXO, and how is it different from an account/balance model?

**Definition:**

Ethereum keeps accounts and balances in state. The intro says a Bitcoin address cannot spend more than it has previously received. That received-and-not-yet-spent output is the UTXO idea. Ethereum does not track coins that way.

**How it works:**

- An Ethereum account has a balance and a nonce. A transfer debits the sender and credits the recipient inside one transaction. The nonce increments so the same account sequence cannot be applied twice.
- A UTXO system consumes whole previous outputs and creates new ones. There is no mutable balance field. The intro's Bitcoin rule is that check: you cannot spend more than was received.
- Ethereum contract storage is an account's trie, not a set of coin outputs. ERC-20 balances are numbers inside a contract, another account-shaped table.
- The nonce is the account model's replay lock. A UTXO model locks a spend by consuming the output, so it cannot be consumed again.

**Sources:**

https://ethereum.org/developers/docs/intro-to-ethereum/
https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/evm/
</details>

<details>
<summary>How are fees determined in a blockchain transaction?</summary>

### How are fees determined in a blockchain transaction?

**Definition:**

On Ethereum the fee is gas used times (base fee + priority fee), paid in ETH. A simple transfer uses 21,000 gas.

**How it works:**

- The base fee is set by the protocol from how full the previous block was, and it moves at most 12.5% per block. It is burned. The priority fee is the tip the sender adds so a validator prefers this transaction.
- maxFeePerGas is the cap. It must cover base fee plus tip. The difference is refunded. Gas left unused under the gas limit is also refunded.
- A gas limit too low for a simple transfer rejects the transaction before inclusion and consumes nothing. Running out of gas during contract execution reverts the state and consumes the gas.
- The transactions page's example at a base fee of 190 gwei and a tip of 10 gwei is (190 + 10) * 21000 = 4,200,000 gwei, or 0.0042 ETH, on top of the value sent.

**Sources:**

https://ethereum.org/developers/docs/gas/
https://ethereum.org/developers/docs/transactions/
</details>

<details>
<summary>Describe the lifecycle of a blockchain transaction from creation to confirmation.</summary>

### Describe the lifecycle of a blockchain transaction from creation to confirmation.

**Definition:**

An externally owned account signs a transaction, the network pools it, a validator includes it in a block, and the block later becomes justified and then finalized.

**How it works:**

- Signing uses the sender's private key. The transactions page shows Geth and account_signTransaction producing a raw RLP payload and a tx hash. The fields include nonce, gas, fees, to, value, and input.
- Broadcast puts it in the pool of pending transactions. A validator must choose it. A tip makes that more likely. A base-fee-only transaction is valid but easy to skip.
- Inclusion executes it. Success changes state. An out-of-gas execution reverts the changes and still consumes the gas. Unused gas is refunded.
- Finality is the confirmation these pages name. A finalized block, the transactions page says, would take a network-level attack costing many billions to alter.

**Sources:**

https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/gas/
</details>

<details>
<summary>What is a mempool in the context of blockchain?</summary>

### What is a mempool in the context of blockchain?

**Definition:**

The mempool is the pool of pending transactions waiting to be included. The transactions page says a broadcast transaction is added to the pool of all other pending network transactions until a validator picks it.

**How it works:**

- It is not yet state. The EVM state changes only when the transaction is executed inside a block.
- Validators choose. A higher priority fee makes inclusion more likely. Paying only the base fee is valid and still easy to leave waiting.
- The same nonce from one account cannot be applied twice. A later nonce waits on the earlier one. The nonce is the account's transaction counter.
- A transaction can sit in the pool and still fail later if it runs out of gas during execution. Pool admission is not finality.

**Sources:**

https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/gas/
</details>

### Blockchain Security

<details>
<summary>Discuss the concept of the 51% attack and its implications.</summary>

### Discuss the concept of the 51% attack and its implications.

**Definition:**

A 51% attack is a majority of consensus weight rewriting history or censoring blocks. On Ethereum that weight is staked ETH, not a separate hash-power vote. The intro says ETH is the collateral and the weight of votes for proposed blocks.

**How it works:**

- Changing an included block means changing every later block and convincing the network. The parent link is what makes a private rewrite expensive.
- The transactions page says a finalized block could be altered only by a network-level attack costing many billions. Finality is the point after which a merchant can treat the payment as settled.
- Stake is destroyed for dishonest behavior. The intro lists that penalty as one of the three ways ETH secures the chain. A majority that misbehaves risks the collateral, which an identity created for free does not have.
- A majority can still censor in the moment before finality. The defense in these pages is collateral, attestations, and finality, not a claim that censorship is impossible.

**Sources:**

https://ethereum.org/developers/docs/intro-to-ethereum/
https://ethereum.org/developers/docs/transactions/
</details>

<details>
<summary>How can a blockchain prevent double-spending?</summary>

### How can a blockchain prevent double-spending?

**Definition:**

Double-spending is applying the same value twice. Ethereum stops it with one agreed state, an account nonce, and finality. Bitcoin's rule on the intro page is the other form: an address cannot spend more than it has received.

**How it works:**

- The nonce is a counter of transactions from the account. Two transactions with the same nonce cannot both be the next one. The second does not fit the sequence.
- Every node re-executes and must get the same state. A spend that was already applied is not still sitting there as a balance.
- Before inclusion, the spend is only in the pending pool. After finality, the transactions page says rewriting it takes a network-level attack costing many billions.
- ERC-20 balances live in one contract's storage. transfer decreases one balance and increases another in the same execution. A second transfer of the same units fails the balance check inside that contract.

**Sources:**

https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/intro-to-ethereum/
https://ethereum.org/developers/docs/standards/tokens/erc-20/
</details>

<details>
<summary>What are some common security threats to blockchain systems?</summary>

### What are some common security threats to blockchain systems?

**Definition:**

The threats these pages actually document are an underpriced spam of computation, a rewrite of finalized history, stranded ERC-20 tokens, and blind signing of opaque calldata.

**How it works:**

- Gas exists so a caller cannot request an infinite loop for free. A fee is charged even when execution fails. A gas limit caps the work.
- A finalized block still has a stated threat: a network-level attack the transactions page prices at many billions. Before finality, a competing history is the 51% problem: majority stake proposing a different chain.
- ERC-20 transfer does not notify the recipient. Tokens sent to a contract that cannot move them are stuck. The page's guard is to reject transfers to the token contract itself and to use approve plus transferFrom for deposits.
- Calldata is opaque hex. The transactions page calls blind signing the result, and points at ERC-7730 descriptors so a wallet can show a human-readable summary before the signature.

**Sources:**

https://ethereum.org/developers/docs/gas/
https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/standards/tokens/erc-20/
</details>

<details>
<summary>Can you explain how blockchain immutability is maintained?</summary>

### Can you explain how blockchain immutability is maintained?

**Definition:**

Each block references its parent, so editing an old block changes every hash after it. The network has to adopt that new chain. On Ethereum a finalized block is the strong form of that claim.

**How it works:**

- The intro says block data cannot change without changing all subsequent blocks, which requires the consensus of the network.
- The transactions page adds finality: after justified and finalized, altering the block would take a network-level attack costing many billions.
- State is also committed. The Merkle Patricia root in the EVM description changes if any account changes, so a quiet edit of one balance does not match the root the chain already stored.
- Immutability is not a delete button. A later transaction can set a new balance. It does not erase the earlier transaction from the history.

**Sources:**

https://ethereum.org/developers/docs/intro-to-ethereum/
https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/evm/
</details>

<details>
<summary>How does a blockchain network defend against Sybil attacks?</summary>

### How does a blockchain network defend against Sybil attacks?

**Definition:**

A Sybil attack is a pile of free identities pretending to be many participants. Ethereum's defense in the intro is that votes are weighed by staked ETH, and that stake is destroyed if the validator misbehaves.

**How it works:**

- Creating an account is not enough to propose blocks. The intro says anyone who wants to add blocks must stake ETH and run validator software.
- The three roles of ETH on that page are the reward, the collateral, and the weight of votes. A thousand empty accounts do not outweigh one staked validator.
- Gas is the other price on execution. Spamming transactions still pays the base fee, which is burned, plus a tip.
- A sidechain with its own consensus, the scaling page says, does not inherit this. Its Sybil defense is whatever rule that chain chose, not Ethereum's stake.

**Sources:**

https://ethereum.org/developers/docs/intro-to-ethereum/
https://ethereum.org/developers/docs/gas/
https://ethereum.org/developers/docs/scaling/
</details>

### Interoperability and Scalability

<details>
<summary>What strategies exist to improve blockchain scalability?</summary>

### What strategies exist to improve blockchain scalability?

**Definition:**

The scaling page splits the options into on-chain changes and off-chain systems. Rollups are the strategy it says replaced sharding as the main plan.

**How it works:**

- Optimistic rollups assume transactions are valid and check a fraud proof only on a challenge. Zero-knowledge rollups execute off-chain and submit a validity proof. Both post data to layer 1.
- Blob transactions, type 3 on the transactions page, exist so rollups can post that data more cheaply. The scaling page calls this the cheaper data attached to Ethereum blocks.
- State channels settle on mainnet after off-chain updates. Sidechains, validiums, and plasma are separate systems. A validium keeps the proof but not the data on layer 1. A sidechain has its own consensus.
- Raising the gas limit is the blunt on-chain lever. The gas page says the limit can move when validators signal. The scaling page's warning is that a heavier block must still be something an ordinary node can check.

**Sources:**

https://ethereum.org/developers/docs/scaling/
https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/gas/
</details>

<details>
<summary>How can different blockchain networks achieve interoperability?</summary>

### How can different blockchain networks achieve interoperability?

**Definition:**

The scaling page's concrete mechanism is a bridge. A sidechain is an independent chain that runs in parallel and connects to mainnet by a two-way bridge. Assets move by a transaction on each side, not by a shared state trie.

**How it works:**

- A rollup is a tighter link. It posts data or a proof to Ethereum, so the layer 1 contract can accept the result under Ethereum's consensus. That is not a bridge to an unrelated consensus.
- These pages do not describe a shared virtual machine across chains. An ERC-20 on Ethereum is a contract in Ethereum state. A token on another chain is a different contract unless a bridge locks one and mints the other.
- Blind trust in the bridge is the risk these pages leave you with: a sidechain does not inherit mainnet security. The scaling page says sidechains, validiums, and plasma derive security separately.
- Calldata and message formats stay per chain. The ABI the transactions page describes is how Ethereum contracts read arguments. Another chain's encoding is not that ABI.

**Sources:**

https://ethereum.org/developers/docs/scaling/
https://ethereum.org/developers/docs/transactions/
</details>

<details>
<summary>Explain the concept of sidechains and how they work.</summary>

### Explain the concept of sidechains and how they work.

**Definition:**

A sidechain is an independent blockchain that runs beside mainnet, talks to it through a two-way bridge, and uses its own consensus and block parameters. The scaling page says it is EVM-compatible in the Ethereum case, and that it does not derive security from mainnet the way a rollup does.

**How it works:**

- A deposit is a transaction on mainnet that locks or records the asset, and a transaction on the sidechain that credits it. A withdrawal reverses that pair. The two chains do not share one state trie.
- Because consensus is separate, a failure of the sidechain's validators is not an Ethereum finality failure. The page groups sidechains with validiums and plasma as systems whose security is their own.
- An application that needs Ethereum's validator set should be a rollup: execution off layer 1, data or proof on layer 1. A sidechain is the choice when the chain wants its own rules.
- Fees on the sidechain are that chain's fees. Mainnet gas still applies to the bridge transactions.

**Sources:**

https://ethereum.org/developers/docs/scaling/
https://ethereum.org/developers/docs/gas/
</details>

<details>
<summary>What role do layer 2 protocols play in blockchain?</summary>

### What role do layer 2 protocols play in blockchain?

**Definition:**

Layer 2, on the Ethereum scaling page, means off-chain execution that still takes its security from mainnet consensus. Rollups and state channels are in that group. A sidechain is not, because its security is separate.

**How it works:**

- Users submit transactions to layer 2 nodes, often called sequencers or operators. Those nodes batch the transactions and anchor them on layer 1. After that anchor, the page says, the batch is secured by layer 1.
- Optimistic rollups post the data and allow a fraud proof. Zero-knowledge rollups post a validity proof. Blob-carrying type 3 transactions exist to make the data posting cheaper.
- The gas page names layer 2 as the practical way to lower mainnet fees. A rollup folds many user operations into one layer 1 transaction.
- The page warns that some explanations call every off-chain system layer 2. Its own definition is narrower: security has to come from mainnet consensus.

**Sources:**

https://ethereum.org/developers/docs/scaling/
https://ethereum.org/developers/docs/gas/
https://ethereum.org/developers/docs/transactions/
</details>

<details>
<summary>Discuss the trade-offs between scalability, security, and decentralization.</summary>

### Discuss the trade-offs between scalability, security, and decentralization.

**Definition:**

The scaling page states the trade directly: more transactions per second must not come from making nodes so heavy that only a few operators can run them. Security and decentralization are the constraints on the speed.

**How it works:**

- A bigger gas limit raises throughput and raises the cost of running a node. The page says keeping that cost low is what prevents a slide into centralized hardware.
- A rollup moves execution off the path every Ethereum node repeats, and posts data or a proof so security still comes from mainnet. A sidechain moves execution off and also moves security off. That is faster for the sidechain and weaker if its own validators fail.
- A validium keeps data off layer 1 and can aim at very high throughput. The page says it uses validity proofs but does not store the data on mainnet. Withholding that data is the new risk.
- Finality has the same shape. Waiting for the transactions page's finalized status is slower and harder to rewrite than trusting a pending pool entry.

**Sources:**

https://ethereum.org/developers/docs/scaling/
https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/gas/
</details>

### Blockchain Development and Testing

<details>
<summary>What tools and environments are available for blockchain development?</summary>

### What tools and environments are available for blockchain development?

**Definition:**

The pages name a small set of tools, not a full vendor catalog. Contracts are written so the EVM can run their bytecode. Most contracts, the transactions page says, are written in Solidity and read calldata by the ABI.

**How it works:**

- Geth is the client the transactions page shows signing a transaction, via account_signTransaction. The result is a raw RLP transaction and a hash.
- web3.py is the library in the ERC-20 and ERC-721 examples. It builds a contract object from an address and an ABI and calls view methods such as balanceOf, symbol, and ownerOf.
- eth_call runs a view or pure function without a gas-paying transaction. A state-changing call is a signed transaction with a gas limit.
- Standalone EVM implementations named on the EVM page include Py-EVM, evmone, ethereumjs-vm, and revm. They are for executing bytecode, not a hosted test network.

**Sources:**

https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/standards/tokens/erc-20/
https://ethereum.org/developers/docs/evm/
</details>

<details>
<summary>How would you approach testing smart contracts?</summary>

### How would you approach testing smart contracts?

**Definition:**

Test the state transition before you pay mainnet gas for it. eth_call runs a read without a transaction. A state-changing test is a signed transaction on a network whose ETH is not mainnet ETH, then a check that storage matches the expectation.

**How it works:**

- Assert the failure cases the gas page separates. A gas limit below 21,000 on a plain transfer should be rejected with no gas consumed. Running out of gas mid-execution should revert storage and still consume the gas.
- For an ERC-20, call totalSupply, balanceOf, and allowance through the ABI the way the web3.py example does. Then send transfer and transferFrom and read the balances again. Include the documented hole: a transfer to a contract that cannot move the tokens.
- For an ERC-721, ownerOf(tokenId) must change on a transfer, and the pair of contract address plus tokenId stays the identity. safeTransferFrom is the method that checks the recipient.
- A view test is not enough for a bug that only appears when two transactions land in one block. Include them in one test block and re-read the state root's effect through the public getters.

**Sources:**

https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/gas/
https://ethereum.org/developers/docs/standards/tokens/erc-20/
https://ethereum.org/developers/docs/standards/tokens/erc-721/
</details>

<details>
<summary>What is a testnet, and why is it important for blockchain developers?</summary>

### What is a testnet, and why is it important for blockchain developers?

**Definition:**

A testnet is a separate Ethereum-style network whose asset is not mainnet ETH. You use it so a deploy and a failed execution do not spend the ETH the gas page describes, and do not become a finalized mainnet record.

**How it works:**

- Mainnet fees are real: gas used times base fee plus tip, with the base fee burned. A test network can run the same transaction fields, including gasLimit and the EIP-1559 caps, without that mainnet burn.
- Finality on mainnet is the transactions page's justified-then-finalized path. A bug discovered after that is a new transaction, not an edit. Testing first is how you avoid publishing the bug into that history.
- eth_call still works as a local read. It does not replace a test of ordering, gas exhaustion, and events. Those need a block producer and a pool, which a test network provides.
- These Ethereum pages do not name a specific testnet. The property that matters is that the chain is not mainnet.

**Sources:**

https://ethereum.org/developers/docs/gas/
https://ethereum.org/developers/docs/transactions/
</details>

<details>
<summary>Can you explain the difference between on-chain and off-chain coding?</summary>

### Can you explain the difference between on-chain and off-chain coding?

**Definition:**

On-chain code is bytecode the EVM executes as part of a block, paid in gas, and stored in state. Off-chain code is everything else: a web3.py script, a wallet, a rollup sequencer, a channel update that has not been settled.

**How it works:**

- A contract call's first four calldata bytes select the function. That function runs on every node that verifies the block. A Python script using web3.py runs on your machine and only becomes on-chain when it submits a signed transaction.
- view and pure functions are still on-chain code. eth_call executes them without a transaction. An internal call from another contract pays gas.
- A rollup's user transactions execute off layer 1. They become on-chain when the batch or the proof is posted. The scaling page says that post is what lets layer 1 secure them.
- A channel update is off-chain until the settlement transaction. The page describes state channels as multisig contracts that settle finality on mainnet.

**Sources:**

https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/evm/
https://ethereum.org/developers/docs/scaling/
</details>

<details>
<summary>How can you ensure that a blockchain application is GDPR compliant, considering the immutability of data?</summary>

### How can you ensure that a blockchain application is GDPR compliant, considering the immutability of data?

**Definition:**

These Ethereum pages do not state a GDPR procedure. They do state that chain data is not edited in place: changing a block changes every later block and, after finality, would take a network-level attack. The engineering consequence is to keep personal data off that history.

**How it works:**

- Put a commitment on-chain, such as a hash in calldata or in contract storage, and keep the personal record in a store you can delete. The chain then proves the record existed. It does not hold the record.
- A later transaction can replace a storage slot with a new value. The transactions page's lifecycle still leaves the old transaction in the finalized history. Overwriting a slot is not erasure of the earlier calldata.
- Events are part of that history too. The ERC-20 Transfer event includes from, to, and value. Do not index a person's name or a government identifier in an event if you must be able to delete it.
- A rollup that posts transaction data to layer 1 publishes that data where the scaling page says layer 1 will keep it. Posting the personal payload to layer 1 has the same immutability problem as putting it in a mainnet transaction.

**Sources:**

https://ethereum.org/developers/docs/intro-to-ethereum/
https://ethereum.org/developers/docs/transactions/
https://ethereum.org/developers/docs/scaling/
</details>
