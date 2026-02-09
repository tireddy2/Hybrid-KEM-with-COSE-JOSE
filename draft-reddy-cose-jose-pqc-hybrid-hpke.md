---
title: "Post-Quantum and Hybrid KEMs for HPKE with JOSE and COSE"
abbrev: "PQC & PQ/T Hybrid KEMs for HPKE (JOSE/COSE)"
category: std

docname: draft-reddy-cose-jose-pqc-hybrid-hpke
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "COSE"
keyword:
 - PQC
 - COSE
 - JOSE
 - Hybrid
 - HPKE
 - Post-Quantum
 

venue:
  group: "cose"
  type: "Working Group"
  mail: "cose@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/cose/"
  

stand_alone: yes
pi: [toc, sortrefs, symrefs, strict, comments, docmapping]

author:
 -
    fullname: Tirumaleswar Reddy
    organization: Nokia
    city: Bangalore
    region: Karnataka
    country: India
    email: "k.tirumaleswar_reddy@nokia.com"
 -
    fullname: Hannes Tschofenig
    organization:
    city:
    country: Germany
    email: "hannes.tschofenig@gmx.net"

 
normative:

informative:
 
  FIPS203:
     title: "Module-Lattice-based Key-Encapsulation Mechanism Standard"
     target: https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.203.pdf
     date: false
  HPKE-IANA:
     author:
        org: IANA
     title: Hybrid Public Key Encryption (HPKE) IANA Registry
     target: https://www.iana.org/assignments/hpke/hpke.xhtml
     date: false

     
--- abstract

This document specifies the use of Post-Quantum (PQ) and Post-Quantum/Traditional (PQ/T) Hybrid Key Encapsulation Mechanisms (KEMs) within the Hybrid Public Key Encryption (HPKE) for JOSE and COSE. It defines algorithm identifiers and key formats to support pure post-quantum algorithms (ML-KEM) and their PQ/T hybrid combinations.

--- middle

# Introduction

The migration to Post-Quantum Cryptography (PQC) is unique in the history of modern digital cryptography in that neither the traditional algorithms nor the post-quantum algorithms are fully trusted to protect data for the required data lifetimes. The traditional algorithms, such as RSA and elliptic curve cryptography (ECC), will fall to quantum cryptanalysis, while the post-quantum algorithms face uncertainty about the underlying mathematics, compliance issues, unknown vulnerabilities, hardware and software implementations that have not had sufficient maturing time to rule out classical cryptanalytic attacks and implementation bugs.

During this transition, deployments may adopt different strategies depending on their security posture, risk tolerance, and external constraints. Hybrid key exchange is generally preferred over pure PQC key exchange because it provides defense in depth by combining the strengths of both traditional and post-quantum algorithms. This approach ensures continued security even if one of the component algorithms is compromised during the transitional period.

However, pure PQC key exchange may be required for specific deployments with regulatory or compliance mandates that necessitate the exclusive use of post-quantum cryptography. Such requirements may arise in environments governed by stringent cryptographic standards that prohibit reliance on traditional public-key algorithms.

Hybrid Public Key Encryption (HPKE) specifies a scheme for encrypting arbitrary-length plaintexts to a recipient’s public key. The use of HPKE with JOSE and COSE is specified in {{?I-D.ietf-jose-hpke-encrypt}} and {{?I-D.ietf-cose-hpke}}, respectively. HPKE can be extended to support both pure PQC and post-quantum/traditional (PQ/T) hybrid Key Encapsulation Mechanisms (KEMs), as defined in {{?I-D.ietf-hpke-pq}}. This document specifies the use of these KEMs in HPKE for JOSE and COSE.

Supporting both pure PQC and PQ/T hybrid KEMs enables flexible deployment choices: hybrid mechanisms provide a conservative transition strategy with defense in depth, while pure PQC mechanisms accommodate deployments with regulatory, compliance, or policy-driven requirements for exclusive use of PQC.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

