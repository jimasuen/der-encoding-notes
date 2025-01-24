# Notes on DER encoding and signatures in Bitcoin

## What is DER encoding?
- In Bitcoin, DER encoding is a format that is used to encode ECDSA signatures. Ensuring the signature follows this format can be referred to as "DER canonization".

- It serialises the ECDSA signature that is then included in the unlocking script of a bitcoin transaction. It proves ownership of the private key that was used to lock the bitcoin.

*(Note: The **unlocking script** is found in the **input section** of a transaction, while the **locking script** is found in the **output section** of a transaction)*

## Signatures

- In ECDSA signatures, the message digest (commitment hash) is signed using a **private key**.

-  The signature involves two key components, **r** and **s**, which are derived from the elliptic curve calculations involving the:

    - signer's private key; 
    - a generator point (**G**); 
    - SECP256k1 order (**n**), which is the number of possible private keys and the number of points formed in the group by the elliptic curve; and
    - a random value (**k**). 

- The value of **k** should be deterministic. What this means is that it is derived from the message digest and the signer's private key so that the same random value would be produced given a particular private key and message digest. Therefore, it is unique for each message digest and private key combination.

- Something called the **low s** tends to be preferred because it ensures consistency, that is, for the same message digest (commitment hash) and private key, the signature will always be the same. It also prevents signature malleability 

- DER encoded signatures follows a specific format:

    - 0x30<length>0x02<length of r><r>0x02<length of s><s><sighash>

- When you're signing, you need to take into consideration whether the message digest (commitment hash) has already been **double hashed with SHA-256** and formatted properly or else you'll end up with an invalid signature.