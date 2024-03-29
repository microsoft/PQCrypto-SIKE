# Warning

The SIKE protocol was proven insecure after a series of attacks starting with a paper by [`Castryck and Decru`](https://eprint.iacr.org/2022/975). 
Therefore, SIKE MUST NOT be used in production.

The software in this repository is only presented for historical reasons, and because some functionality may be reusable in other cryptographic applications.

## This software is part of "Supersingular Isogeny Key Encapsulation" [1], a submission to the NIST Post-Quantum Standardization project.
It contains a exact copy of the optimized implementation and the additional implementations for x64 developed by Microsoft Research.

Other software included in the submission, but copyrighted by others, is not included.

[1] Reza Azarderakhsh, Matthew Campagna, Craig Costello, Luca De Feo, Basil Hess, Amir Jalali, David Jao, Brian Koziel, Brian LaMacchia, Patrick Longa, Michael Naehrig, Joost Renes, Vladimir Soukharev, and David Urbanik, "Supersingular Isogeny Key Encapsulation". Submission to the NIST Post-Quantum Standardization project (to appear soon), 2017.

# Supersingular Isogeny Key Encapsulation software

This document describes basic instructions to compile and test the
software implementations accompanying the Supersingular Isogeny Key
Encapsulation (SIKE) submission.

All of the code is available under the MIT License. 

- The implementations in the folders "Reference_Implementation" and
  "Additional_Implementations/SIKE-Weierstrass" were developed by Basil
  Hess (InfoSec Global).
- The implementations in the folders "Additional_Implementations/x64"
  and "Optimized_Implementation" were developed by Microsoft Research.
- The ARM64 implementation in "Additional_Implementations/ARM64" was
  developed by Matthew Campagna (Amazon).
- The VHDL implementation in "Additional_Implementations/VHDL" was
  developed by Brian Koziel (Texas Instruments) and Reza Azarderakhsh
  (Florida Atlantic University).


## CONTENT

The SIKE submission includes implementations for three security levels.
The corresponding schemes are called SIKEp503, SIKEp751, and SIKEp964.

The media folder accompanying this submission contains the following
folders:
 
- KAT/: KAT values generated with the code ("PQCgenKAT_kem.c") provided
  by NIST.
- Reference_Implementation/: simple and portable implementation
  exclusively written in ANSI C.
- Optimized_Implementation/portable/<SIKEp#>/: portable implementations
  optimized for speed and exclusively written in ANSI C.
- Additional_Implementations/x64/<SIKEp#>/: optimized, supplementary
  implementations exploiting x64 assembly.
- Additional_Implementations/ARM64/<SIKEp#>/: optimized, supplementary
  implementations exploiting ARM64 assembly.
- Additional_Implementations/VHDL/SIKEp751/: optimized VHDL model,
  implemented in both FPGA and ASIC 
- Additional_Implementations/SIKE-Weierstrass/: simple, textbook
  implementation using elliptic curves in short Weierstrass form
- Supporting_Documentation/: Specification of the SIKE protocol.

In this README, we describe basic instructions to compile and test the
software implementations in the folders "Optimized_Implementation" and
"Additional_Implementations".

### COMPLEMENTARY CRYPTO FUNCTIONS

Random values are generated with /dev/urandom. Check the folder
<implementation>/<SIKEp#>/random for details.


## QUICK INSTRUCTIONS

Instructions in this section apply only to the optimized implementation
and the assembly-optimized implementations.  Regarding the reference
implementation, refer to the README file in the Reference_Implementation
folder.

<SIKEp#> refers to any of {SIKEp503, SIKEp751}. The SIKEp964 parameter
set is only supported by the reference implementation, and is not
supported by the optimized implementations.

<implementation> refers to any of {Optimized_Implementation/portable,
Additional_Implementations/x64, Additional_Implementations/ARM64}.

Pick a given implementation and scheme, and then do:

$ cd <implementation>/<SIKEp#>
$ make clean
$ make

Testing and benchmarking results are obtained by running:

$ ./sike/test_KEM

To run the implementations against the KATs provided in the KAT folder,
execute:

$ ./sike/PQCtestKAT_kem

These instructions are intended for x64 platforms by default.
Compilation is performed with GNU GCC by default. To change these
values, use compilation options as described in the next section.


### ADDITIONAL OPTIONS

For the optimized implementations, we have the following compilation
options: 

make CC=[gcc/clang] ARCH=[x64/x86/ARM/ARM64] SET=EXTENDED

For the additional assembly-optimized implementations, we have the
following compilation options:

make CC=[gcc/clang] SET=EXTENDED USE_MULX=[TRUE/FALSE] USE_ADX=[TRUE/FALSE]

Setting "SET=EXTENDED" adds the flags -fwrapv -fomit-frame-pointer
-march=native. The use of mulx and adx instructions is included by
default in the additional implementations. The user is responsible for
checking if these instructions are supported in the targeted platform
and then setting the corresponding flags above accordingly.

Note: USE_ADX can only be set to TRUE if USE_MULX=TRUE.


## LICENSE

The SIKE software is licensed under the MIT License; see License.txt for
details.  It includes some third party modules that are licensed
differently. In particular:

- <implementation>/<SIKEp#>/tests/aes/aes.c: public domain
- <implementation>/<SIKEp#>/tests/aes/aes_c.c: public domain
- <implementation>/<SIKEp#>/tests/rng/rng.c: copyrighted by Lawrence E.
  Bassham 
- <implementation>/<SIKEp#>/tests/PQCtestKAT_kem.c: copyrighted by
  Lawrence E. Bassham 
- <implementation>/<SIKEp#>/sha3/fips202.c: public domain


## Contributors

Other contributors include:

- Joost Renes, while he was an intern with Microsoft Research.
