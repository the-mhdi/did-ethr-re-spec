# did:ethr re-specification
## Motivation & Gap Analysis

Based on our audit of the current ecosystem, we have identified several critical flaws:

* current did:ethr relies heavily on ERC-1056 event logs for state management and document resulotion, this leads to 1. inconsistent resolver behavior across chains/clients and Non-deterministic DID resolution, 2. migrarion to newer versions of ERC-1056 contracts almost impossible as upgrades require resolvers to track multiple registries indefinitely and 3. not being able to efficiently verify historical state or support light-client nodes.

*  current design does not support Zero-Knowledge Verifiable Credentials (ZK-VCs) in any native or standardized way. There’s no canonical commitments (e.g., claimsRoot, revocationRoot) to anchor credential trees, no standardized encoding rules for ZK circuits (CBOR, Poseidon, Merkle hashing, etc.) and no method level APIs for status checks, revocation proofs etc. 

* did:ethr lacks documented guidelines on key management, off-chain/on-chain state management, credential issuance, claim anchoring, or ZK-VC workflows. this absence creates ambiguity for developers, auditors, integrators, and identity-wallet. the current spec predates the final W3C DID Core v1.0 recommendation so lack of clear guidance on verificationMethod relationships (authentication vs. assertion), service endpoint definitions, and controller logic is visable.

* in the current method specification, there is no documented privacy model to explain 1. what parts of the identity state are public or private 2. what data leaks occur through DID Document operations and 3. how key rotations and delegations reveal user behavior.
