# Diffie-Hellman

Cryptography - 200p

## Introduction

The challenge title refers to "Diffie-Hellman" key exchange. Due to some complications in the way this problem was written, this problem will not be moving to the picoGym.

## Challenge description

>Alice and Bob wanted to exchange information secretly. The two of them agreed to use the Diffie-Hellman key exchange algorithm, using p = 13 and g = 5. They both chose numbers secretly where Alice chose 7 and Bob chose 3. Then, Alice sent Bob some encoded text (with both letters and digits) using the generated key as the shift amount for a Caesar cipher over the alphabet and the decimal digits. Can you figure out the contents of the message? Download the message here. Wrap your decrypted message in the picoCTF flag format like: picoCTF{decrypted_message}

## Solution

Message: `H98A9W_H6UM8W_6A_9_D6C_5ZCI9C8I_D9FF6IFD`

Using a diffie-hellman calculator [such as this one](https://www.irongeek.com/diffie-hellman.php), we get the key is 5. Reversing the Caesar Cipher with an alphanumeric caesar cipher reverser [such as this one](https://crypto.interactive-maths.com/caesar-shift-cipher.html), with the alphabet set to including numbers gave the flag `c4354r_c1ph3r_15_4_817_0u7d473d_84aa1da8`, we wrapped it in `picoCTF{}`, and voila.
