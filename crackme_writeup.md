# CrackMe Writeup

## Overview

This crackme validated user input by computing its SHA-256 hash and
comparing it to a hardcoded target hash:

    811eb81b9d11d65a36c53c3ebdb738ee303403cb79d781ccf4b40764e0a9d12a

The goal was to determine the correct password.

------------------------------------------------------------------------

## Step 1: Static Analysis

Opening the binary in a disassembler (Ghidra/Radare/objdump) revealed a
function that:

1.  Reads user input.
2.  Computes `SHA256(input)`.
3.  Compares the resulting hash to the above constant.
4.  Prints success if the hash matches.

There was no obfuscation or custom hashing; it used standard SHA-256.

------------------------------------------------------------------------

## Step 2: Hash-Reversal Attempt With Brute Force

Instead of reversing the hash (impossible mathematically), I tried
brute-forcing and wordlist attacks:

-   Tried small brute-force scripts (Python `itertools`)
-   Tried common password lists using Hashcat

Because the crackme used a simple SHA256 hash and no salt, it is ideal
for wordlists.

------------------------------------------------------------------------

## Step 3: Cracking With Hashcat

I placed the hash into a file (`hash.txt`) and used the common
`rockyou.txt` wordlist:

    hashcat -m 1400 hash.txt rockyou.txt

Hashcat quickly identified the matching plaintext:

    chicken

This indicates the author used a very common password.

------------------------------------------------------------------------

## Step 4: Verification

Running the binary:

    $ ./crackme
    Enter password: chicken
    You win :)

The program accepted the input.

------------------------------------------------------------------------

## Conclusion

The crackme was solved by recognizing that the binary simply compared
SHA-256 hashes, extracting the target hash, and using a wordlist attack
to recover the password.

**Final Password:** `chicken`

This writeup avoids patching or distributing unneeded files, following
the crackme rules.
