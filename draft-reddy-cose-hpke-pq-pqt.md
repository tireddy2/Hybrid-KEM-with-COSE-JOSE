---
title: "COSE HPKE PQ & PQ/T Algorithm Registrations"
abbrev: "COSE HPKE PQ"
category: std

docname: draft-reddy-cose-hpke-pq-pqt-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "CBOR Object Signing and Encryption"
keyword:
 - COSE
 - HPKE
 - post-quantum
 - hybrid
 - ML-KEM
 - PQ
 - PQ/T
 - CRQC
venue:
  group: "cose"
  type: "Working Group"
  mail: "cose@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/cose/"
  github: "tireddy2/Hybrid-KEM-with-COSE-JOSE"
  latest: "https://tireddy2.github.io/Hybrid-KEM-with-COSE-JOSE/draft-reddy-cose-hpke-pq-pqt.html"

author:
 -
    fullname: Tirumaleswar Reddy
    organization: Nokia
    email: k.tirumaleswar_reddy@nokia.com
 -
    fullname: Hannes Tschofenig
    organization: University of the Bundeswehr Munich
    abbrev: UniBw M.
    email: hannes.tschofenig@gmx.net
 -
    fullname: Filip Skokan
    organization: Okta
    email: panva.ip@gmail.com
 -
    fullname: Brian Campbell
    organization: Ping Identity
    email: bcampbell@pingidentity.com

normative:
  I-D.ietf-cose-hpke:
  I-D.ietf-hpke-pq:
  I-D.ietf-cose-dilithium:

informative:
  RFC9052:
  RFC9053:
  RFC9794:
  I-D.ietf-pquip-pqc-engineers:
  CNSA2.0:
    title: "Announcing the Commercial National Security Algorithm Suite 2.0"
    author:
      org: National Security Agency
    date: 2025-05
    target: https://media.defense.gov/2025/May/30/2003728741/-1/-1/0/CSA_CNSA_2.0_ALGORITHMS.PDF

...

--- abstract

This document registers Post-Quantum (PQ) and Post-Quantum/Traditional (PQ/T)
hybrid algorithm identifiers for use with CBOR Object Signing and Encryption
(COSE), building on the Hybrid Public Key Encryption (HPKE) framework.


--- middle

# Introduction

{{I-D.ietf-cose-hpke}} defines how to use Hybrid Public Key Encryption (HPKE)
with COSE_Encrypt0 and COSE_Encrypt structures ({{RFC9052}}) using traditional
Key Encapsulation Mechanisms (KEM) based on Elliptic-curve Diffie-Hellman (ECDH).

This document extends the set of registered HPKE algorithms to include Post-Quantum
(PQ) and Post-Quantum/Traditional (PQ/T) hybrid KEMs, as defined in
{{I-D.ietf-hpke-pq}}. These algorithms provide protection against attacks by
cryptographically relevant quantum computers.

The term "PQ/T hybrid" is used here consistent with {{I-D.ietf-hpke-pq}} to denote a
combination of post-quantum and traditional algorithms, and should not be confused
with HPKE's use of "hybrid" to describe the combination of asymmetric and symmetric
encryption.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

This document uses the terms "Traditional Algorithm", "Post-Quantum Algorithm",
"PQ/T Hybrid Scheme", and "PQ/T Hybrid KEM" as defined in {{RFC9794}}. The
term "pure post-quantum" is used in this document to refer to a
single-algorithm scheme using only a post-quantum algorithm, with no
traditional component.


# Algorithm Identifiers {#algorithm-identifiers}

This section defines the algorithm identifiers for PQ and PQ/T HPKE-based
encryption in COSE. Each algorithm is defined by a combination of an HPKE KEM,
a Key Derivation Function (KDF), and an Authenticated Encryption with
Associated Data (AEAD) algorithm.

All algorithms defined in this section follow the same operational model as
those in {{I-D.ietf-cose-hpke}}, supporting both integrated encryption
as defined in {{Section 3.2 of I-D.ietf-cose-hpke}} and key encryption
as defined in {{Section 3.3 of I-D.ietf-cose-hpke}}.

Test vectors for all algorithms defined in this section are provided in
{{test-vectors}}.

## PQ/T Hybrid Integrated Encryption Algorithms

