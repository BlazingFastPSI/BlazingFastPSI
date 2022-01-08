## Private Set Intersection with Blazing Fast Online Phase
This is the source code repository for the
*Private Set Intersection with Blazing Fast Online Phase* paper.

While development and all benchmarks have been made under Linux, you will also be able to compile and run under Mac OS X (Big Sur, using Homebrew). 

## Dependencies
* Development was done with `gcc` in version 10 under Linux. We support Apple's `clang-1300.0.29.30` for most of the code (see below). To compile, you also need OpenSSL and GMP libraries.

* For SGX, you need Intel's development toolkit, driver for your kernel etc. installed working and the architectural enclave service manager running.

* To precisely control bandwith between parties, we use Wondershaper.  To reproduce the paper's bandwidth settings, you can use scripts `5g, 1g, 100m, 10m` found in the `src`directory.


## Preparation
* Compile the EMP and SEAL submodules. We assume that you build them in place using `cmake .` in the respective submodule directories.
* As the exact value of $n$ impacts several aspects of the source code, you need to edit `include.h` and set `#include "include-20.h"` to reflect your value of $\log{}n$. So, `#include "include-20.h"` will set $\log{}n=20$, `#include "include-22.h"` will set $\log{}n=22$ etc. That is, you switch between different $n$ by selecting the proper include file in `include.h`. We provide include files for $\log{}n$ between 20 and 26 as in the paper.
* Enter the source directory (`cd src`). 

## OT Offline Phase
```
make ot
./run_ot_offline
``` 

## TTP Offline Phase
```
make opt-sender
./run_ttp_offline
```
## LBC Offline Phase
```
cd build
CMAKE_PREFIX_PATH=../../SEAL/ cmake .. #on Mac, you do OPENSSL_ROOT_DIR=/usr/local/opt/openssl CMAKE_PREFIX_PATH=../../SEAL/ cmake ..
make
./run_psi_seal
```

## SGX Offline Phase
```
cd sgx
CC=gcc-10 CXX=g++-10 make
sudo systemctl restart aesmd.service
./run_sgx
```
This does not work on Mac OS X.

## Online Phase
```
make alice
./run_psi
```