This document makes use of the terms defined in {{?I-D.ietf-pquip-pqt-hybrid-terminology}}. For the purposes of this document, it is helpful to be able to divide cryptographic algorithms into two classes:

"Traditional Algorithm":  An asymmetric cryptographic algorithm based on integer factorisation, finite field discrete logarithms, elliptic curve discrete logarithms, or related mathematical problems. In the context of JOSE, examples of traditional key exchange algorithms include Elliptic Curve Diffie-Hellman Ephemeral Static {{?RFC6090}} {{?RFC8037}}. In the context of COSE, examples of traditional key exchange algorithms include Ephemeral-Static (ES) DH and Static-Static (SS) DH {{?RFC9052}}. 

"Post-Quantum Algorithm":  An asymmetric cryptographic algorithm that is believed to be secure against attacks using quantum computers as well as classical computers. Examples of PQC key exchange algorithms include ML-KEM.

"Post-Quantum Traditional (PQ/T) Hybrid Scheme":  A multi-algorithm scheme where at least one component algorithm is a post-quantum algorithm and at least one is a traditional algorithm.

"PQ/T Hybrid Key Encapsulation Mechanism":  A multi-algorithm KEM made up of two or more component KEM algorithms where at least one is a post-quantum algorithm and at least one is a traditional algorithm.

# Construction

ML-KEM is a one-pass (store-and-forward) cryptographic mechanism for an originator to securely send keying material to a recipient using the recipient's ML-KEM public key. Three parameter sets for ML-KEMs are specified by {{FIPS203}}. In order of increasing security strength (and decreasing performance), these parameter sets
are ML-KEM-512, ML-KEM-768, and ML-KEM-1024. For pure PQC, the ML-KEM algorithms are used as standalone KEMs within the HPKE framework as defined in {{?I-D.ietf-hpke-pq}}. 

While this document defines ciphersuites for all three parameter sets, implementers should follow the guidance in Section 3 of {{?I-D.ietf-hpke-pq}} regarding the selection of security levels. Specifically, it is noted that while ML-KEM-512 provides NIST security category 1, the use of ML-KEM-768 or ML-KEM-1024 is generally preferred to provide a higher security margin against potential future cryptanalysis

PQ/T algorithms for HPKE {{?I-D.ietf-hpke-pq}} use a multi-algorithm scheme, where one component algorithm is a post-quantum algorithm and another one is a traditional algorithm. The hybrid combiner construction, such as the C2PRICombiner defined in {{?I-D.irtf-cfrg-hybrid-kems}}, combines the shared secrets and public values from a post-quantum KEM and a traditional KEM to derive a single shared secret for HPKE.

# Alignment with JOSE HPKE Modes

The JOSE HPKE specification {{?I-D.ietf-jose-hpke-encrypt}} and the COSE HPKE specification {{?I-D.ietf-cose-hpke}} define the use of HPKE with two Key Management Modes:

* Key Encryption, and
* Integrated Encryption.

In both JOSE and COSE, the selected Key Management Mode determines how HPKE is applied at the message layer. In Key Encryption mode, HPKE is used to encrypt a Content Encryption Key (CEK), which is then used to encrypt the payload. In Integrated Encryption mode, HPKE is used directly to encrypt the payload, and no separate CEK is employed.

Each Key Management Mode is identified by a distinct algorithm identifier (alg) in both JOSE and COSE. This document registers separate HPKE algorithm identifiers for Key Encryption and Integrated Encryption for both pure PQC and PQ/T hybrid HPKE instantiations.

This separation ensures that JOSE and COSE implementations can determine the intended HPKE Key Management Mode solely from the alg value, without ambiguity, and preserves compatibility with existing HPKE processing models.

# Ciphersuite Registration

This specification registers a set of pure PQC and PQ/T hybrid KEMs for use with HPKE. In this context, an HPKE ciphersuite is defined as a combination of the following algorithm components:

