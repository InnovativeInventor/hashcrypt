=========
hashcrypt
=========

Modular block encryption based upon a secure hash function. Very experimental, not suitable for production.

Round Description
=================

Given a key :math:`k`, for each 256-bit block in the plaintext, :math:`p_1, p_2, p_3` . . .

the ciphertext :math:`c_1 := H(k) xor p_1` and each encrypted block is defined recursively:

.. math::
    c_n := H(n|H(n|c_{n-1})|H(n|k)) xor p_n 

where H(x) is a secure hash function.

Note: :math:`c_n = R(p_n).`

Iterating rounds
================
Rounds can be successively applied like so, up until the :math:`nth` round where :math:`n` less than or equal to the number of blocks needed to encrypt:

Round 1: :math:`R(p_1), R(p_2), R(p_3)` . . .

Round 2: :math:`R(p_1), R(R(p_2)), R(R(p_3))` . . .

Round n: :math:`R(p_1), R(R(R(p_2)))` . . .

Security
========
The security of this cipher scheme depends on the preimage resistance of :math:`H(x)` in known-plaintext attacks. Additionally, it assumes that from :math:`H(x)` or :math:`H(k|x)` for any :math:`k ≠ n`, it is impossible to predict :math:`H(n|x)`.

To construct an ideal H(x), you can combine multiple hash functions from various families like so:

.. math::
    H(x) = keccak(x) xor BLAKE(BLAKE2(BLAKE3(x))) xor SHA256(x)

Rationale and considerations for constructing H(x):
- provides security, even if one of the families of hash functions is thoroughly broken
- various hashes have a limited output space, xoring multiple different families helps to increase the output space to be closer to 256-bit
- makes it extremely difficult to undo.

Implementation
==============
Coming soon.
