# Cryptography — Interview

Answers are filled for every question in this note.

[← Back to hub](./0_system_design.md)

### Related docs

- [Cryptography — Interview](./0_sd_cryptography_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Cryptography](https://devinterview.io/questions/software-architecture-and-system-design/cryptography-interview-questions/).

### Cryptography Fundamentals

<details>
<summary>What is cryptography, and what are its main goals?</summary>

### What is cryptography, and what are its main goals?

**Definition:**

Cryptography is the practice of using math so that secrets stay secret, changes are detectable, and a party can be tied to a message.

**How it works:**

- Confidentiality: encryption turns plaintext into ciphertext that is useless without the key. Decryption reverses it.
- Symmetric encryption uses one shared key. Asymmetric encryption uses a public key to encrypt and a private key to decrypt.
- Integrity: a hash or a MAC shows whether the bytes changed. Encryption alone does not always prove that.
- Authentication: a proof that the other party holds a secret you trust (a key, a certificate).
- Non-repudiation: a signature the signer cannot later deny, because only their private key could have produced it.
- None of this works if keys are weak, reused, or stored in the clear. Key generation, storage, and rotation are part of the system.
- Prefer vetted algorithms and implementations. Inventing a cipher is not a goal.

**Examples:**

A disk image is encrypted with a key so a stolen drive is unreadable (confidentiality). A signed software update fails verification if one byte changes (integrity) and only the vendor’s key validates (authentication and non-repudiation).
</details>

<details>
<summary>Explain the difference between symmetric and asymmetric cryptography.</summary>

### Explain the difference between symmetric and asymmetric cryptography.

**Definition:**

Symmetric cryptography uses one shared secret for both directions. Asymmetric cryptography uses a key pair: a public key anyone may have, and a private key that stays with the owner.

**How it works:**

- Symmetric algorithms (AES, ChaCha20) are fast and suit bulk data. The hard part is getting the same key to both parties without leaking it.
- Asymmetric algorithms (RSA, elliptic curves) are slower and suit small payloads: key exchange, signatures, and encrypting a symmetric key.
- Typical sizes: AES keys are 128 or 256 bits. RSA keys are thousands of bits for similar strength. Elliptic-curve keys are a few hundred bits.
- Real protocols combine them. TLS uses asymmetric cryptography to agree a session key, then symmetric encryption for the bytes.
- Symmetric failure mode: anyone who copies the key can read and forge. Asymmetric failure mode: a lost private key or a trusted public key that actually belongs to an attacker.
- Do not encrypt a large file with RSA directly. Encrypt the file with AES and wrap the AES key.

**Examples:**

Alice and Bob share a 256-bit AES key and encrypt a backup with it. For a first contact, Alice encrypts that AES key with Bob’s RSA public key. Only Bob’s private key unwraps it. After that, the file uses AES.
</details>

<details>
<summary>What is a cryptographic hash function, and what properties must it have?</summary>

### What is a cryptographic hash function, and what properties must it have?

**Definition:**

A cryptographic hash maps any input to a fixed-length digest so that the digest does not reveal the input and is impractical to forge.

**How it works:**

- The output length is fixed (SHA-256 yields 32 bytes) no matter how long the input is.
- Preimage resistance: given a digest, finding an input that hashes to it is infeasible.
- Second-preimage resistance: given an input, finding a different input with the same digest is infeasible.
- Collision resistance: finding any two different inputs with the same digest is infeasible. This is a stronger demand, and the birthday bound makes it the first property to fail as hashes age.
- A small input change should flip about half the output bits. That is the avalanche effect.
- Hashes are not encryption. There is no key and no way to “decrypt.” For a keyed integrity check, use an HMAC.
- MD5 and SHA-1 are broken for collisions. Use SHA-256, SHA-3, or a password hash such as Argon2 when the input is a password.

**Examples:**

A download page publishes the SHA-256 of a file. You hash your copy and compare. A matching digest means you almost certainly have the same bytes. It does not by itself prove who published the digest. That takes a signature.
</details>

<details>
<summary>Describe the concept of public key infrastructure (PKI).</summary>

### Describe the concept of public key infrastructure (PKI).

**Definition:**

PKI is the set of roles and documents that let you trust a public key belongs to a named party, not merely that some key exists.

**How it works:**

- Each party has a key pair. The private key stays secret. The public key is published.
- A certificate binds a public key to a name, signed by a certificate authority (CA). Trust comes from already trusting that CA, often through a chain up to a root in your trust store.
- Registration checks the name before the CA signs. Revocation (CRL or OCSP) marks a certificate unusable before it expires.
- You then get confidentiality (encrypt to the public key), integrity and authentication (verify a signature), and non-repudiation when the private key stayed with the owner.
- On the web this is TLS certificates for site names. The same pattern covers signed email, code signing, and VPN client certs.
- A CA that signs the wrong name breaks the system. Pinning, short lifetimes, and certificate transparency exist because of that.

**Examples:**

A browser trusts a root CA. That CA signed an intermediate, which signed `shop.example`. The TLS handshake presents the chain. The browser checks the signatures, the name, and the dates, then uses the leaf public key. A self-signed cert for the same name fails because no trusted CA vouches for it.
</details>

<details>
<summary>What is a digital signature, and how does it work?</summary>

### What is a digital signature, and how does it work?

**Definition:**

A digital signature is a value produced with a private key over a hash of a message. Anyone with the public key can check that the message was not altered and that the private key took part.

**How it works:**

- The signer hashes the message and applies the private key to the digest (RSA, ECDSA, Ed25519).
- The verifier hashes the message again and checks the signature with the public key. A mismatch means a different message or a different key.
- Hashing first keeps the signature small and covers every byte.
- The public key has to be the right person’s. A certificate or a previously exchanged key supplies that. The signature does not by itself name a human.
- This is stronger than a scanned “electronic” signature image, which anyone can copy and which no math checks.
- Keep the private key offline or in a hardware module. A copied private key lets someone else sign as you.

**Examples:**

A release file is hashed with SHA-256 and signed with the vendor’s Ed25519 private key. Your package tool hashes the file it downloaded and verifies the signature. One flipped bit fails the check. A signature from any other key also fails.
</details>

<details>
<summary>Can you explain what a nonce is and how it’s used in cryptography?</summary>

### Can you explain what a nonce is and how it’s used in cryptography?

**Definition:**

A nonce is a number used once in a cryptographic operation so that the same inputs do not produce a repeatable result an attacker can replay or correlate.

**How it works:**

- It does not have to be secret. It does have to be unique for a given key. A counter or a large random value both work if you never repeat the pair.
- In AES-GCM and ChaCha20-Poly1305, repeating a nonce with the same key leaks plaintext and can break the authenticator.
- Protocols put a nonce in a handshake so an old captured message cannot be sent again as a fresh one.
- Hashed constructions mix a nonce or salt in so equal passwords do not hash equal, and so precomputed tables miss.
- Store the nonce beside the ciphertext. The decrypting side needs the same value. Losing uniqueness is worse than storing it in the clear.

**Examples:**

```text
ciphertext = AES-GCM(key, nonce=counter, plaintext)
counter = counter + 1
```

Each message under that key gets the next counter. Resetting the counter to 0 after a restart, with the same key, reuses nonces and is unsafe.
</details>

<details>
<summary>What does it mean for a cryptographic algorithm to be “computationally secure”?</summary>

### What does it mean for a cryptographic algorithm to be “computationally secure”?

**Definition:**

Computationally secure means breaking the algorithm takes more work than any realistic attacker can spend, not that breaking it is impossible in principle.

**How it works:**

- Information-theoretic security (a one-time pad used correctly) stays secret even against unlimited computing. Almost nothing else does.
- Computational security assumes a bound: so many operations, so much memory, so many years. A 128-bit key is secure because searching the key space is beyond that bound.
- “Secure” is always against a stated attack and a stated margin. A hash can be fine against preimages and weak against collisions.
- The bound moves. Hardware and new cryptanalysis shrink it. That is why key sizes grow and algorithms get retired.
- A computationally secure primitive still fails if the mode, the nonce, or the key handling is wrong. The definition is about the algorithm under its stated rules.
- Pick primitives whose bounds are published and reviewed. A secret design has no public bound to trust.

**Examples:**

AES-128 is treated as computationally secure: a brute-force search of the key is not a plan anyone can carry out. A one-time pad is the stricter idea, perfect secrecy, and it demands a fresh random key as long as the message. Using AES with a repeated nonce is not “AES is insecure.” It is a misuse outside the bound.
</details>

<details>
<summary>Describe the concept of perfect secrecy and name an encryption system that achieves it.</summary>

### Describe the concept of perfect secrecy and name an encryption system that achieves it.

**Definition:**

Perfect secrecy means the ciphertext gives an attacker with unlimited computing no new information about the plaintext. Seeing the ciphertext does not change the odds of any candidate message.

**How it works:**

- Claude Shannon’s condition: for every plaintext and ciphertext pair that can occur, the encryption must be consistent with enough keys that the ciphertext does not favor one plaintext.
- That forces the key to be at least as long as the message, chosen uniformly at random, and kept secret.
- The one-time pad does this. Ciphertext is plaintext XOR a random key of the same length. Every plaintext is still possible, with a different key.
- The key is used once. Reuse lets an attacker XOR two ciphertexts and cancel the key.
- The pad must be truly random, not a stretched password. Generating, carrying, and destroying that much key material is why almost every real system uses computational security instead.
- A one-time pad does not authenticate. An attacker can flip bits in the ciphertext and flip the same bits in the plaintext.

**Examples:**

Plaintext bit `1`, key bit `1`, ciphertext bit `0`. The same ciphertext is also plaintext `0` with key `0`. Without the key, both messages remain possible. Use that key on a second message and the two ciphertexts reveal the XOR of the two plaintexts.
</details>

### Encryption Algorithms

<details>
<summary>What are substitution and permutation in the context of encryption algorithms?</summary>

### What are substitution and permutation in the context of encryption algorithms?

**Definition:**

Substitution replaces a unit of plaintext with another unit. Permutation (transposition) reorders units without changing them. Modern ciphers mix both so the result is neither a simple codebook nor a shuffle.

**How it works:**

- Substitution creates confusion: the relationship between key and ciphertext is tangled. An S-box in DES or AES is a fixed substitution of a small group of bits.
- Permutation creates diffusion: a change in one input bit spreads across the output. AES ShiftRows and MixColumns move and mix bytes. A classical transposition cipher only reorders letters.
- Substitution alone keeps letter frequencies (a Caesar shift still shows the same peaks). Permutation alone keeps the letters, so an anagram attack still sees them.
- A product cipher repeats substitute-then-permute for many rounds, with key material mixed in, so local structure does not survive.
- These are teaching ciphers, not things to deploy. They show the two moves. AES is the construction that uses them safely.

**Examples:**

```text
Caesar shift by 1:  cat -> dbu          # substitution
Rail fence, 2 rails: cat -> cta          # c then a then t, reordered
```

Neither hides a short message. AES runs a keyed substitution and a mixing permutation over many rounds so those classroom attacks do not apply.
</details>

<details>
<summary>Explain the basic principle behind the AES encryption algorithm.</summary>

### Explain the basic principle behind the AES encryption algorithm.

**Definition:**

AES encrypts a 128-bit block by repeating a substitution-permutation round, mixing in a key schedule derived from a 128-, 192-, or 256-bit key.

**How it works:**

- The block is a 4 by 4 array of bytes, called the state.
- Each full round does SubBytes (an S-box on every byte), ShiftRows (rotate rows), MixColumns (mix each column), and AddRoundKey (XOR a round key).
- The last round skips MixColumns. The first step is an initial AddRoundKey.
- Key length picks the round count: 10 rounds for 128-bit keys, 12 for 192, 14 for 256. The key schedule expands the key into one round key per round.
- It is a substitution-permutation network, not a Feistel network. Decryption uses inverse steps and the round keys in reverse order.
- AES is a block primitive. Real use needs a mode such as GCM, with a unique nonce, if you want both secrecy and integrity.

**Examples:**

A 16-byte block and a 32-byte key run 14 rounds. The same plaintext under the same key and a new GCM nonce produces different ciphertext. Reusing the nonce with that key is the failure mode, not a short key.
</details>

<details>
<summary>What is the Data Encryption Standard (DES), and why is it considered insecure today?</summary>

### What is the Data Encryption Standard (DES), and why is it considered insecure today?

**Definition:**

DES is a 1970s block cipher: 64-bit blocks, a 56-bit key, and 16 Feistel rounds. The algorithm’s structure held up better than its key length.

**How it works:**

- The 64-bit key includes 8 parity bits, so the secret is 56 bits. Each round expands, substitutes through S-boxes, permutes, and XORs a subkey.
- A 56-bit key can be searched. Purpose-built hardware did that in the late 1990s, and ordinary computing makes it cheaper now.
- The 64-bit block is a second limit. In common modes, enough blocks under one key invite birthday collisions.
- Differential and linear cryptanalysis exist. They are not why DES was retired in practice. The short key is.
- Triple DES runs DES three times to stretch the key, and AES replaces both with a 128-bit block and a longer key.
- Do not use DES for new systems. Treat any DES ciphertext as something to re-encrypt.

**Examples:**

A key space of 2^56 is about 72 quadrillion keys. A cluster that tries billions of keys a second finishes a single-key search in hours to days. AES-128’s space is 2^128, which that approach does not dent.
</details>

<details>
<summary>Describe the differences between RSA and ECC (Elliptic Curve Cryptography).</summary>

### Describe the differences between RSA and ECC (Elliptic Curve Cryptography).

**Definition:**

RSA’s security rests on factoring large integers. Elliptic-curve cryptography rests on a discrete-log problem on a curve. For the same security margin, ECC uses much shorter keys.

**How it works:**

- An RSA public key is an exponent and a modulus that is a product of two large primes. Operations are modular exponentiation. Common sizes are 2048 or 3072 bits.
- ECC uses a point on a named curve. A 256-bit curve key is in the same league as a 3072-bit RSA key for current estimates.
- Shorter keys mean smaller certificates, less bandwidth, and faster private-key operations on small devices. RSA verification with a small public exponent can still be very fast.
- Both can sign and both can wrap a symmetric key. Neither should encrypt bulk data.
- Curve choice matters. Use a reviewed curve (P-256, Curve25519) and a reviewed implementation. A home-made curve is not a shortcut.
- Both fall to a large quantum computer running Shor’s algorithm. That is a reason for post-quantum work, not a reason to prefer RSA’s longer keys today.

**Examples:**

A TLS certificate with a P-256 key carries a much smaller public key than a 3072-bit RSA certificate and still targets a comparable classical security level. Handshake CPU on a phone usually favors the curve.
</details>

<details>
<summary>How does a stream cipher differ from a block cipher?</summary>

### How does a stream cipher differ from a block cipher?

**Definition:**

A stream cipher combines the plaintext with a keystream a unit at a time. A block cipher transforms a fixed-size block, and a mode of operation handles messages that are not one block.

**How it works:**

- A stream cipher (or a block cipher in counter mode) produces a keystream and XORs it with the plaintext. The first byte can be encrypted before the rest exists. There is no padding.
- A block cipher such as AES maps 16 bytes to 16 bytes. ECB encrypts each block alone. CBC, CTR, and GCM define how blocks chain and whether a nonce is required.
- Reusing a stream keystream (same key and nonce) is fatal: XOR of two ciphertexts cancels it. A repeated block under ECB leaks equality of blocks, which is a different leak.
- Bit errors: a flipped ciphertext bit flips the same plaintext bit in a raw stream XOR. In CBC it damages that block and shifts into the next. An AEAD mode rejects the message instead of returning damaged plaintext.
- CTR mode is a block cipher used as a stream. The distinction in practice is often “which mode,” not a separate algorithm.

**Examples:**

ChaCha20 XORs each plaintext byte with the next keystream byte. AES-ECB would encrypt `0123456789abcdef` as one block and repeat the pattern if the next block is identical. AES-GCM hides that repetition and adds a tag, provided the nonce changes every message.
</details>

<details>
<summary>Can you describe the Feistel cipher structure?</summary>

### Can you describe the Feistel cipher structure?

**Definition:**

A Feistel cipher splits each block into two halves and repeats a round that mixes one half into the other with a function of the round key. The function itself does not have to be reversible.

**How it works:**

- Start with left half L and right half R.
- One round sets the new left to the old right, and the new right to the old left XOR F(old right, round key). Some descriptions swap the names. The XOR-and-swap is the idea.
- After the last round the halves are the ciphertext. Decryption runs the same rounds with the round keys in reverse order. F is applied the same way, so F can be a one-way mix.
- More rounds and a strong F raise the work of cryptanalysis. Too few rounds leak structure.
- DES is the famous example: 64-bit blocks, 32-bit halves, 16 rounds, F built from expansion, S-boxes, and a permutation.
- AES is not a Feistel cipher. It uses a substitution-permutation network on the whole state, with invertible steps.

**Examples:**

```text
L, R = split(block)
L, R = R, L xor F(R, k1)
L, R = R, L xor F(R, k2)
```

To undo round 2, XOR F(L, k2) back into R and swap. You never invert F. You only recompute it with the same round key.
</details>

<details>
<summary>What are the key differences between DES and 3DES?</summary>

### What are the key differences between DES and 3DES?

**Definition:**

3DES (Triple DES) applies the DES round structure three times with more key material. It stretches the key. It does not fix the 64-bit block.

**How it works:**

- DES uses one 56-bit key and 16 Feistel rounds on a 64-bit block.
- 3DES runs encrypt, decrypt, encrypt. Two-key 3DES uses two DES keys (about 112 bits). Three-key 3DES uses three (about 168 bits). The middle decrypt step lets a 3DES implementation act as DES when all keys are equal.
- The S-boxes and the Feistel F are the same. The extra strength is the repeated keyed passes, not a new substitution design.
- 3DES is three times the work of DES and still has a 64-bit block. In long-lived modes that birthday bound (the Sweet32 attack on CBC) is a practical limit even when the key is long.
- Standards have retired both for new work. AES is the replacement: a 128-bit block and a key that does not need three passes.
- If you meet 3DES, treat it as a compatibility path to migrate off, not as a peer of AES.

**Examples:**

A payment device that still speaks two-key 3DES can talk to an old peer. A new design uses AES-GCM. Wrapping the same DES key three times (`E-D-E` with K, K, K) is just DES, which is why the three keys have to differ for the extra strength to exist.
</details>

<details>
<summary>Explain the main security features of the RSA algorithm.</summary>

### Explain the main security features of the RSA algorithm.

**Definition:**

RSA is a public-key construction. The public key is a modulus and an exponent. The private key is the matching private exponent, or the prime factors that make that exponent usable.

**How it works:**

- RFC 8017 defines an RSA public key as two components: the modulus n and the public exponent e. In a valid key, n is a product of u distinct odd primes, u at least 2, and e is an integer between 3 and n - 1 with GCD(e, lambda(n)) = 1. The first two primes are conventionally p and q.
- The private key may be the pair (n, d), the private exponent, or the factors and CRT coefficients (p, q, dP, dQ, qInv, and further triplets if there are more primes). Those two representations are the ones the RFC specifies.
- Encryption and signature are different schemes on those primitives. The RFC specifies RSAES-OAEP and RSAES-PKCS1-v1_5 for encryption, and separate signature schemes. Do not use the raw modular exponent as the whole scheme.
- Security depends on the private factors staying secret and on using the scheme the RFC specifies, including its padding. Publishing n and e is the point of the public key. Publishing d or the primes breaks it.
- This note does not describe how to recover factors or how to break a padding check. Validation belongs to an implementation of the RFC, not to an attack procedure.

**When to use:**

Use the public key to encrypt to one holder or to check a signature. Use the private key only on the holder. Prefer the OAEP encryption scheme the RFC specifies over inventing a padding.

**Sources:**

- https://www.rfc-editor.org/rfc/rfc8017#section-3.1
</details>

<details>
<summary>What is quantum cryptography, and how might it impact current encryption methods?</summary>

### What is quantum cryptography, and how might it impact current encryption methods?

**Definition:**

Quantum cryptography uses properties of quantum states, most often to agree a shared key. The impact that NIST is standardizing for is different: a future quantum computer may defeat many of today’s widely used public-key systems.

**How it works:**

- Quantum key distribution relies on the fact that measuring a quantum state disturbs it, so an on-path observer can be noticed while the key is being established. It does not replace a symmetric cipher by itself.
- NIST’s post-quantum project treats large quantum computers as a future threat to current public-key cryptography, not as a reason to abandon symmetric encryption wholesale.
- In August 2024 NIST published three principal standards: FIPS 203 (ML-KEM, key establishment), FIPS 204 (ML-DSA, signatures), and FIPS 205 (SLH-DSA, hash-based signatures).
- Those standards are meant to be deployed now. Under NIST IR 8547, quantum-vulnerable algorithms are to be removed from NIST standards by 2035, with high-risk systems moving earlier.
- Falcon and HQC were selected for further standardization as additional options. They are not the three standards already published.

**When to use:**

Use a published PQC scheme when you are migrating key establishment or signatures off algorithms NIST calls quantum-vulnerable. Keep a vetted symmetric cipher for bulk data, with the key sizes your current standard already requires.

**Sources:**

- https://csrc.nist.gov/projects/post-quantum-cryptography
</details>

### Cryptanalysis

<details>
<summary>What is cryptanalysis?</summary>

### What is cryptanalysis?

**Definition:**

Cryptanalysis is the study of mathematical techniques for attempting to defeat cryptographic techniques and information-system security, including looking for errors or weaknesses in an algorithm or in its implementation. It also covers operations that defeat cryptographic protection without an initial knowledge of the key.

**How it works:**

- NIST SP 800-57 Part 1 Rev. 5 uses that definition. The work is analysis of the design or the implementation, not a normal use of the key.
- A weakness may sit in the algorithm itself or only in how a product implements it. Both are in scope for the term.
- Interview answers stay at this level: name the kind of weakness (short key, bad mode, leaky implementation) and the impact (the protection no longer holds). They do not walk through a procedure.
- Designers answer cryptanalysis by using standardized algorithms, published schemes, and implementations that have been reviewed against those schemes.

**Sources:**

- https://csrc.nist.gov/glossary/term/cryptanalysis
</details>

<details>
<summary>What is a brute force attack, and how can systems be protected against it?</summary>

### What is a brute force attack, and how can systems be protected against it?

**Definition:**

A brute-force search tries keys from the key space until one works. The protection is to make that search impractical, not to hide which algorithm you used.

**How it works:**

- The cost tracks the size of the key space. A longer key is a larger space, which is why retired short-key ciphers are not a match for AES.
- For passwords, NIST SP 800-63B requires verifiers to store a salted password hash from a password hashing scheme that also takes a cost factor. The cost factor should be as high as practical, and raised over time as computers get faster, so each guess is expensive.
- The same publication treats a password as a memorized secret sent over an authenticated protected channel. Limiting guesses at the verifier is part of keeping online guessing impractical.
- This note does not describe how to search a key space or a password file.

**When to use:**

Prefer a standardized cipher with a modern key length for data, and a salted password hash with a high cost factor for memorized secrets. A fast general-purpose hash is the wrong tool for a password.

**Sources:**

- https://pages.nist.gov/800-63-4/sp800-63b.html
</details>

<details>
<summary>Describe a man-in-the-middle attack and how it can be prevented.</summary>

### Describe a man-in-the-middle attack and how it can be prevented.

**Definition:**

A manipulator in the middle sits on the path between two parties and can read or change what they exchange. Prevention is to authenticate the peer, not to hope the path is private.

**How it works:**

- MDN describes TLS as a client/server protocol that uses cryptography so a third party cannot eavesdrop on or tamper with the messages. The server must present a valid digital certificate that confirms its identity before the secure connection is established.
- Both sides can authenticate if each presents its own certificate. That is mutual authentication.
- The `Secure` attribute on `Set-Cookie` sends the cookie only on `https:` (except localhost). MDN states that this makes the cookie more resistant to a manipulator in the middle. `Secure` does not by itself stop a script from reading the cookie if `HttpOnly` is absent.
- This note does not describe how to insert a party onto a path.

**When to use:**

Use TLS with a certificate the client can check whenever the bytes cross a network you do not control. Add `Secure` and `HttpOnly` on cookies that must not travel in the clear or be readable from page script.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/TLS
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie
</details>

<details>
<summary>Explain what a side-channel attack is.</summary>

### Explain what a side-channel attack is.

**Definition:**

A side-channel attack is enabled by leakage from a physical or deployed cryptosystem. NIST lists timing, power consumption, and electromagnetic and acoustic emissions as characteristics that can be used.

**How it works:**

- The cipher’s math can be sound while the implementation still leaks. The leak is a measurement of how the device behaves, not a field in the ciphertext.
- NIST SP 800-63-4, SP 800-227, and SP 1800-21B all use that framing: timing, power, electromagnetic emissions, and acoustic emissions.
- FIPS 140-3 includes non-invasive security among the areas a cryptographic module must address, alongside physical security and self-tests.
- This note does not describe how to measure a device or how to turn a measurement into a key.

**When to use:**

Treat constant-time, reviewed implementations and a validated module as the response when the key is valuable enough that a lab measurement matters. Algorithm choice alone does not close a side channel.

**Sources:**

- https://csrc.nist.gov/glossary/term/side_channel_attack
- https://csrc.nist.gov/pubs/fips/140-3/final
</details>

<details>
<summary>What is a chosen plaintext attack?</summary>

### What is a chosen plaintext attack?

**Definition:**

A chosen-plaintext setting is one in which the analyst can obtain ciphertext for plaintexts they select. A confidentiality scheme is expected to keep the plaintext hidden even in that setting.

**How it works:**

- NIST defines cryptanalysis as the study of techniques for attempting to defeat cryptographic protection, including weaknesses in an algorithm or its implementation. Chosen plaintext is one of the models used in that study.
- The analyst’s extra power is access to an encryption operation, not knowledge of the key. The security claim is that this access still does not reveal other plaintexts.
- RSA in this note is not raw modular exponentiation. RFC 8017 specifies RSAES-OAEP and RSAES-PKCS1-v1_5 as the encryption schemes. The padding is part of the scheme, which is why interviews reject “encrypt the message as m^e mod n” as the whole design.
- This note does not describe how to choose plaintexts or how to use the resulting ciphertexts.

**Sources:**

- https://csrc.nist.gov/glossary/term/cryptanalysis
- https://www.rfc-editor.org/rfc/rfc8017#section-3.1
</details>

<details>
<summary>How does a frequency analysis attack work against certain ciphers?</summary>

### How does a frequency analysis attack work against certain ciphers?

**Definition:**

Frequency analysis matters for ciphers that preserve the frequency of symbols, such as a simple substitution in which one letter always becomes the same other letter. The ciphertext then still carries the language’s letter frequencies.

**How it works:**

- A classical substitution cipher maps each symbol to one symbol. Common letters stay common, only under a different name. That is the property the analysis uses.
- A modern block cipher does not keep that property. AES, covered earlier in this note, mixes substitution with a keyed permutation across the whole block, so a single letter’s frequency does not appear as a single ciphertext symbol’s frequency.
- The impact is limited to ciphers that leak symbol frequency. It is not a property of AES, and it is not a procedure this note will spell out.
- If a design still encrypts letter by letter with a fixed substitution, it has this weakness regardless of how the alphabet is scrambled.

**Sources:**

- https://csrc.nist.gov/glossary/term/cryptanalysis
</details>

### Hash Functions and Digital Signatures

<details>
<summary>What is the difference between HMAC and a simple hash function?</summary>

### What is the difference between HMAC and a simple hash function?

**Definition:**

A hash function maps a message to a digest with no secret. HMAC is a message authentication code: the same kind of hash, plus a shared secret key, so only a holder of the key can produce or check the tag.

**How it works:**

- RFC 2104 defines HMAC as a MAC built from any iterative cryptographic hash, denoted H, and a secret shared key. The strength depends on the properties of that hash.
- The RFC’s goals are to use existing hash functions unchanged, keep their performance, handle keys simply, rest the analysis on assumptions about H, and swap H when a better hash appears.
- The RFC’s 1997 examples are HMAC-MD5 and HMAC-SHA1. Those names illustrate the construction. A current call is Node’s `crypto.createHmac(algorithm, key)`, for example `createHmac('sha256', key)`, then `update` and `digest`.
- A bare hash is public. Anyone who has the message can recompute it, so it does not authenticate the sender. HMAC does, because the key is required on both sides.
- Compare tags with a constant-time compare. Node’s `crypto.timingSafeEqual` requires the two buffers to have the same length.

**When to use:**

Use a hash when you need a fingerprint and no one has to prove they hold a key. Use HMAC when two parties share a secret and must detect a changed message.

**Examples:**

```js
import { createHmac } from 'node:crypto';

const tag = createHmac('sha256', key).update(message).digest('hex');
```

**Sources:**

- https://www.rfc-editor.org/rfc/rfc2104.html
- https://nodejs.org/api/crypto.html#cryptocreatehmacalgorithm-key-options
</details>

<details>
<summary>Explain the concept of a collision in hash functions and why it is significant.</summary>

### Explain the concept of a collision in hash functions and why it is significant.

**Definition:**

A collision is two different inputs that a hash function maps to the same digest. It matters because a digital signature is a hash of the data followed by a signature function: a collision means one signature can verify for a different message.

**How it works:**

- NIST SP 800-107 describes a digital signature as those two functions in that order. Origin authentication, integrity, and non-repudiation are claims about the message that was hashed.
- If a second message hashes to the same digest, the signature check succeeds for the second message as well. The private key never touched that second message.
- A cryptographic hash is expected to make finding such a pair impractical. A hash that does not is the wrong function under a signature.
- This note does not describe how to search for a colliding pair.

**Sources:**

- https://csrc.nist.gov/glossary/term/digital_signature
</details>

<details>
<summary>Describe the birthday attack and its relevance to hash functions.</summary>

### Describe the birthday attack and its relevance to hash functions.

**Definition:**

The birthday bound is the point at which a collision between two hash outputs becomes plausible: on the order of the square root of the number of possible digests, not after a search of every digest. For an n-bit hash that is about 2^(n/2) evaluations.

**How it works:**

- The name comes from the same counting as the birthday paradox: matches show up much earlier than a scan of the whole space.
- The impact on hash choice is direct. A 128-bit collision bound needs a hash whose output is about 256 bits, which is why SHA-256 is the usual answer when that bound is the requirement.
- RFC 6962’s Merkle tree separates leaf hashes and inner hashes (a `0x00` prefix on a leaf, a `0x01` prefix on an inner node) and says that domain separation is required to give second-preimage resistance. That is a concrete place where hash collisions are a design constraint, not an afterthought.
- This note does not describe a search that looks for the collision.

**Sources:**

- https://www.rfc-editor.org/rfc/rfc6962#section-2.1
- https://csrc.nist.gov/glossary/term/digital_signature
</details>

<details>
<summary>How does a digital signature guarantee the integrity and authenticity of a message?</summary>

### How does a digital signature guarantee the integrity and authenticity of a message?

**Definition:**

A digital signature is the result of a cryptographic transformation of data. When it is implemented properly, NIST says it provides origin authentication, data integrity, and signatory non-repudiation. It does not provide confidentiality.

**How it works:**

- NIST SP 800-63-4 describes the operation as asymmetric: the private key signs, and the public key verifies.
- NIST SP 800-107 describes the transformation as two functions in order: a hash of the data, then a signature function on that hash. A changed message hashes differently, so verification fails. That is the integrity check.
- Authenticity is the check that the public key which verifies the signature belongs to the signer you meant. The signature does not by itself tell you whose key that is; a certificate or another trusted binding does.
- Non-repudiation is the claim that only the private-key holder could have produced the signature. NIST also notes that signatures do not provide replay protection.
- This note does not describe how to forge a signature.

**Sources:**

- https://csrc.nist.gov/glossary/term/digital_signature
</details>

<details>
<summary>What are RSA signatures, and how do they differ from RSA encryption?</summary>

### What are RSA signatures, and how do they differ from RSA encryption?

**Definition:**

RSA signatures and RSA encryption use the same kind of key pair and different schemes. RFC 8017 specifies RSASSA-PSS and RSASSA-PKCS1-v1_5 for signatures, and RSAES-OAEP and RSAES-PKCS1-v1_5 for encryption.

**How it works:**

- A signature uses the private key to sign and the public key to verify. NIST states that this gives authenticity, integrity, and non-repudiation, and not confidentiality.
- Encryption uses the public key so that only the private-key holder can read the message. That is confidentiality for one recipient, not a signature.
- The public key in RFC 8017 is the modulus n and the public exponent e. The private key is the private exponent d, or the prime factors and CRT coefficients. Publishing n and e is the point. Publishing d or the primes is not.
- Do not treat raw modular exponentiation as either scheme. The RFC specifies the padding and the encoding for each scheme, and the two jobs are not interchangeable.
- Prefer the OAEP encryption scheme and the PSS signature scheme the RFC specifies when you are choosing among the schemes it lists.

**When to use:**

Sign when you need integrity and origin. Encrypt when you need confidentiality to the holder of the private key. Use a separate key pair for each job.

**Sources:**

- https://www.rfc-editor.org/rfc/rfc8017#section-3.1
- https://csrc.nist.gov/glossary/term/digital_signature
</details>

<details>
<summary>Explain the importance of using a secure hash algorithm for digital signatures.</summary>

### Explain the importance of using a secure hash algorithm for digital signatures.

**Definition:**

NIST describes a digital signature as a hash of the data followed by a signature function. The signature checks the hash, so the hash has to be a function for which a second message with the same digest is impractical.

**How it works:**

- Integrity fails open if a second message collides with the signed hash: verification succeeds for a message the private key never signed.
- The birthday bound puts that collision near 2^(n/2) evaluations for an n-bit digest. A hash whose output is too short does not meet a collision requirement even when the signature math is fine.
- RFC 2104 already treated the hash as replaceable, and its 1997 examples (MD5, SHA-1) are not what you put under a new signature. Node’s `createHmac('sha256', key)` shows the modern habit: name a current hash explicitly.
- XML Signature 1.1, in a related standard, called out additional hash algorithms as a conformance change for the same reason: the hash under the signature is part of the security claim.

**When to use:**

Hash with a current approved function such as SHA-256 or stronger, then sign that digest with the signature scheme. Do not sign with MD5 or SHA-1 in a new design.

**Sources:**

- https://csrc.nist.gov/glossary/term/digital_signature
- https://www.w3.org/TR/xmldsig-core1/#sec-Overview
</details>

<details>
<summary>What is a Merkle tree, and how is it used in cryptography?</summary>

### What is a Merkle tree, and how is it used in cryptography?

**Definition:**

A Merkle hash tree commits a list of entries to one root hash. RFC 6962 uses a binary Merkle hash tree so a log can be audited efficiently: a leaf can be shown to sit under the signed root without sending every entry.

**How it works:**

- The hash in that RFC is SHA-256. The input is an ordered list of data entries. Those entries are hashed to form the leaves. The output is a single 32-byte Merkle Tree Hash.
- An empty list hashes as SHA-256 of an empty string. A single entry (a leaf) is SHA-256 of `0x00` concatenated with the entry. For a longer list, the root is SHA-256 of `0x01` concatenated with the hashes of the left and right parts. The RFC says this domain separation is required for second-preimage resistance.
- The tree need not be a full power of two. Its shape is fixed by the number of leaves.
- A Merkle audit path is the shortest list of extra nodes needed to recompute the root from one leaf. If that root matches the signed root, the leaf is in the tree.
- Certificate Transparency uses this so a client can check inclusion. The same shape appears wherever a signed root must cover many items.

**Examples:**

A log signs one root. A client that holds one certificate and the audit path recomputes the root and compares it to the signed root. The client does not need every certificate in the log.

**Sources:**

- https://www.rfc-editor.org/rfc/rfc6962#section-2.1
</details>

### Key Management and Protocols

<details>
<summary>What is key distribution, and what challenges does it present?</summary>

### What is key distribution, and what challenges does it present?

**Definition:**

Key distribution is getting keying material to the parties who must use it, with the protection that type of key requires. NIST SP 800-57 Part 1 Rev. 5 is the general guidance: security services, algorithms, key types, and the protection each type needs.

**How it works:**

- A secret or private key must stay confidential on the way. A public key must stay unmodified, because a substituted public key sends data to the wrong holder or verifies the wrong signer.
- Symmetric encryption needs the same secret at both ends before any ciphertext is useful. That shared secret is the distribution problem.
- Asymmetric encryption publishes the public key and keeps the private key at one holder. Distribution then becomes an integrity problem for the public key, which is what a certificate is for.
- Diffie-Hellman, below, agrees a shared secret without sending that secret. It still needs the parties to authenticate each other, or the agreed secret is with the wrong peer.
- SP 800-57 discusses the functions of key management and the issues that show up when cryptography is actually used. Generation, storage, and who may see each key are part of distribution, not a later cleanup.

**Sources:**

- https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final
</details>

<details>
<summary>Describe Diffie-Hellman key exchange and its primary use.</summary>

### Describe Diffie-Hellman key exchange and its primary use.

**Definition:**

Diffie-Hellman lets two parties agree a shared secret. Each keeps a private value and sends a public value. The secret is computed from one side’s private value and the other side’s public value. It is not sent on the wire.

**How it works:**

- Node’s `crypto.createDiffieHellman(prime[, primeEncoding][, generator][, generatorEncoding])` builds a key-exchange object from a prime and an optional generator. The generator defaults to 2.
- `crypto.diffieHellman({ privateKey, publicKey })` computes the shared secret. Both keys must be the same asymmetric type and must support DH or ECDH. The return value is a `Buffer`.
- The primary use is key agreement: the shared secret becomes keying material for a symmetric cipher, so the long-term problem is no longer “ship the symmetric key.”
- Agreement is not authentication. TLS 1.3 uses (EC)DHE with a pre-shared key when it wants forward secrecy together with shared keys. A PSK used alone, the RFC says, drops forward secrecy for the application data.
- This note does not include parameters chosen so that the exchange is weak, and it does not describe how to recover a private value.

**When to use:**

Use DH or ECDH when two parties need a fresh shared secret and can authenticate the public values. Use the Node functions only with keys that already support DH or ECDH.

**Sources:**

- https://nodejs.org/api/crypto.html#cryptocreatediffiehellmanprime-primeencoding-generator-generatorencoding
- https://www.rfc-editor.org/rfc/rfc8446#section-1.2
</details>

<details>
<summary>Explain what a key escrow is and its purpose in cryptography.</summary>

### Explain what a key escrow is and its purpose in cryptography.

**Definition:**

Key escrow, in the NIST glossary, is the retention of the private component of the key pair associated with a subscriber’s encryption certificate, so that the key can be recovered.

**How it works:**

- The purpose is recovery. If the holder loses the private decryption key, retained material can still decrypt data that was encrypted to that certificate.
- The glossary ties the retained component to an encryption certificate. A signature key is a different object: escrow of a signing key would let someone else produce signatures, which is not the recovery goal stated there.
- Escrow is a policy and a storage problem. SP 800-57 Part 1 requires each type of keying material to be protected as that type requires. Escrowed private keys need the same confidentiality as any other private decryption key, plus a rule for who may ask for recovery.
- It is not a substitute for a backup you do not control. It is an intentional copy, held so recovery is possible.

**When to use:**

Use escrow when the organization must be able to decrypt data after the holder is gone, and when policy allows a copy of that decryption key. Do not escrow a signing key in order to decrypt.

**Sources:**

- https://csrc.nist.gov/glossary/term/key_escrow
- https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final
</details>

<details>
<summary>How do certificate authorities help in securing communications over the internet?</summary>

### How do certificate authorities help in securing communications over the internet?

**Definition:**

A certificate authority binds a name to a public key by signing that binding. TLS then uses the certificate so the client can confirm the server’s identity before the secure session.

**How it works:**

- MDN states that a TLS server must provide a valid digital certificate confirming its identity in order to establish a secure connection. Without that check, the client cannot tell which public key belongs to the name it asked for.
- A digital signature, per NIST, gives origin authentication and integrity when the private key signs and the public key verifies. The CA’s signature on the certificate is that operation applied to the name and the subject public key.
- The client already trusts the CA’s public key (or a chain up to one it trusts). Checking the signature fails if the certificate was altered.
- Both parties may present certificates. MDN calls that mutual authentication.
- The CA does not encrypt the traffic. TLS does that after the identity check. The CA’s job is the binding the check relies on.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/TLS
- https://csrc.nist.gov/glossary/term/digital_signature
</details>

<details>
<summary>What is forward secrecy, and what protocols use this property?</summary>

### What is forward secrecy, and what protocols use this property?

**Definition:**

Forward secrecy means a later compromise of a long-term key does not reveal earlier session keys. NIST’s glossary defines perfect forward secrecy for IPsec as a new secret key created and shared through a new Diffie-Hellman exchange for each IPsec security association.

**How it works:**

- The IPsec wording in NIST SP 800-77 Rev. 1 is specific: each security association gets its own Diffie-Hellman secret, so an old key cannot be used against newer keys that still protect integrity and confidentiality.
- TLS 1.3 states the same property for pre-shared keys. A PSK used with (EC)DHE key exchange provides forward secrecy in combination with the shared keys. A PSK used alone loses forward secrecy for the application data.
- The session key is what encrypts the data. If that key was produced by a fresh Diffie-Hellman and then discarded, a later copy of the long-term key does not recompute it.
- RSA key transport of a session key does not have this property: the long-term private key can still decrypt the transported session key.

**When to use:**

Prefer a fresh Diffie-Hellman (or ECDH) per session when recorded traffic must stay confidential even if a long-term key is later copied. In TLS 1.3, that means (EC)DHE, including with a PSK, rather than a PSK alone.

**Sources:**

- https://csrc.nist.gov/glossary/term/perfect_forward_secrecy
- https://www.rfc-editor.org/rfc/rfc8446#section-1.2
</details>

### Authentication and Access Control

<details>
<summary>Explain the role of cryptography in user authentication.</summary>

### Explain the role of cryptography in user authentication.

**Definition:**

Authentication shows that a claimant controls an authenticator bound to the subscriber. NIST SP 800-63B requires approved cryptographic techniques at Authentication Assurance Level 2 and above, and uses cryptography so a verifier can check that control without receiving a copy of a long-term private key.

**How it works:**

- A password is “something you know.” It is sent over an authenticated protected channel. The verifier stores a salted hash, not the password. That hash is a cryptographic primitive used so a stolen store is not the password list.
- Single-factor and multi-factor cryptographic authenticators prove possession of a key. The output is generally some type of signed message. At AAL3, multi-factor cryptographic authenticators must use public-key cryptography so the verifier’s copy of the check does not expose the authentication secret.
- TLS supplies the authenticated protected channel those passwords and protocol messages travel in. MDN requires the server certificate before that channel exists.
- Authentication is not authorization. Cryptography here answers “who controls this authenticator,” not “what that identity may do.”

**Sources:**

- https://pages.nist.gov/800-63-4/sp800-63b.html
- https://developer.mozilla.org/en-US/docs/Glossary/TLS
</details>

<details>
<summary>What is multi-factor authentication, and how is cryptography involved?</summary>

### What is multi-factor authentication, and how is cryptography involved?

**Definition:**

Multi-factor authentication requires proof of possession and control of two distinct authentication factors. NIST SP 800-63B defines Authentication Assurance Level 2 that way, and requires approved cryptographic techniques there.

**How it works:**

- AAL2 allows either one multi-factor authenticator or a combination of two single-factor authenticators. A multi-factor authenticator needs two factors for a single authentication event, such as a cryptographic device that will not run until a biometric sensor activates it.
- Permitted multi-factor types at AAL2 include a multi-factor out-of-band authenticator, a multi-factor one-time password, and multi-factor cryptographic authentication.
- Multi-factor cryptographic authentication proves control of a key that is activated by a second factor. The output is generally a signed message. The key is “something you have”; the activation factor is “something you know” or “something you are.”
- At AAL3 those authenticators must use public-key cryptography so compromise of the verifier does not reveal the authentication secret.
- A password alone is one factor, “something you know,” and NIST says passwords are not phishing-resistant. Adding a second factor is what moves the event to AAL2.

**When to use:**

Use a multi-factor authenticator, or two distinct factors, when the account needs AAL2 or higher. Use public-key cryptographic authentication when the verifier must not hold a copy of the authentication secret.

**Sources:**

- https://pages.nist.gov/800-63-4/sp800-63b.html
</details>

<details>
<summary>Describe the OAuth 2.0 protocol and its use cases in authentication.</summary>

### Describe the OAuth 2.0 protocol and its use cases in authentication.

**Definition:**

OAuth 2.0 is an authorization framework. RFC 6749 says it lets a third-party application obtain limited access to an HTTP service, either on behalf of a resource owner or on its own behalf. It obsoletes the OAuth 1.0 protocol in RFC 5849.

**How it works:**

- The resource owner approves the access. The application receives a limited grant, not the owner’s password.
- “On its own behalf” is the other case in the abstract: the application is the client, and there is no end-user approval step for that client’s own access.
- OAuth is not itself a login protocol. Authentication of the user is what the approval step relies on. OpenID Connect and similar layers add an identity token on top; RFC 6749’s job is the limited access.
- Cryptography shows up in the HTTPS channel those HTTP requests use, and in any token that must be integrity-protected. The RFC’s subject is the authorization grant, not a new cipher.
- RFC 8252, RFC 8996, and RFC 9700 are later updates listed on the RFC 6749 page. Read those before treating every 2012 grant type as still recommended.

**When to use:**

Use OAuth 2.0 when an application needs limited access to an HTTP API without collecting the resource owner’s password. Do not treat the access token as proof of identity unless a profile you have actually adopted says it is.

**Sources:**

- https://www.rfc-editor.org/rfc/rfc6749#section-1.1
</details>

<details>
<summary>Explain the concept of role-based access control (RBAC) and its cryptographic requirements.</summary>

### Explain the concept of role-based access control (RBAC) and its cryptographic requirements.

**Definition:**

Role-based access control decides what a subject may do from a role, not from the subject’s individual identity. NIST SP 800-53 Rev. 5 describes a role as a collection of access authorizations a user receives by assuming that role. A role may apply to one person or to several.

**How it works:**

- NIST SP 800-95, citing the OASIS XACML profile, says permitted actions are identified with roles rather than with individual subject identities. Permissions may be inherited through a role hierarchy.
- RBAC is an authorization model. It does not encrypt data and it does not define a cipher.
- The cryptographic piece sits in front of the role check and around the data that carries the role. The subject must be authenticated (NIST SP 800-63B). A token or assertion that states the role needs integrity, which is a signature or a MAC, or the role can be rewritten in transit.
- Confidentiality of the resource is a separate decision. RBAC can allow a role to read a field that is still encrypted to a different key.

**When to use:**

Use RBAC when many people share the same job and you want to grant the job, not each person. Authenticate the subject and protect the integrity of the role statement. Do not expect the role model itself to be a cryptographic algorithm.

**Sources:**

- https://csrc.nist.gov/glossary/term/role_based_access_control
- https://pages.nist.gov/800-63-4/sp800-63b.html
</details>

### Cryptography in Software and Applications

<details>
<summary>How does HTTPS utilize cryptography to secure communications over the web?</summary>

### How does HTTPS utilize cryptography to secure communications over the web?

**Definition:**

HTTPS is HTTP over TLS. TLS is the protocol that gives the HTTP messages confidentiality and integrity, and that checks the server’s identity with a certificate.

**How it works:**

- RFC 8446 says TLS lets client/server applications communicate over the Internet in a way designed to prevent eavesdropping, tampering, and message forgery.
- MDN states that the server must provide a valid digital certificate confirming its identity before the secure connection is established. That certificate is how the client knows which public key belongs to the name.
- The session that follows uses the keys the handshake agreed. TLS 1.3 can combine a pre-shared key with (EC)DHE so the application data has forward secrecy. A PSK alone does not.
- Cookies set over that channel should carry `Secure`, so the browser sends them only on `https:`. `HttpOnly` keeps page script from reading them. Those are HTTP attributes on top of TLS, not a second encryption layer.
- Major browsers removed TLS 1.0 and 1.1 in early 2020. MDN says the server needs TLS 1.2 or 1.3.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/TLS
- https://www.rfc-editor.org/rfc/rfc8446#section-1.2
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie
</details>

<details>
<summary>What is a TLS/SSL certificate, and how does it establish a secure session?</summary>

### What is a TLS/SSL certificate, and how does it establish a secure session?

**Definition:**

A TLS certificate is the digital certificate a TLS peer presents to confirm its identity. MDN says every modern browser requires the server to provide a valid one before a secure connection is established. SSL is the former name of the protocol; the certificate’s job did not change when the protocol was renamed TLS.

**How it works:**

- The certificate carries the server’s public key and an issuer’s signature over that binding. Checking the signature with a trusted issuer’s public key is what makes the binding trustworthy. NIST’s definition of a digital signature is the operation: private key signs, public key verifies, and the result supports origin authentication and integrity.
- After the identity check, the handshake agrees the keys that protect the application data. RFC 8446 designs TLS to prevent eavesdropping, tampering, and message forgery. The certificate does not itself encrypt the HTTP body.
- The client may also present a certificate. MDN calls that mutual authentication.
- A certificate the client cannot chain to a trust anchor does not establish the session the browser will accept, even if the bytes on the wire are otherwise well formed.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/TLS
- https://csrc.nist.gov/glossary/term/digital_signature
- https://www.rfc-editor.org/rfc/rfc8446#section-1.2
</details>

<details>
<summary>Explain how cryptography is used in blockchain technology.</summary>

### Explain how cryptography is used in blockchain technology.

**Definition:**

A blockchain uses hash chaining so a change in an earlier block changes every later hash, signatures so only the key holder authorizes a transaction, and often a Merkle tree so one root commits to many transactions.

**How it works:**

- A hash link means each block includes the digest of the previous block. Changing an earlier block changes that digest, so the chain of hashes no longer matches. That is the integrity property from the hash, the same property a digital signature relies on when it signs a digest.
- NIST describes a signature as a private-key operation that supports origin authentication, integrity, and non-repudiation, and not confidentiality. A transaction signature is that operation: verifiers use the public key, and the signer cannot later claim a different message under the same signature unless the hash collides.
- RFC 6962 shows the Merkle shape used for a signed root over many leaves. A light client recomputes the root from one leaf and an audit path and compares it to the signed root.
- Confidentiality is not what those three primitives provide. A public chain publishes the transactions. Encryption is a separate choice when the payload must stay private.

**Sources:**

- https://csrc.nist.gov/glossary/term/digital_signature
- https://www.rfc-editor.org/rfc/rfc6962#section-2.1
</details>

<details>
<summary>Describe the use of cryptographic primitives in password hashing.</summary>

### Describe the use of cryptographic primitives in password hashing.

**Definition:**

A password hashing scheme turns a password into a stored verifier. NIST SP 800-63B says verifiers must salt and hash passwords with a scheme that takes a password, a salt, and a cost factor, so a stolen file does not give the password and each guess is expensive.

**How it works:**

- The salt makes two equal passwords store as different hashes. The cost factor is what makes each guess slow. NIST says the cost factor should be as high as practical without hurting verifier performance, and it should be raised over time as computers get faster.
- A fast hash such as a bare SHA-256 is the wrong primitive. It has no cost factor, so a stolen file can be tested at the speed of the hash.
- Node’s `crypto.pbkdf2(password, salt, iterations, keylen, digest, callback)` is one such function: an HMAC digest is applied for `iterations` passes to derive `keylen` bytes. The Node documentation says the salt should be random and at least 16 bytes, and iterations should be a positive number set as high as possible.
- The password still travels to the verifier. NIST requires an authenticated protected channel for that request. Hashing replaces storage, not TLS.
- This note does not describe how to test a stolen hash file.

**When to use:**

Use a password hashing scheme with a per-password salt and a high cost factor for any centrally verified password. Use PBKDF2 from `node:crypto` when that is the approved function you have adopted, with a random salt of at least 16 bytes.

**Examples:**

```js
import { pbkdf2 } from 'node:crypto';

pbkdf2(password, salt, iterations, 64, 'sha512', (err, derivedKey) => {
  if (err) throw err;
  // store salt and derivedKey, not the password
});
```

**Sources:**

- https://pages.nist.gov/800-63-4/sp800-63b.html
- https://nodejs.org/api/crypto.html#cryptopbkdf2password-salt-iterations-keylen-digest-callback
</details>

<details>
<summary>How do secure cookies work, and what cryptographic measures are involved?</summary>

### How do secure cookies work, and what cryptographic measures are involved?

**Definition:**

A cookie is a name-value pair the server sends in `Set-Cookie` and the browser sends back later. The cryptographic measures are the HTTPS channel that carries it, plus attributes that keep it off unencrypted requests and away from page script.

**How it works:**

- MDN defines `Set-Cookie` as the response header that sends the cookie. `Secure` means the cookie is sent only on `https:` (localhost excepted), which MDN says makes it more resistant to a manipulator in the middle.
- `HttpOnly` forbids JavaScript from reading the cookie through `Document.cookie`. The cookie is still sent on `fetch` and `XMLHttpRequest`. MDN says this mitigates cross-site scripting against the cookie.
- `SameSite` controls whether the cookie is sent on cross-site requests. `Strict` sends it only for same-site requests. `Lax` also sends it on top-level navigations that use a safe method. `None` requires `Secure`.
- `__Host-` cookies must be set with `Secure` from HTTPS, must not set `Domain`, and must set `Path=/`. That keeps the cookie on the host that set it.
- None of these attributes encrypt the cookie value. TLS encrypts it on the wire. A value that must stay secret from the browser’s script also needs `HttpOnly`.

**When to use:**

Set `Secure`, `HttpOnly`, and an explicit `SameSite` on a session cookie. Use the `__Host-` prefix when the cookie must be host-only and path `/`.

**Examples:**

```http
Set-Cookie: sessionId=38afes7a8; Secure; HttpOnly; SameSite=Lax; Path=/
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie
</details>

### Advanced Topics

<details>
<summary>Explain the principles behind homomorphic encryption and its potential applications.</summary>

### Explain the principles behind homomorphic encryption and its potential applications.

**Definition:**

Fully homomorphic encryption computes on encrypted data. NIST’s privacy-enhancing cryptography project describes it as computing over encrypted data without learning the plaintext input or output, while still applying the intended function. The result is ciphertext of the result.

**How it works:**

- The worker applies the function to ciphertexts. It does not decrypt. The holder of the decryption key obtains the plaintext of the function’s output, not a view of the intermediate values.
- NIST groups FHE with other privacy-enhancing tools (zero-knowledge proofs, secure multi-party computation) whose point is to let parties interact without revealing extraneous private information.
- The same page points at HomomorphicEncryption.org as an open effort toward standardizing fully homomorphic encryption. This note does not name a parameter set or a library call that page does not specify.
- Potential uses NIST lists around these tools include statistics over distributed data and delegating computation to a party who must not see the plaintext.

**When to use:**

Consider FHE when a party must compute on data it is not allowed to see in the clear, and when the cost of that computation is acceptable. For a joint function over several parties’ inputs, NIST’s description of multi-party computation is the closer match.

**Sources:**

- https://csrc.nist.gov/projects/privacy-enhancing-cryptography
</details>

<details>
<summary>What is zero-knowledge proof, and how does it maintain privacy?</summary>

### What is zero-knowledge proof, and how does it maintain privacy?

**Definition:**

A zero-knowledge proof lets a prover convince a verifier that a statement is true without providing any more information than that one bit: true rather than false. NISTIR 8301 states it that way. NIST’s privacy-enhancing cryptography page adds the usual interview form: prove knowledge of a secret solution without revealing the solution.

**How it works:**

- The verifier learns that the statement holds. The verifier does not learn the witness, the secret that makes the statement true.
- Privacy is that gap. The proof is not a copy of the secret and not an encryption of the secret for the verifier to decrypt.
- NIST lists identification and authentication among the application areas, including a credential that proves a predicate on private data (for example, that a person is of voting age) while the rest of the data stays hidden.
- The glossary definition is the security claim. It is not a circuit, a curve, or a proof system this note will instantiate.

**When to use:**

Use a zero-knowledge proof when the verifier needs to accept a fact and must not receive the underlying secret. If the verifier must learn the result of a function on several parties’ private inputs, multi-party computation is the closer NIST description.

**Sources:**

- https://csrc.nist.gov/glossary/term/zero_knowledge_proof
- https://csrc.nist.gov/projects/privacy-enhancing-cryptography
</details>

<details>
<summary>Describe the concept of secure multi-party computation.</summary>

### Describe the concept of secure multi-party computation.

**Definition:**

Secure multi-party computation lets several parties jointly compute a function over inputs that stay distributed, without each party revealing its input. NIST describes the result as what a trusted third party would have computed: correct outputs, and no sharing of the inputs.

**How it works:**

- The parties are often mutually distrustful. The protocol replaces the trusted party. Each party learns the agreed output, not the other parties’ inputs.
- NIST’s privacy-enhancing cryptography project lists MPC next to fully homomorphic encryption and zero-knowledge proofs. FHE is one party computing on ciphertext. MPC is several parties, each holding an input.
- One example on that page is a calculation whose data sits at multiple sources that cannot release the data. The function still runs, and the sources do not publish their rows.
- NIST is also gathering reference material on threshold schemes, and it treats multi-party computation as the baseline paradigm for those schemes. This note does not specify a protocol, a round count, or a library.

**When to use:**

Use MPC when the inputs belong to different parties and none of them should see the others’ inputs, but all of them need the function’s result. If a single worker can compute on ciphertext alone, FHE matches that shape more closely.

**Sources:**

- https://csrc.nist.gov/projects/privacy-enhancing-cryptography
</details>

<details>
<summary>Explain the purpose of post-quantum cryptography.</summary>

### Explain the purpose of post-quantum cryptography.

**Definition:**

Post-quantum cryptography is cryptography meant to stay secure against a future quantum computer. NIST’s project exists to protect information from machines that may be years or decades away and that could defeat many of today’s widely used public-key systems.

**How it works:**

- The algorithms are classical: they run on today’s computers. “Post-quantum” describes the threat they are designed against, not a quantum device you must deploy.
- In August 2024 NIST released three principal standards. FIPS 203 is ML-KEM, a key-encapsulation mechanism. FIPS 204 is ML-DSA, a lattice-based signature. FIPS 205 is SLH-DSA, a stateless hash-based signature.
- NIST says those three should be put into use now and will be the foundation for most deployments. Falcon (signatures) and HQC (key encapsulation) were selected for further standardization.
- Under the timeline in NIST IR 8547, NIST will remove quantum-vulnerable algorithms from its standards by 2035. High-risk systems are expected to move earlier. Migration means finding where the vulnerable algorithms are used and replacing them.
- This is not quantum key distribution. QKD uses quantum states to agree a key. PQC replaces the public-key algorithms a quantum computer would threaten.

**When to use:**

Start with ML-KEM for key establishment and ML-DSA or SLH-DSA for signatures when you are leaving a quantum-vulnerable public-key algorithm. Inventory first, then replace.

**Sources:**

- https://csrc.nist.gov/projects/post-quantum-cryptography
</details>

### Standards and Protocols

<details>
<summary>What is the NIST, and what role does it play in cryptography?</summary>

### What is the NIST, and what role does it play in cryptography?

**Definition:**

NIST is the National Institute of Standards and Technology, a U.S. federal laboratory. In cryptography it publishes Federal Information Processing Standards and Special Publications, and it runs the programs that select and validate algorithms and modules.

**How it works:**

- FIPS are the standards federal agencies use for cryptographic modules and for algorithms NIST has standardized. FIPS 140-3 is the current module standard. FIPS 203, 204, and 205 are the principal post-quantum standards released in August 2024.
- Special Publications such as SP 800-57 Part 1 Rev. 5 give key-management guidance, and SP 800-63B gives authenticator requirements, including how passwords are stored.
- The Cryptographic Module Validation Program, a joint effort with the Canadian Centre for Cyber Security, validates modules against those FIPS so agencies have a metric when they buy equipment.
- The post-quantum project ran an international competition and now tells organizations to migrate. NIST’s role there is selection, publication, and a transition timeline, not operation of anyone’s production keys.
- The glossary and the publications are the interview sources. NIST does not replace a vendor’s implementation; it says which algorithms and which module requirements a federal system must meet.

**Sources:**

- https://csrc.nist.gov/projects/post-quantum-cryptography
- https://csrc.nist.gov/projects/cryptographic-module-validation-program
- https://csrc.nist.gov/pubs/fips/140-3/final
</details>

<details>
<summary>How do cryptographic modules and algorithms get validated (e.g., FIPS 140-2)?</summary>

### How do cryptographic modules and algorithms get validated (e.g., FIPS 140-2)?

**Definition:**

A cryptographic module is validated under the Cryptographic Module Validation Program against FIPS 140-3, which superseded FIPS 140-2. Independent laboratories test the module. CMVP reviews the submission and, on success, lists the module.

**How it works:**

- FIPS 140-3, published 22 March 2019, defines four increasing qualitative levels. The areas include module specification, interfaces, roles, services, and authentication, software and firmware security, the operating environment, physical security, non-invasive security, management of sensitive security parameters, self-tests, life-cycle assurance, and mitigation of other attacks.
- CMVP is joint work by NIST and the Canadian Centre for Cyber Security. Testing laboratories are accredited by NVLAP. CMVP began accepting FIPS 140-3 submissions on 22 September 2020. It stopped accepting FIPS 140-2 submissions for new certificates; the last new-certificate date in the program table is 22 September 2021, with a narrower exception through 1 April 2022.
- A validated module is Active for 5 years, or 2 years for an interim validation. FIPS 140-2 modules may still be used for new systems through 21 September 2026. After that they move to the Historical list and are only for existing systems.
- For U.S. federal agencies, if cryptography is required, CMVP states that non-validated cryptography is treated as providing no protection: the data is unprotected plaintext. A revoked module may no longer be used.

**When to use:**

Buy or build to FIPS 140-3 when a federal system must cryptographically protect information. Treat a FIPS 140-2 module as a legacy validation with the 21 September 2026 limit for new systems.

**Sources:**

- https://csrc.nist.gov/pubs/fips/140-3/final
- https://csrc.nist.gov/projects/cryptographic-module-validation-program
</details>

<details>
<summary>Explain the importance and usage of the Common Criteria for Information Technology Security Evaluation.</summary>

### Explain the importance and usage of the Common Criteria for Information Technology Security Evaluation.

**Definition:**

Common Criteria is the governing document that provides a comprehensive, rigorous method for specifying security function and assurance requirements for products and systems. That is the NIST glossary definition, drawn from CNSSI 4009-2022.

**How it works:**

- The method separates what the product claims to do (security functions) from how strongly that claim was examined (assurance). A buyer can ask for a given set of requirements instead of trusting a vendor’s brochure.
- It is a product-evaluation framework. It is not a cipher and not FIPS 140-3. FIPS 140-3 and CMVP validate a cryptographic module. Common Criteria evaluates an IT product against a stated security target.
- NIST SP 800-53 Rev. 5 and SP 800-37 Rev. 2 point at the same glossary entry when they need a name for that evaluation method. Usage is procurement and assurance: require an evaluation against a protection profile or a security target when the buyer needs a third-party examination of the product’s security claims.
- This note does not list evaluation assurance levels, because the glossary page does not define them.

**When to use:**

Use Common Criteria when you need a rigorous statement of a product’s security functions and of the assurance applied to that statement. Use CMVP when the question is specifically whether a cryptographic module meets FIPS 140-3.

**Sources:**

- https://csrc.nist.gov/glossary/term/common_criteria
</details>

<details>
<summary>Describe the purpose of the Transport Layer Security (TLS) protocol and its predecessor SSL.</summary>

### Describe the purpose of the Transport Layer Security (TLS) protocol and its predecessor SSL.

**Definition:**

TLS lets a client and a server communicate over the Internet in a way designed to prevent eavesdropping, tampering, and message forgery. RFC 8446 specifies TLS 1.3. MDN states that TLS was formerly called SSL.

**How it works:**

- Both are client/server protocols. The server presents a valid digital certificate so the client can confirm its identity. Mutual authentication is possible when the client presents a certificate too.
- TLS 1.3 obsoletes RFC 5246 (TLS 1.2’s previous base, as cited by RFC 8446) in the sense that RFC 8446 is the current protocol specification and also sets new requirements for TLS 1.2 implementations. MDN says major browsers removed TLS 1.0 and 1.1 in early 2020, so a server needs TLS 1.2 or 1.3.
- Forward secrecy in TLS 1.3 comes from (EC)DHE. A pre-shared key used with (EC)DHE keeps it. A pre-shared key used alone loses it for the application data.
- SSL is the predecessor name. New work speaks TLS. The purpose did not change: an authenticated channel that hides and protects the bytes.

**When to use:**

Use TLS 1.3 where both peers support it, and TLS 1.2 where you still need that generation. Do not design for SSL or for TLS 1.0 or 1.1.

**Sources:**

- https://www.rfc-editor.org/rfc/rfc8446#section-1.2
- https://developer.mozilla.org/en-US/docs/Glossary/TLS
</details>

<details>
<summary>What is the S/MIME standard, and what is it used for?</summary>

### What is the S/MIME standard, and what is it used for?

**Definition:**

S/MIME is Secure/Multipurpose Internet Mail Extensions. RFC 8551 defines version 4.0 as a consistent way to send and receive secure MIME data. It obsoletes RFC 5751.

**How it works:**

- Digital signatures provide authentication, message integrity, and non-repudiation with proof of origin.
- Encryption provides data confidentiality.
- Compression can be used to reduce data size. It is a third operation the RFC lists, not a security service.
- The protected object is MIME data, so the standard fits email and any other MIME body, not a TLS session and not a raw packet.
- The signature and encryption services match the NIST split: a signature does not make the body confidential, and encryption does not by itself prove who sent it. S/MIME can apply both.

**When to use:**

Use S/MIME when the message is MIME and you need signed origin, integrity, or confidentiality of that message as it sits in a mailbox or is forwarded. Use TLS when you need the transport protected, including messages that are not MIME.

**Sources:**

- https://www.rfc-editor.org/rfc/rfc8551#section-1
</details>

### Practical Implementation and Best Practices

<details>
<summary>When should you use hardware-based cryptographic modules such as HSMs in an application?</summary>

### When should you use hardware-based cryptographic modules such as HSMs in an application?

**Definition:**

Use a hardware cryptographic module when the private or secret key must stay inside a module that has been validated for physical security, interfaces, and management of sensitive security parameters. An HSM is that kind of module: the key is used on the device instead of being loaded into ordinary process memory as an exportable file.

**How it works:**

- FIPS 140-3’s requirements include physical security, non-invasive security, roles and authentication, self-tests, and management of sensitive security parameters. Those are the properties you are buying, not a faster CPU.
- CMVP says that for U.S. federal agencies, if cryptography is required, non-validated cryptography is treated as no protection. A validated module is the metric agencies use when they procure equipment.
- The application calls the module for signing or decryption. The private key does not need to leave the module for those operations.
- A software library on a general-purpose server is the right default for low-value development keys. It is the wrong place for a key whose copy would let someone else sign or decrypt production data.

**When to use:**

Use an HSM or another validated hardware module for production signing keys, decryption keys that must survive a host compromise, and any federal system that must meet FIPS 140-3. Prefer a module with a current FIPS 140-3 validation over a new deployment that only has FIPS 140-2.

**Sources:**

- https://csrc.nist.gov/pubs/fips/140-3/final
- https://csrc.nist.gov/projects/cryptographic-module-validation-program
</details>

<details>
<summary>What are the best practices for managing cryptographic keys within an enterprise?</summary>

### What are the best practices for managing cryptographic keys within an enterprise?

**Definition:**

Manage keys by the type of key and the security service it provides. NIST SP 800-57 Part 1 Rev. 5 is the general recommendation: which algorithms and key types to use, what protection each type of keying material requires, and which management functions the organization has to operate.

**How it works:**

- Name the service first. Confidentiality, integrity, and origin authentication are different services, and NIST’s glossary does not treat a signature key as a confidentiality key. Use separate keys for signing and for encryption. RFC 8017 already splits RSA signature schemes from RSA encryption schemes.
- Protect each type as that type requires. Secret and private keys need confidentiality. Public keys need integrity, which is what a certificate provides. SP 800-57 is where those protection requirements are specified.
- Generate and store high-value private keys in a module validated under CMVP when the data must be cryptographically protected for a federal system. CMVP treats non-validated cryptography as providing no protection in that case.
- Distribution is part of management. A shared symmetric key has to reach the other party without being copied by anyone else. A public key has to arrive unmodified.
- Passwords are not general-purpose keys. SP 800-63B requires a salted password hash with a cost factor, on an authenticated channel. Do not store them next to encryption keys as raw strings.
- Plan for recovery of encryption keys only when policy allows escrow or backup of that specific key. Do not copy a signing key in order to decrypt.

**Sources:**

- https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final
- https://csrc.nist.gov/projects/cryptographic-module-validation-program
- https://pages.nist.gov/800-63-4/sp800-63b.html
</details>