- KEM algorithm, which may be either:
  - a pure PQC KEM (for example, ML-KEM-768), or
  - a PQ/T hybrid KEM that combines a PQC KEM with a traditional
    key-exchange algorithm (for example, ML-KEM-768 + X25519, defined as
    "MLKEM768-X25519" in {{?I-D.ietf-hpke-pq}})
- KDF algorithm
- AEAD algorithm

The values for KEM, KDF, and AEAD are drawn from the HPKE IANA registry {{HPKE-IANA}}. Consequently, JOSE and COSE can only use algorithm combinations that are already defined and registered for HPKE.

The HPKE ciphersuites defined for use with JOSE and COSE, including both pure PQC and PQ/T hybrid KEMs, are specified in {{IANA}}.

Note that the pure PQC and PQ/T hybrid KEMs defined for HPKE are not authenticated KEMs. As a result, only the HPKE Base mode is supported when using these KEMs, in accordance with the HPKE and JOSE/COSE HPKE specifications.

# AKP Key for Pure PQC and PQ/T Hybrid Algorithms in HPKE

This section describes the required parameters for an "AKP" key type, as
defined in {{!I-D.ietf-cose-dilithium}}, and its use with pure PQC
and PQ/T hybrid algorithms for HPKE, as defined in {#XWING} and {#XWING-KE}. 
An example key representation is also provided for illustration.

## Required Parameters

A JSON Web Key (JWK) or COSE_Key with a key type (`kty`) for use with pure PQC
or PQ/T hybrid algorithms for HPKE includes the following parameters:

- kty (Key Type)  
  The `kty` parameter MUST be present and MUST be set to "AKP".

- alg (Algorithm)  
  The `alg` parameter MUST be present and MUST identify the pure PQC or
  PQ/T hybrid algorithm for HPKE, as defined in {#XWING} or {#XWING-KE}.  
  HPKE algorithms using pure PQC or PQ/T hybrid KEMs are those registered
  in the "JSON Web Signature and Encryption Algorithms" registry and the
  "COSE Algorithms" registry, and are derived from the corresponding
  KEM identifiers in the HPKE IANA registry.

- pub (Public Key)  
  The `pub` parameter MUST be present and MUST contain the public
  encapsulation key (`pk`) as defined in Section 5.1 of
  {{?I-D.irtf-cfrg-hybrid-kems}}.  
  When represented as a JWK, this value MUST be base64url-encoded.

- priv (Private Key)  
  When representing a private key, the `priv` parameter MUST be present
  and MUST contain the private decapsulation key (`sk`) as defined in
  Section 5.1 of {{?I-D.irtf-cfrg-hybrid-kems}}.  
  When represented as a JWK, this value MUST be base64url-encoded.


### Example

The following is an example JWK representation of an "AKP" key for the "MLKEM768-X25519-SHAKE256-AES-256-GCM" algorithm:

~~~
{
    "kty"  : "AKP", 
    "alg"  : "HPKE-7", 
    "pub"  : "4iNrNajCSz...tmrrIzQSQQO9lNA", 
    "priv" : "f5wrpOiP...rPpm7yY" 
}
~~~

# Security Considerations

The security considerations in {{?I-D.ietf-hpke-pq}}, {{?I-D.ietf-jose-hpke-encrypt}} and {{?I-D.ietf-cose-hpke}} are to be taken into account.

The shared secrets computed in the hybrid key exchange should be computed in a way that achieves the "hybrid" property: the resulting secret is secure as long as at least one of the component key exchange algorithms is unbroken. PQC KEMs used in the manner described in this document MUST explicitly be designed to be secure in the event that the public key is reused, such as achieving IND-CCA2 security.  ML-KEM has such security properties.

## Post-Quantum Security for Multiple Recipients 

In HPKE JWE Key Encryption, when encrypting the Content Encryption Key (CEK) for multiple recipients, it is crucial to consider the security requirements of the message to safeguard against "Harvest Now, Decrypt Later" attacks. For messages requiring post-quantum security, all recipients MUST use algorithms supporting post-quantum cryptographic methods, such as PQC KEMs or Hybrid PQ/T KEMs. Using traditional algorithms (e.g., ECDH-ES) for any recipient in these scenarios compromises the overall security of the message.

# IANA Considerations {#IANA}

## JOSE

This document requests IANA to add new values to the "JSON Web Signature and Encryption Algorithms" registry.

### JOSE Algorithms for Integrated Encryption {#XWING}

- Algorithm Name: HPKE-7
- Algorithm Description: Integrated Encryption with HPKE that uses the P-256 + ML-KEM-768 Hybrid KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Documents(s): TODO

- Algorithm Name: HPKE-8
- Algorithm Description: Integrated Encryption with HPKE that uses the P-256 + ML-KEM-768 Hybrid KEM, the SHAKE256 KDF, and the ChaCha20Poly1305 AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Documents(s): TODO

- Algorithm Name: HPKE-9
- Algorithm Description: Integrated Encryption with HPKE that uses the X25519 + ML-KEM-768 Hybrid KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Documents(s): TODO

- Algorithm Name: HPKE-10
- Algorithm Description: Integrated Encryption with HPKE that uses the X25519 + ML-KEM-768 Hybrid KEM, the SHAKE256 KDF, and the ChaCha20Poly1305 AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Documents(s): TODO

- Algorithm Name: HPKE-11
- Algorithm Description: Integrated Encryption with HPKE that uses the P-384 + ML-KEM-1024 Hybrid KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Documents(s): TODO

- Algorithm Name: HPKE-12
- Algorithm Description: Integrated Encryption with HPKE that uses the P-384 + ML-KEM-1024 Hybrid KEM, the SHAKE256 KDF, and the ChaCha20Poly1305 AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Documents(s): TODO

- Algorithm Name: HPKE-13
- Algorithm Description: Integrated Encryption with HPKE that uses the ML-KEM-512 KEM, the SHAKE256 KDF, and the AES-128-GCM AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Document(s): TODO

- Algorithm Name: HPKE-14
- Algorithm Description: Integrated Encryption with HPKE that uses the ML-KEM-768 KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Document(s): TODO

- Algorithm Name: HPKE-15
- Algorithm Description: Integrated Encryption with HPKE that uses the ML-KEM-1024 KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Document(s): TODO

### JOSE Algorithms for Key Encryption {#XWING-KE}

- Algorithm Name: HPKE-7-KE
- Algorithm Description: Key Encryption with HPKE that uses the P-256 + ML-KEM-768 Hybrid KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Document(s): TODO

- Algorithm Name: HPKE-8-KE
- Algorithm Description: Key Encryption with HPKE that uses the P-256 + ML-KEM-768 Hybrid KEM, the SHAKE256 KDF, and the ChaCha20Poly1305 AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Document(s): TODO

- Algorithm Name: HPKE-9-KE
- Algorithm Description: Key Encryption with HPKE that uses the X25519 + ML-KEM-768 Hybrid KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Document(s): TODO

- Algorithm Name: HPKE-10-KE
- Algorithm Description: Key Encryption with HPKE that uses the X25519 + ML-KEM-768 Hybrid KEM, the SHAKE256 KDF, and the ChaCha20Poly1305 AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Document(s): TODO

- Algorithm Name: HPKE-11-KE
- Algorithm Description: Key Encryption with HPKE that uses the P-384 + ML-KEM-1024 Hybrid KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Document(s): TODO

- Algorithm Name: HPKE-12-KE
- Algorithm Description: Key Encryption with HPKE that uses the P-384 + ML-KEM-1024 Hybrid KEM, the SHAKE256 KDF, and the ChaCha20Poly1305 AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Document(s): TODO

- Algorithm Name: HPKE-13-KE
- Algorithm Description: Key Encryption with HPKE that uses the ML-KEM-512 KEM, the SHAKE256 KDF, and the AES-128-GCM AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Document(s): TODO

- Algorithm Name: HPKE-14-KE
- Algorithm Description: Key Encryption with HPKE that uses the ML-KEM-768 KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Document(s): TODO

- Algorithm Name: HPKE-15-KE
- Algorithm Description: Key Encryption with HPKE that uses the ML-KEM-1024 KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
- Algorithm Usage Location(s): "alg"
- JOSE Implementation Requirements: Optional
- Change Controller: IANA
- Specification Document(s): [[TBD: This RFC]]
- Algorithm Analysis Document(s): TODO

## COSE

This document requests IANA to add new values to the 'COSE Algorithms' registry.

### COSE Algorithms Registry 

*  Name: MLKEM768-P256-SHAKE256-AES-256-GCM
*  Value: TBD1 
*  Description: Cipher suite for COSE-HPKE in Base Mode that uses the ML-KEM-768 + P-256 Hybrid KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
*  Capabilities: [kty]
*  Change Controller: IANA
*  Reference: [[TBD: This RFC]]

*  Name: MLKEM768-P256-SHAKE256-ChaCha20Poly1305
*  Value: TBD2
*  Description: Cipher suite for COSE-HPKE in Base Mode that uses the ML-KEM-768 + P-256 Hybrid KEM, the SHAKE256 KDF, and the ChaCha20Poly1305 AEAD.
*  Capabilities: [kty]
*  Change Controller: IANA
*  Reference: [[TBD: This RFC]]

*  Name: MLKEM768-X25519-SHAKE256-AES-256-GCM
*  Value: TBD3
*  Description: Cipher suite for COSE-HPKE in Base Mode that uses the ML-KEM-768 + X25519 Hybrid KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
*  Capabilities: [kty]
*  Change Controller: IANA
*  Reference: [[TBD: This RFC]]

*  Name: MLKEM768-X25519-SHAKE256-ChaCha20Poly1305
*  Value: TBD4
*  Description: Cipher suite for COSE-HPKE in Base Mode that uses the ML-KEM-768 + X25519 Hybrid KEM, the SHAKE256 KDF, and the ChaCha20Poly1305 AEAD.
*  Capabilities: [kty]
*  Change Controller: IANA
*  Reference: [[TBD: This RFC]]

*  Name: MLKEM1024-P384-SHAKE256-AES-256-GCM
*  Value: TBD5 
*  Description: Cipher suite for COSE-HPKE in Base Mode that uses the ML-KEM-1024 + P-384 Hybrid KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
*  Capabilities: [kty]
*  Change Controller: IANA
*  Reference: [[TBD: This RFC]]

*  Name: MLKEM1024-P384-SHAKE256-ChaCha20Poly1305
*  Value: TBD6
*  Description: Cipher suite for COSE-HPKE in Base Mode that uses the ML-KEM-1024 + P-384 Hybrid KEM, the SHAKE256 KDF, and the ChaCha20Poly1305 AEAD.
*  Capabilities: [kty]
*  Change Controller: IANA
*  Reference: [[TBD: This RFC]]

* Name: MLKEM512-SHAKE256-AES-128-GCM
* Value: TBD7
* Description: Cipher suite for COSE-HPKE in Base Mode that uses the ML-KEM-512 KEM, the SHAKE256 KDF, and the AES-128-GCM AEAD.
* Capabilities: [kty]
* Change Controller: IANA
* Reference: [[TBD: This RFC]]

* Name: MLKEM768-SHAKE256-AES-256-GCM
* Value: TBD8
* Description: Cipher suite for COSE-HPKE in Base Mode that uses the ML-KEM-768 KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
* Capabilities: [kty]
* Change Controller: IANA
* Reference: [[TBD: This RFC]]

* Name: MLKEM1024-SHAKE256-AES-256-GCM
* Value: TBD9
* Description: Cipher suite for COSE-HPKE in Base Mode that uses the ML-KEM-1024 KEM, the SHAKE256 KDF, and the AES-256-GCM AEAD.
* Capabilities: [kty]
* Change Controller: IANA
* Reference: [[TBD: This RFC]]

# Acknowledgments
{: numbered="false"}

Thanks to Ilari Liusvaara and Orie Steele for the discussion and comments.