# MultiSig (MPC) Wallet

## Our Solution: BLS Aggregation

Our solution leverages BLS aggregation, which is compatible with the `p2_delegated_puzzle_or_hidden_puzzle`, the most important smart contract in Chia. This approach combines the efficiency and security of BLS signatures with the robust infrastructure of Chia's ecosystem.

However, it is crucial to address potential vulnerabilities, especially the **Rogue Public Key Attack**. To mitigate such risks, users must generate their public keys independently to prevent potential forgery. The security of this scheme relies heavily on truly independent key generation processes and proper management of synthetic keys, which are critical for maintaining overall security.

## Technical Definition and Security Analysis

### Mathematical Definition

A multisig scheme using BLS aggregation consists of four main functions: **generate**, **sign**, **aggregate**, and **verify**.

#### Key Generation
1. Each participant generates a random private key: \\( x_1, x_2 \\in (0,r) \\)
2. Public keys are derived: \\( P_1 = g^{x_1}, P_2 = g^{x_2} \\in \mathbb{G}_1 \\)
3. Aggregated public key is calculated: \\( P_a = g^{x_1} + g^{x_2} \\in \mathbb{G}_1 \\)
4. Synthetic private key is derived: \\( x_s = H(P_a) \\in \mathbb{F}_r \\)
5. Synthetic public key is calculated: \\( P_s = g^{x_s} + P_a \\)

#### Signing Process
Given private keys \\( x_1, x_2 \\) and a message \\( m \\), the signature is computed by:
1. Hash the message and synthetic public key: \\( h = H(m, P_s) \\)
2. Generate three signatures:
   - \\( \sigma_s = h^{x_s} \\in \mathbb{G}_2 \\)
   - \\( \sigma_1 = h^{x_1} \\)
   - \\( \sigma_2 = h^{x_2} \\)

#### Signature Aggregation
The signatures are aggregated as: \\( \sigma = \sigma_s + \sigma_1 + \sigma_2 \\)

#### Verification
The signature \\( \sigma \\) is verified by checking: \\( e(g, \sigma) = e(P_s, h) \\)


> This can be expanded as:
>
> \\[ e(g, \sigma) = e(P_s, h) \\]
>
> \\[ e(g, \sigma_s + \sigma_1 + \sigma_2) = e(g^{x_s} + g^{x_1} + g^{x_2}, h) \\]
>
> \\[ e(g, h^{x_s+x_1+x_2}) = e(g^{x_s + x_1 + x_2}, h) \\]
>
> \\[ e(g, h) = e(g, h) \\]

### Security Analysis and Considerations

#### Rogue Public Key Attack
In this attack, an adversary could register a public key \\( P_2 = g^{x'_2} - P_1 \\in \mathbb{G}_1 \\), where \\( x'_2 \\) is carefully chosen by the attacker. The synthetic public key would then be \\( P_s = g^{x_s} + P_1 + P_2 = g^{x_s} + P_1 + g^{x'_2} - P_1 = g^{x_s} + g^{x'_2} \\). The attacker could then forge an aggregate signature \\( \sigma = \sigma_s + \sigma_2 \\) that would pass verification.

#### Mitigation Strategies
1. **Independent Key Generation**: Ensure that both parties generate their private keys independently.
2. **Commit-and-Open Approach**: Both parties first provide a hash of their public key before revealing the actual public key.
3. **Proof of Possession**: Require each participant to prove possession of their private key.


## Related Approaches

### 1. Custody Tool
The Custody Tool provided by Chia Network offers a comprehensive solution for managing digital assets but is considered too heavy and complex for our specific use case. It provides advanced features for institutional custody but introduces unnecessary complexity for our implementation.

Reference: [Chia Custody Tool Documentation](https://docs.chia.net/custody-tool/)

### 2. BLS Aggregation for Threshold Signature
BLS threshold signatures allow for a flexible m-of-n signature scheme. In a \\( m \\)/\\( n \\) threshold signature scheme, \\( m \\) participants must contribute to create a valid signature.

The key generation process involves:
1. Each participant generates their private key
2. Public keys are calculated individually
3. An aggregate public key is derived by combining individual public keys

While this approach offers flexibility, the method currently lacks maturity and may not yet be fully reliable for all use cases.

References:
- [BLS Multi-Signatures With Public-Key Aggregation](https://crypto.stanford.edu/~dabo/pubs/papers/BLSmultisig.html)
- [BLS Signature Aggregation](https://muens.io/bls-signature-aggregation)
- [Exploring the power of threshold BLS](https://csrc.nist.gov/presentations/2023/mpts2023-day1-talk-threshold-BLS)
- [Threshold BLS Signatures](https://www.jcraige.com/threshold-bls-signatures)
- [Path to understanding: elliptic curves, pairings, and BLS signatures](https://geeklaunch.io/blog/understanding-elliptic-curves-pairings-bls-signatures/)
- [Fast and Scalable BLS Threshold Signatures](https://alinush.github.io/2020/03/12/scalable-bls-threshold-signatures.html)

### 3. p2_m_of_n_delegate_direct
This approach provides a more generalized m-of-n multisignature scheme, allowing for flexible threshold requirements. It extends the functionality beyond the 2/2 case to support scenarios where only a subset of signers is required. However, it requires specific smart contract of coin, which is not compactible with the main-stream coins - `p2_delegated_puzzle_or_hidden_puzzle`.

Reference: [Chia GitHub Repository](https://github.com/Chia-Network/chia-blockchain/blob/main/chia/wallet/puzzles/p2_m_of_n_delegate_direct.clsp)