The following table lists the algorithm identifiers for PQ/T hybrid integrated
encryption, where HPKE directly encrypts the plaintext without a separate
Content Encryption Key:

<!-- begin:table cose-pqt-hybrid-integrated-table "PQ/T Hybrid Integrated Encryption Algorithms" ; see README for regeneration instructions, do not edit -->

| Name    | Value             | HPKE KEM                   | HPKE KDF            | HPKE AEAD              |
| ------- | ----------------- | -------------------------- | ------------------- | ---------------------- |
| HPKE-8  | TBD (Assumed: 54) | MLKEM768-P256 (`0x0050`)   | SHAKE256 (`0x0011`) | AES-256-GCM (`0x0002`) |
| HPKE-9  | TBD (Assumed: 56) | MLKEM768-X25519 (`0x647a`) | SHAKE256 (`0x0011`) | AES-256-GCM (`0x0002`) |
| HPKE-10 | TBD (Assumed: 58) | MLKEM1024-P384 (`0x0051`)  | SHAKE256 (`0x0011`) | AES-256-GCM (`0x0002`) |
{: #cose-pqt-hybrid-integrated-table title="PQ/T Hybrid Integrated Encryption Algorithms" }

<!-- end:table -->

These algorithms combine ML-KEM with a traditional elliptic curve algorithm in a
PQ/T hybrid KEM, with the goal that compromise of either the post-quantum or
the traditional component alone does not undermine the security of the resulting
encryption.

## Pure PQ Integrated Encryption Algorithms

The following table lists the algorithm identifiers for pure post-quantum
integrated encryption:

<!-- begin:table cose-pure-pq-integrated-table "Pure PQ Integrated Encryption Algorithms" ; see README for regeneration instructions, do not edit -->

| Name    | Value             | HPKE KEM               | HPKE KDF            | HPKE AEAD              |
| ------- | ----------------- | ---------------------- | ------------------- | ---------------------- |
| HPKE-11 | TBD (Assumed: 60) | ML-KEM-512 (`0x0040`)  | SHAKE256 (`0x0011`) | AES-128-GCM (`0x0001`) |
| HPKE-12 | TBD (Assumed: 62) | ML-KEM-768 (`0x0041`)  | SHAKE256 (`0x0011`) | AES-256-GCM (`0x0002`) |
| HPKE-13 | TBD (Assumed: 64) | ML-KEM-1024 (`0x0042`) | SHAKE256 (`0x0011`) | AES-256-GCM (`0x0002`) |
{: #cose-pure-pq-integrated-table title="Pure PQ Integrated Encryption Algorithms" }

<!-- end:table -->

These algorithms provide pure post-quantum security using ML-KEM without a
traditional algorithm component.

## PQ/T Hybrid Key Encryption Algorithms

The following table lists the algorithm identifiers for PQ/T hybrid key
encryption, where HPKE encrypts the Content Encryption Key:

<!-- begin:table cose-pqt-hybrid-key-encryption-table "PQ/T Hybrid Key Encryption Algorithms" ; see README for regeneration instructions, do not edit -->

| Name       | Value             | HPKE KEM                   | HPKE KDF            | HPKE AEAD              |
| ---------- | ----------------- | -------------------------- | ------------------- | ---------------------- |
| HPKE-8-KE  | TBD (Assumed: 55) | MLKEM768-P256 (`0x0050`)   | SHAKE256 (`0x0011`) | AES-256-GCM (`0x0002`) |
| HPKE-9-KE  | TBD (Assumed: 57) | MLKEM768-X25519 (`0x647a`) | SHAKE256 (`0x0011`) | AES-256-GCM (`0x0002`) |
| HPKE-10-KE | TBD (Assumed: 59) | MLKEM1024-P384 (`0x0051`)  | SHAKE256 (`0x0011`) | AES-256-GCM (`0x0002`) |
{: #cose-pqt-hybrid-key-encryption-table title="PQ/T Hybrid Key Encryption Algorithms" }

<!-- end:table -->

These are the key encryption counterparts of the PQ/T hybrid integrated
encryption algorithms defined in {{cose-pqt-hybrid-integrated-table}}.

## Pure PQ Key Encryption Algorithms

The following table lists the algorithm identifiers for pure post-quantum key
encryption:

<!-- begin:table cose-pure-pq-key-encryption-table "Pure PQ Key Encryption Algorithms" ; see README for regeneration instructions, do not edit -->

| Name       | Value             | HPKE KEM               | HPKE KDF            | HPKE AEAD              |
| ---------- | ----------------- | ---------------------- | ------------------- | ---------------------- |
| HPKE-11-KE | TBD (Assumed: 61) | ML-KEM-512 (`0x0040`)  | SHAKE256 (`0x0011`) | AES-128-GCM (`0x0001`) |
| HPKE-12-KE | TBD (Assumed: 63) | ML-KEM-768 (`0x0041`)  | SHAKE256 (`0x0011`) | AES-256-GCM (`0x0002`) |
| HPKE-13-KE | TBD (Assumed: 65) | ML-KEM-1024 (`0x0042`) | SHAKE256 (`0x0011`) | AES-256-GCM (`0x0002`) |
{: #cose-pure-pq-key-encryption-table title="Pure PQ Key Encryption Algorithms" }

<!-- end:table -->

These are the key encryption counterparts of the pure PQ integrated
encryption algorithms defined in {{cose-pure-pq-integrated-table}}.


# COSE_Key Representation

Keys for the algorithms defined in this document use the "AKP" (Algorithm
Key Pair) COSE key type defined in {{Section 3 of I-D.ietf-cose-dilithium}}.
The required "alg" (label 3) parameter identifies the HPKE ciphersuite as
well as whether the key is used for Integrated Encryption or Key Encryption.

The public key parameter (label -1) contains the SerializePublicKey() output
for the corresponding KEM, and for private keys the private key parameter
(label -2) contains the SerializePrivateKey() output, both as defined in
{{Section 4 of !I-D.ietf-hpke-hpke}}. Both values are encoded as CBOR byte
strings.

Examples of COSE_Keys for each algorithm are provided in {{test-vectors}}.


# Security Considerations

The security considerations of {{I-D.ietf-cose-hpke}} and
{{I-D.ietf-hpke-pq}} apply to this document.
{{I-D.ietf-pquip-pqc-engineers}} provides general background on the
threat posed by cryptographically relevant quantum computers (CRQCs),
the properties of KEMs, and considerations for PQ/T hybrid schemes.

This document registers ciphersuites based on ML-KEM-512.
As noted in {{Section 3 of I-D.ietf-hpke-pq}}, given the
relative novelty of ML-KEM, there is concern that new cryptanalysis
might reduce the security level of ML-KEM-512. Use of ML-KEM-768 or
ML-KEM-1024 acts as a hedge against such cryptanalysis at a modest
performance penalty, and is RECOMMENDED where the additional overhead
is acceptable.

The PQ/T hybrid ciphersuites registered by this document are motivated
by the PQ/T Hybrid Confidentiality property ({{Section 5 of RFC9794}},
{{Section 13.1 of I-D.ietf-pquip-pqc-engineers}}): confidentiality is
preserved as long as at least one of the component algorithms remains
secure. The traditional component protects against unforeseen
cryptanalysis of ML-KEM, while the post-quantum component protects
against Harvest Now, Decrypt Later (HNDL) attacks
({{Section 7 of I-D.ietf-pquip-pqc-engineers}}) by a future CRQC.
PQ/T hybrid ciphersuites are generally preferred for this reason during
the transition to post-quantum cryptography.

The pure PQ ciphersuites are registered to accommodate deployments with
regulatory or compliance mandates that require the exclusive use of
post-quantum algorithms, such as those governed by the Commercial
National Security Algorithm Suite 2.0 {{CNSA2.0}}, as well as
deployments where the size or performance overhead of a traditional
component is undesirable.

When the Key Encryption algorithms defined in
{{cose-pqt-hybrid-key-encryption-table}} or {{cose-pure-pq-key-encryption-table}}
are used in a COSE_Encrypt structure with multiple COSE_Recipient entries,
all recipients MUST use a quantum-resistant Key Management algorithm.
Including a recipient that uses a quantum-susceptible algorithm would
allow an adversary performing an HNDL attack to recover the Content
Encryption Key once a CRQC becomes available; see
{{Section 15.4 of I-D.ietf-pquip-pqc-engineers}}.

## Security Strength

Ciphersuites based on ML-KEM-512 target NIST post-quantum security
level 1; those based on ML-KEM-768 target security level 3; and those
based on ML-KEM-1024 target security level 5 (see
{{Section 11 of I-D.ietf-pquip-pqc-engineers}}).
In the PQ/T hybrid ciphersuites, the traditional component provides an
additional classical security floor: P-256 and X25519 offer approximately
128-bit classical security, while P-384 offers approximately 192-bit
classical security. The -KE variants share the same cryptographic
properties as their integrated encryption counterparts.

All ciphersuites use SHAKE256 as the KDF, aligning with the hash family
used internally by ML-KEM. The AEAD is paired with the KEM security
level: ML-KEM-512 ciphersuites use AES-128-GCM, while ML-KEM-768,
ML-KEM-1024, and the PQ/T hybrid ciphersuites use AES-256-GCM. As
discussed in {{Section 3.1 of I-D.ietf-pquip-pqc-engineers}}, symmetric
primitives are only modestly affected by quantum attacks and doubling
key sizes is not strictly required; AES-256-GCM is nonetheless selected
for the higher-strength ciphersuites to provide a comfortable margin
consistent with security level 3 and 5 parameter sets and with
contemporary guidance such as {{CNSA2.0}}. AES-128-GCM is used with
ML-KEM-512 since pairing a level-1 KEM with a level-5 AEAD would not
improve the overall security level while increasing implementation
and bandwidth cost. The widespread hardware acceleration and broad
deployment of AES-GCM make it a reasonable choice for all ciphersuites
defined in this document.


# IANA Considerations

## COSE Algorithms Registry

This document requests registration of the following values in the
IANA "COSE Algorithms" registry established by {{RFC9053}}:

<!-- begin:cose-iana-registrations ; see README for regeneration instructions, do not edit -->

### HPKE-8
{: toc="exclude"}

- Name: HPKE-8
- Value: TBD (Assumed: 54)
- Description: Integrated Encryption with HPKE using MLKEM768-P256 KEM, SHAKE256 KDF, and AES-256-GCM AEAD
- Capabilities: [kty]
- Change Controller: IETF
- Reference: {{cose-pqt-hybrid-integrated-table}} of this document
- Recommended: Yes

### HPKE-8-KE
{: toc="exclude"}

- Name: HPKE-8-KE
- Value: TBD (Assumed: 55)
- Description: Key Encryption with HPKE using MLKEM768-P256 KEM, SHAKE256 KDF, and AES-256-GCM AEAD
- Capabilities: [kty]
- Change Controller: IETF
- Reference: {{cose-pqt-hybrid-key-encryption-table}} of this document
- Recommended: Yes

### HPKE-9
{: toc="exclude"}

- Name: HPKE-9
- Value: TBD (Assumed: 56)
- Description: Integrated Encryption with HPKE using MLKEM768-X25519 KEM, SHAKE256 KDF, and AES-256-GCM AEAD
- Capabilities: [kty]
- Change Controller: IETF
- Reference: {{cose-pqt-hybrid-integrated-table}} of this document
- Recommended: Yes

### HPKE-9-KE
{: toc="exclude"}

- Name: HPKE-9-KE
- Value: TBD (Assumed: 57)
- Description: Key Encryption with HPKE using MLKEM768-X25519 KEM, SHAKE256 KDF, and AES-256-GCM AEAD
- Capabilities: [kty]
- Change Controller: IETF
- Reference: {{cose-pqt-hybrid-key-encryption-table}} of this document
- Recommended: Yes

### HPKE-10
{: toc="exclude"}

- Name: HPKE-10
- Value: TBD (Assumed: 58)
- Description: Integrated Encryption with HPKE using MLKEM1024-P384 KEM, SHAKE256 KDF, and AES-256-GCM AEAD
- Capabilities: [kty]
- Change Controller: IETF
- Reference: {{cose-pqt-hybrid-integrated-table}} of this document
- Recommended: Yes

### HPKE-10-KE
{: toc="exclude"}

- Name: HPKE-10-KE
- Value: TBD (Assumed: 59)
- Description: Key Encryption with HPKE using MLKEM1024-P384 KEM, SHAKE256 KDF, and AES-256-GCM AEAD
- Capabilities: [kty]
- Change Controller: IETF
- Reference: {{cose-pqt-hybrid-key-encryption-table}} of this document
- Recommended: Yes

### HPKE-11
{: toc="exclude"}

- Name: HPKE-11
- Value: TBD (Assumed: 60)
- Description: Integrated Encryption with HPKE using ML-KEM-512 KEM, SHAKE256 KDF, and AES-128-GCM AEAD
- Capabilities: [kty]
- Change Controller: IETF
- Reference: {{cose-pure-pq-integrated-table}} of this document
- Recommended: Yes

### HPKE-11-KE
{: toc="exclude"}

- Name: HPKE-11-KE
- Value: TBD (Assumed: 61)
- Description: Key Encryption with HPKE using ML-KEM-512 KEM, SHAKE256 KDF, and AES-128-GCM AEAD
- Capabilities: [kty]
- Change Controller: IETF
- Reference: {{cose-pure-pq-key-encryption-table}} of this document
- Recommended: Yes

### HPKE-12
{: toc="exclude"}

- Name: HPKE-12
- Value: TBD (Assumed: 62)
- Description: Integrated Encryption with HPKE using ML-KEM-768 KEM, SHAKE256 KDF, and AES-256-GCM AEAD
- Capabilities: [kty]
- Change Controller: IETF
- Reference: {{cose-pure-pq-integrated-table}} of this document
- Recommended: Yes

### HPKE-12-KE
{: toc="exclude"}

- Name: HPKE-12-KE
- Value: TBD (Assumed: 63)
- Description: Key Encryption with HPKE using ML-KEM-768 KEM, SHAKE256 KDF, and AES-256-GCM AEAD
- Capabilities: [kty]
- Change Controller: IETF
- Reference: {{cose-pure-pq-key-encryption-table}} of this document
- Recommended: Yes

### HPKE-13
{: toc="exclude"}

- Name: HPKE-13
- Value: TBD (Assumed: 64)
- Description: Integrated Encryption with HPKE using ML-KEM-1024 KEM, SHAKE256 KDF, and AES-256-GCM AEAD
- Capabilities: [kty]
- Change Controller: IETF
- Reference: {{cose-pure-pq-integrated-table}} of this document
- Recommended: Yes

### HPKE-13-KE
{: toc="exclude"}

- Name: HPKE-13-KE
- Value: TBD (Assumed: 65)
- Description: Key Encryption with HPKE using ML-KEM-1024 KEM, SHAKE256 KDF, and AES-256-GCM AEAD
- Capabilities: [kty]
- Change Controller: IETF
- Reference: {{cose-pure-pq-key-encryption-table}} of this document
- Recommended: Yes

<!-- end:cose-iana-registrations -->

--- back

# Test Vectors {#test-vectors}

This appendix provides test vectors for each algorithm defined in this document.
For each algorithm, a private COSE_Key and an example encrypted COSE message
(COSE_Encrypt0 for integrated encryption suites, or COSE_Encrypt with a single
COSE_Recipient for key encryption suites) are provided, each shown in CBOR
diagnostic notation and as hex-encoded CBOR.

<!-- begin:cose-test-vectors ; see README for regeneration instructions, do not edit -->

## HPKE-8
{: toc="exclude"}

~~~ cbor-diag
{::include examples/cose-keys/HPKE-8-diag.txt}
~~~
{: title="HPKE-8 COSE_Key (Diagnostic Notation)"}

~~~
{::include examples/cose-keys/HPKE-8-hex.txt}
~~~
{: title="HPKE-8 COSE_Key (Hex-Encoded CBOR)"}

~~~ cbor-diag
{::include examples/cose/HPKE-8-diag.txt}
~~~
{: title="HPKE-8 COSE_Encrypt0 (Diagnostic Notation)"}

~~~
{::include examples/cose/HPKE-8-hex.txt}
~~~
{: title="HPKE-8 COSE_Encrypt0 (Hex-Encoded CBOR)"}

## HPKE-8-KE
{: toc="exclude"}

~~~ cbor-diag
{::include examples/cose-keys/HPKE-8-KE-diag.txt}
~~~
{: title="HPKE-8-KE COSE_Key (Diagnostic Notation)"}

~~~
{::include examples/cose-keys/HPKE-8-KE-hex.txt}
~~~
{: title="HPKE-8-KE COSE_Key (Hex-Encoded CBOR)"}

~~~ cbor-diag
{::include examples/cose/HPKE-8-KE-diag.txt}
~~~
{: title="HPKE-8-KE COSE_Encrypt (Diagnostic Notation)"}

~~~
{::include examples/cose/HPKE-8-KE-hex.txt}
~~~
{: title="HPKE-8-KE COSE_Encrypt (Hex-Encoded CBOR)"}

## HPKE-9
{: toc="exclude"}

~~~ cbor-diag
{::include examples/cose-keys/HPKE-9-diag.txt}
~~~
{: title="HPKE-9 COSE_Key (Diagnostic Notation)"}

~~~
{::include examples/cose-keys/HPKE-9-hex.txt}
~~~
{: title="HPKE-9 COSE_Key (Hex-Encoded CBOR)"}

~~~ cbor-diag
{::include examples/cose/HPKE-9-diag.txt}
~~~
{: title="HPKE-9 COSE_Encrypt0 (Diagnostic Notation)"}

~~~
{::include examples/cose/HPKE-9-hex.txt}
~~~
{: title="HPKE-9 COSE_Encrypt0 (Hex-Encoded CBOR)"}

## HPKE-9-KE
{: toc="exclude"}

~~~ cbor-diag
{::include examples/cose-keys/HPKE-9-KE-diag.txt}
~~~
{: title="HPKE-9-KE COSE_Key (Diagnostic Notation)"}

~~~
{::include examples/cose-keys/HPKE-9-KE-hex.txt}
~~~
{: title="HPKE-9-KE COSE_Key (Hex-Encoded CBOR)"}

~~~ cbor-diag
{::include examples/cose/HPKE-9-KE-diag.txt}
~~~
{: title="HPKE-9-KE COSE_Encrypt (Diagnostic Notation)"}

~~~
{::include examples/cose/HPKE-9-KE-hex.txt}
~~~
{: title="HPKE-9-KE COSE_Encrypt (Hex-Encoded CBOR)"}

## HPKE-10
{: toc="exclude"}

~~~ cbor-diag
{::include examples/cose-keys/HPKE-10-diag.txt}
~~~
{: title="HPKE-10 COSE_Key (Diagnostic Notation)"}

~~~
{::include examples/cose-keys/HPKE-10-hex.txt}
~~~
{: title="HPKE-10 COSE_Key (Hex-Encoded CBOR)"}

~~~ cbor-diag
{::include examples/cose/HPKE-10-diag.txt}
~~~
{: title="HPKE-10 COSE_Encrypt0 (Diagnostic Notation)"}

~~~
{::include examples/cose/HPKE-10-hex.txt}
~~~
{: title="HPKE-10 COSE_Encrypt0 (Hex-Encoded CBOR)"}

## HPKE-10-KE
{: toc="exclude"}

~~~ cbor-diag
{::include examples/cose-keys/HPKE-10-KE-diag.txt}
~~~
{: title="HPKE-10-KE COSE_Key (Diagnostic Notation)"}

~~~
{::include examples/cose-keys/HPKE-10-KE-hex.txt}
~~~
{: title="HPKE-10-KE COSE_Key (Hex-Encoded CBOR)"}

~~~ cbor-diag
{::include examples/cose/HPKE-10-KE-diag.txt}
~~~
{: title="HPKE-10-KE COSE_Encrypt (Diagnostic Notation)"}

~~~
{::include examples/cose/HPKE-10-KE-hex.txt}
~~~
{: title="HPKE-10-KE COSE_Encrypt (Hex-Encoded CBOR)"}

## HPKE-11
{: toc="exclude"}

~~~ cbor-diag
{::include examples/cose-keys/HPKE-11-diag.txt}
~~~
{: title="HPKE-11 COSE_Key (Diagnostic Notation)"}

~~~
{::include examples/cose-keys/HPKE-11-hex.txt}
~~~
{: title="HPKE-11 COSE_Key (Hex-Encoded CBOR)"}

~~~ cbor-diag
{::include examples/cose/HPKE-11-diag.txt}
~~~
{: title="HPKE-11 COSE_Encrypt0 (Diagnostic Notation)"}

~~~
{::include examples/cose/HPKE-11-hex.txt}
~~~
{: title="HPKE-11 COSE_Encrypt0 (Hex-Encoded CBOR)"}

## HPKE-11-KE
{: toc="exclude"}

~~~ cbor-diag
{::include examples/cose-keys/HPKE-11-KE-diag.txt}
~~~
{: title="HPKE-11-KE COSE_Key (Diagnostic Notation)"}

~~~
{::include examples/cose-keys/HPKE-11-KE-hex.txt}
~~~
{: title="HPKE-11-KE COSE_Key (Hex-Encoded CBOR)"}

~~~ cbor-diag
{::include examples/cose/HPKE-11-KE-diag.txt}
~~~
{: title="HPKE-11-KE COSE_Encrypt (Diagnostic Notation)"}

~~~
{::include examples/cose/HPKE-11-KE-hex.txt}
~~~
{: title="HPKE-11-KE COSE_Encrypt (Hex-Encoded CBOR)"}

## HPKE-12
{: toc="exclude"}

~~~ cbor-diag
{::include examples/cose-keys/HPKE-12-diag.txt}
~~~
{: title="HPKE-12 COSE_Key (Diagnostic Notation)"}

~~~
{::include examples/cose-keys/HPKE-12-hex.txt}
~~~
{: title="HPKE-12 COSE_Key (Hex-Encoded CBOR)"}

~~~ cbor-diag
{::include examples/cose/HPKE-12-diag.txt}
~~~
{: title="HPKE-12 COSE_Encrypt0 (Diagnostic Notation)"}

~~~
{::include examples/cose/HPKE-12-hex.txt}
~~~
{: title="HPKE-12 COSE_Encrypt0 (Hex-Encoded CBOR)"}

## HPKE-12-KE
{: toc="exclude"}

~~~ cbor-diag
{::include examples/cose-keys/HPKE-12-KE-diag.txt}
~~~
{: title="HPKE-12-KE COSE_Key (Diagnostic Notation)"}

~~~
{::include examples/cose-keys/HPKE-12-KE-hex.txt}
~~~
{: title="HPKE-12-KE COSE_Key (Hex-Encoded CBOR)"}

~~~ cbor-diag
{::include examples/cose/HPKE-12-KE-diag.txt}
~~~
{: title="HPKE-12-KE COSE_Encrypt (Diagnostic Notation)"}

~~~
{::include examples/cose/HPKE-12-KE-hex.txt}
~~~
{: title="HPKE-12-KE COSE_Encrypt (Hex-Encoded CBOR)"}

## HPKE-13
{: toc="exclude"}

~~~ cbor-diag
{::include examples/cose-keys/HPKE-13-diag.txt}
~~~
{: title="HPKE-13 COSE_Key (Diagnostic Notation)"}

~~~
{::include examples/cose-keys/HPKE-13-hex.txt}
~~~
{: title="HPKE-13 COSE_Key (Hex-Encoded CBOR)"}

~~~ cbor-diag
{::include examples/cose/HPKE-13-diag.txt}
~~~
{: title="HPKE-13 COSE_Encrypt0 (Diagnostic Notation)"}

~~~
{::include examples/cose/HPKE-13-hex.txt}
~~~
{: title="HPKE-13 COSE_Encrypt0 (Hex-Encoded CBOR)"}

## HPKE-13-KE
{: toc="exclude"}

~~~ cbor-diag
{::include examples/cose-keys/HPKE-13-KE-diag.txt}
~~~
{: title="HPKE-13-KE COSE_Key (Diagnostic Notation)"}

~~~
{::include examples/cose-keys/HPKE-13-KE-hex.txt}
~~~
{: title="HPKE-13-KE COSE_Key (Hex-Encoded CBOR)"}

~~~ cbor-diag
{::include examples/cose/HPKE-13-KE-diag.txt}
~~~
{: title="HPKE-13-KE COSE_Encrypt (Diagnostic Notation)"}

~~~
{::include examples/cose/HPKE-13-KE-hex.txt}
~~~
{: title="HPKE-13-KE COSE_Encrypt (Hex-Encoded CBOR)"}

<!-- end:cose-test-vectors -->

# Acknowledgments
{:numbered="false"}

Thanks to Ilari Liusvaara and Orie Steele for the discussion and comments.

# Document History
{:numbered="false"}

draft-reddy-cose-hpke-pq-pqt-00

- Replaces draft-reddy-cose-jose-pqc-hybrid-hpke
- Removed ChaCha20Poly1305 AEAD ciphersuites
- Adapted source from draft-skokan-jose-hpke-pq-pqt-04 for COSE
- Added Filip Skokan as author
