# Very Smooth

Cryptography - 300p

## Abstract

We use pollard p-1 factoring, leveraging the fact that the generation of the primes are smooth, using a tool found on GitHub.

## Introduction

We are provided with [gen.py](gen.py), and output data [output.txt](output.txt). After analysis of the code, we understand that the 2 prime numbers are a [smooth number](https://en.wikipedia.org/wiki/Smooth_number)+1 prime. This fits the requirements for pollard p-1 factoring. 

## Attack

We use [ZeroBone/PollardRsaCracker](https://github.com/ZeroBone/PollardRsaCracker) to factor this, but it's not readily apparent the biggest prime, until we look at the smoothness. The smoothness is set to 16 bits, meaning that the largest prime is likely 65537, however, 65537 is filtered out as that is our e, meaning that the largest prime is likely to be and no greater than 65521. Entering that as our 'b', and looking through the code for `e=65537`, we decode this ciphertext, giving us the plaintext: `38251710328773353863707413799071193053053` in decimal. We convert this to hexadecimal, then use the python code `bytearray.fromhex("7069636f4354467b33373665626665377d").decode()` to give us the final flag
> picoCTF{376ebfe7}