This is a sample project for setting up development environment for EOS contracts (Dapp) with the CLion IDE, it can be used as a template.

The sample contract ```currency``` is copied from the [EOS project](), and the same as ```wasm.make```.

### Pre-condition

- EOS Dawn 2.x has been built successfully and has run make install

### Import the project to CLion

##### Clone the github project:
```
git clone https://github.com/eosapp/sample-contract-clion.git
```

##### Import to CLion
In CLion IDEA, select File ➜ Import Project... to open the project.

After import the project successfully, go to ```CLion ➜ Preferences ➜ Build, Execution, Deployment ➜ CMake``` and make following changes:

- Set Build type to ```Release```.
- Set CMake options to ```-DCMAKE_TOOLCHAIN_FILE=wasm.cmake```

You might need to modify the ```wasm.cmake``` and ```CMakeLists.txt``` for your local environment:


```
#wasm.cmake

set(WASM_TOOLCHAIN FALSE)
set(BINARYEN_ROOT /usr/local/binaryen)
set(ABIGEN $ENV{HOME}/eos/build/install/bin/abi_gen)
```


```
#CMakeLists.txt

set(target currency)
set(EOSIO_INSTALL_DIR $ENV{HOME}/eos/build/install)
set(ABIGEN ${EOSIO_INSTALL_DIR}/bin/abi_gen)
```

#### Build in CLion
Select ```Run ➜ Build```, you should see below in CLion console:

```
/Applications/CLion.app/Contents/bin/cmake/bin/cmake --build /workspace/sample-contract-clion/cmake-build-release --target assembly -- -j 2
Scanning dependencies of target currency
[ 16%] Building LLVM bitcode currency.cpp.bc
[ 33%] Linking LLVM bitcode currency.bc
[ 50%] Generating textual assembly currency.s
[ 66%] Generating WAST currency.wast
[ 83%] Generating currency.wast.hpp
[ 83%] Built target currency
Scanning dependencies of target assembly
[100%] Generated /workspace/sample-contract-clion/cmake-build-release/currency.abi
Adding type currency_tokens (currency_tokens)
Adding type transfer (currency::transfer)
Adding type account (currency::account)
[100%] Built target assembly
```

And you can see all built files are in ```cmake-build-release``` folder:

```
cmake-build-release >  ls -alrt
total 296
-rw-r--r--   1 someone  staff   2824  3 20 19:12 currency.hpp
-rw-r--r--   1 someone  staff  21386  3 20 19:12 CMakeCache.txt
-rw-r--r--   1 someone  staff   4793  3 20 19:12 Makefile
-rw-r--r--   1 someone  staff   1263  3 20 19:12 cmake_install.cmake
-rw-r--r--   1 someone  staff   4572  3 20 19:12 hello.cbp
-rw-r--r--   1 someone  staff   6720  3 20 19:12 currency.cpp.bc
-rw-r--r--   1 someone  staff   6668  3 20 19:12 currency.bc
-rw-r--r--   1 someone  staff  20712  3 20 19:12 currency.s
-rw-r--r--   1 someone  staff  25246  3 20 19:12 currency.wast
-rw-r--r--   1 someone  staff  25292  3 20 19:12 currency.wast.hpp
drwxr-xr-x  14 someone  staff    448  3 20 19:12 .
-rw-r--r--   1 someone  staff    751  3 20 19:12 currency.abi
drwxr-xr-x  18 someone  staff    576  3 20 19:12 CMakeFiles
drwxr-xr-x  10 someone  staff    320  3 20 19:15 ..
```

## Here you go!



