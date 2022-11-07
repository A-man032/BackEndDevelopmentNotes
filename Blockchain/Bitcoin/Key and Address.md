# Key and Address

Bitcoin uses elliptic curve multiplication as the base for its cryptography.

## Generation Process

- The private key is a number, usually picked at random.
- From the private key, we use elliptic curve multiplication to generate a public key.
- From the public key, we use cryptographic hash function to generate a bitcoin address.

![image-20221103133033453](.\png\GenerationProcess.png)

> The public key can be calculated from the private key so that **only storing the private key** is very enough.

### Asymmetric Cryptography

#### Bitcoin Scenario

A private key can be applied to the digital fingerprint of a transaction to produce a numerical signature. **This signature can only be produced by someone with knowledge of the private key. However, anyone with access to the public key and the transaction fingerprint can use them to verify the signature.**

#### Application

Asymmetric cryptography makes it possible for anyone to verify every signature on every transaction, while ensuring that only the owner of private key can produce valid signature.

## Private Key Generation



## Reference

[Cryptography in Bitcoin: How to Create an Address with C // nick (nickfarrow.com)](https://nickfarrow.com/Cryptography-in-Bitcoin-with-C/)

