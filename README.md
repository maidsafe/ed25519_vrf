ed25519_vfr

Ed25519 Verifiable Random Function (VRF)

A VRF (Verifiable Random Function) is a cryptographic primitive that combines the properties of a digital signature and a pseudorandom function (PRF). It allows the generation of a random output and a proof for a given input while ensuring that the output can only be produced by the secret key holder.

One popular construction of a VRF using Ed25519 is the Edwards-curve VRF (ECVRF), which leverages the elliptic curve properties and the Ed25519 signature scheme. ECVRF provides both the pseudorandomness and uniqueness properties required for a VRF while also benefiting from the security and performance of the Ed25519 signature scheme.

This library uses ed25519_dalek v2 to make use of the verify_strict method on Ed25519 signatures.

ED25519 verify_strict (from the dalek docs)

verify_strict strictly verifies a signature on a message with the keypair's public key.

On The (Multiple) Sources of Malleability in Ed25519 Signatures, this version of verification is technically non-RFC8032 compliant. The following explains why.

Scalar Malleability
The authors of RFC8032 explicitly stated that verification of an Ed25519 signature must fail if the scalar s is not properly reduced mod ℓ:

To verify a signature on a message M using public key A, with F being 0 for Ed25519ctx, 1 for Ed25519ph, and if Ed25519ctx or Ed25519ph is being used, C being the context, first split the signature into two 32-octet halves. Decode the first half as a point R, and the second half as an integer S, in the range 0 <= s < L. Decode the public key A as point A’. If any of the decodings fail (including S being out of range), the signature is invalid.

All verify_*() functions within ed25519-dalek perform this check.

Point Malleability
The authors of RFC8032 added a malleability check to step #3 in §5.1.7, for small torsion components in the R value of the signature, which is not strictly required, as they state:

Check the group equation [8][S]B = [8]R + [8][k]A’. It’s sufficient, but not required, to instead check [S]B = R + [k]A’.
