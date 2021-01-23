---
layout: post
title:  "Witness Encryption"
date:   2020-01-18 20:00:00 +0100
author: Anders Konring
toc: true
draft: true
categories: cryptography
tags: [research, blockchain, we, witness-encryption]
---

The concept of Witness Encryption (WE) was introduced by [[GGSW13]](https://eprint.iacr.org/2013/258.pdf).
Using Witness Encryption a user can encrypt a message under a particular *problem instance* and produce a ciphertext.
The receiver of the ciphertext can then decrypt if she knows a solution (witness) to the problem.


In particular, in a Witness Encryption scheme for an NP language $$L$$, a party can encrypt a message $$m$$ using the statement $$x$$
and another party can decrypt if she knows $$w$$ such that $$(x,w)\in R$$ (i.e. a witness that shows $$x\in L$$).<br>
If $$x$$ is not in the language, $$L$$, then no polynomial-time attacker can distinguish two ciphertexts encrypting messages of equal length. <br/>
Notice, that the encrypter *does not* have to know the witness in order to create the ciphertext.


## Applications


### Public-key Encryption
One can easily construct a Public-Key Encryption scheme from Witness Encryption and Pseudorandom Generators (PRGs)[^2].<br>
Let the secret key, $$s$$, be input to the PRF and let the output be $$t\leftarrow G(s)$$. <br>
Then encryption can be done under the statement; $$t$$ is in the range of values for $$G$$
Another party can then decrypt using the secret key/witness $$s$$.

Other applications described in the [[GGSW13]](https://eprint.iacr.org/2013/258.pdf)-paper include [Identity/Attribute-based Encryption](https://en.wikipedia.org/wiki/Identity-based_encryption).


### Time-lock Encryption
[[J15]](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.738.3582&rep=rep1&type=pdf) explores how to combine Witness Encryption and so-called Computational Reference Clocks (fx blockchain) to do Time-lock encryption.<br>
This means that one can only decrypt when some pre-defined time has passed. [[J15]](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.738.3582&rep=rep1&type=pdf) suggests a construction using the Bitcoin blockchain to realize Time-lock Encryption.

## Definition

A Witness Encryption scheme for a language $$L$$ with witness relation $$R$$ has the following poly-time algorithms:

- $$Encrypt$$ <br>
$$c\leftarrow Encrypt(1^\lambda,x,m)$$<br>
On inputs; security parameter, problem statement and message the algorithm outputs the ciphertext.

- $$Decrypt$$ <br>
$$m|\bot\leftarrow Decrypt(c,w)$$<br>
On inputs; ciphertext and a witness the algorithm outputs the orignal message or bottom.

The following conditions should hold according to definition in [[GGSW13]](https://eprint.iacr.org/2013/258.pdf):

- *Correctness* <br>
For any security parameter $$\lambda$$, message $$m$$ and $$w$$ witnessing that $$x\in L$$:

$$
Pr[Decrypt(Encrypt(1^\lambda,x,m),w)=m]=1
$$


- *Soundness Security* <br>
For any probabilistic, polynomial-time adversary $$A$$, there exists a negligible function $$neg()$$ such that when $$x\notin L$$:

$$
|Pr[A(Encrypt(1^\lambda,x,0))=1]-Pr[A(Encrypt(1^\lambda,x,1))=1]| < neg(\lambda)
$$

In other words, to a PPT adversary (not knowing a witness $$w$$) an encryption to zero will look indistinguishable from an encryption to one.

## Constructions

### WE for NP
[[GGSW13]](https://eprint.iacr.org/2013/258.pdf) explores an approach to construct Witness Encryption using the [Exact Cover](https://en.wikipedia.org/wiki/Exact_cover) problem which is NP-complete. The construction uses [Cryptographic Multilinear Maps](https://en.wikipedia.org/wiki/Cryptographic_multilinear_map)[^1].

[[GGH13]](https://eprint.iacr.org/2012/610.pdf) contains a small "warm-up" proof that [Indistinguishability Obfuscation](https://en.wikipedia.org/wiki/Indistinguishability_obfuscation) (IO) implies Witness Encryption.
While there have been much skepticism around IO constructions, [new research](https://www.wired.com/story/computer-scientists-achieve-the-crown-jewel-of-cryptography/) show promising results.

### WE for Restricted Languages
Other studies explore the possibility of creating Witness Encryption for smaller languages.
In particular, given a Smooth Projective Hash Function (SPHF) for a language $$L$$ one can derive a Witness Encryption scheme.<br>
In [[DS15]](https://eprint.iacr.org/2015/1073.pdf), the language of choice is an algebraic language that enables the user to encrypt wrt. proofs using the popular Groth-Sahai non-interactive witness-indistinguishable/zero-knowledge proof framework.<br><br>
[[BL20]](https://eprint.iacr.org/2020/221.pdf) is another interesting language that verifies NIZK proofs of the validity of computation over committed values.


[^1]: Currently no "pure" (as defined in [[BS03]](https://eprint.iacr.org/2002/080.pdf)) cryptographic multilinear map exists which is why the construction is using "approximate" maps also known as graded-encoding systems [([GGH13])](https://eprint.iacr.org/2012/610.pdf)
[^2]: The PRG needs to expand by a super-logarithmic number of bits in order to prove security.
