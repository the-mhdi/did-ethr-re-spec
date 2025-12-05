# did:ethr re-specification
## Motivation & Gap Analysis

Based on our audit of the current ecosystem, we have identified several critical flaws:

* current did:ethr relies heavily on ERC-1056 event logs for state management and document resulotion, this leads to 1. inconsistent resolver behavior across chains/clients and Non-deterministic DID resolution, 2. migrarion to newer versions of ERC-1056 contracts almost impossible as upgrades require resolvers to track multiple registries indefinitely and 3. not being able to efficiently verify historical state or support light-client nodes.

*  current design does not support Zero-Knowledge Verifiable Credentials (ZK-VCs) in any native or standardized way. There’s no canonical commitments (e.g., claimsRoot, revocationRoot) to anchor credential trees, no standardized encoding rules for ZK circuits (CBOR, Poseidon, Merkle hashing, etc.) and no method level APIs for status checks, revocation proofs etc. 

* did:ethr lacks documented guidelines on key management, off-chain/on-chain state management, credential issuance, claim anchoring, or ZK-VC workflows. this absence creates ambiguity for developers, auditors, integrators, and identity-wallet. the current spec predates the final W3C DID Core v1.0 recommendation so lack of clear guidance on verificationMethod relationships (authentication vs. assertion), service endpoint definitions, and controller logic is visable.

* in the current method specification, there is no documented privacy model to explain 1. what parts of the identity state are public or private 2. what data leaks occur through DID Document operations and 3. how key rotations and delegations reveal user behavior.
## DID Method Name
  we propose to change the naming from ethr to `eid`, but keep using `ethr` throughout this doc.
  
## Method Specific Identifier
The method specific identifier is represented as the HEX-encoded secp256k1 public key (in compressed form), or the corresponding HEX-encoded Ethereum address on the target network, prefixed with 0x.

      ethr-did = "did:ethr:" ethr-specific-identifier
      ethr-specific-identifier = [ ethr-blockchain ":" ethr-networkChainID ] ethereum-address / public-key-hex
      ethr-blockchain = "eth" /"eip155" / "polygon" / "sonic" 
      ethr-networkChainID = "main" / "sepolia" / "test" / "amoy"/ 1 / 137 
      ethereum-address = "0x" 40*HEXDIG
      public-key-hex = "0x" 66*HEXDIG

### Example 
Note, if no ethr-blockchain was specified, it is assumed that the DID is anchored on the Ethereum by default(eip155).

    did:ethr:0xb9c5714089478a327f09197987f16f9e5d936e8a
    did:ethr:sepolia:0xb9c5714089478a327f09197987f16f9e5d936e8a
    did:ethr:polygon:amoy:0xb9c5714089478a327f09197987f16f9e5d936e8a
    did:ethr:sonic:main:0xb9c5714089478a327f09197987f16f9e5d936e8a

## DID Document
Example of DID document of an un-registered identity `did:ethr:0xb9c5714089478a327f09197987f16f9e5d936e8a`:

  ```json
    {
    "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/secp256k1recovery-2020/v2"
    ],
    "id": "did:ethr:0xb9c5714089478a327f09197987f16f9e5d936e8a",
    "verificationMethod": [
    {
      "id": "did:ethr:0xb9c5714089478a327f09197987f16f9e5d936e8a#controller",
      "type": "EcdsaSecp256k1RecoveryMethod2020",
      "controller": "did:ethr:0xb9c5714089478a327f09197987f16f9e5d936e8a",
      "blockchainAccountId": "eip155:1:0xb9c5714089478a327f09197987f16f9e5d936e8a"
    }
    ],
    "authentication": [
    "did:ethr:0xb9c5714089478a327f09197987f16f9e5d936e8a#controller"
    ],
    "assertionMethod": [
    "did:ethr:0xb9c5714089478a327f09197987f16f9e5d936e8a#controller"
    ],


    "states" : {
    "publicState":{
            "gateway" :"contract address or ipfs gaveway/cid or Rest API"
            "publicStateRoot":"TBD",
            "previousPublicRoot":"",
            "createdAtBlock":"",
            "replacedAtBlock":""
        }
    "privateState":{
           "privateStateRoot":"TBD",
            "previousPrivateRoot":"",
            "createdAtBlock":"0",
            "replacedAtBlock":"0"
    }
    "IssuanceState":{  
     
            "issuanceStateRoot":"TBD/revocation tree root",
            "previousIssuanceRoot":"",
            "createdAtBlock":"0",
            "replacedAtBlock":"0"
    }
    }
```
## CRUD Operation Definitions

  ### Create (Register)
  
  ### Resolve(Read)
  
  ### Updata
  
  ### Delete(Deactive)
