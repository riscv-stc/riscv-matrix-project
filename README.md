# RISC-V Matrix Project

This is a demo project for matrix extension proposal.

Modules are shown below.

* riscv-matrix-spec:   The matrix extension proposal
* llvm-project:        llvm toolchain to support matrix extension proposal
* riscv-isa-sim:       Spike ISS to support matrix extension proposal
* chipyard:            Chipyard project to support matrix extension proposal with BOOM Core
* riscv-pvp-matrix:    RISC-V Matrix Extension ISA Verification using RISC-V PVP
* riscv-dnn:           A small DNN library for RISC-V, using RISC-V Vector and Matrix extensions

## Quick start

prepare and clone repos.

    $ export RISCV=~/opt/riscv
    $ export PATH=$RISCV/bin:$PATH

    $ git clone https://github.com/riscv-stc/riscv-matrix-project
    $ cd riscv-matrix-project
    $ git submodule update --init

compile and install llvm toolchain

    $ cd llvm-project
    $ mkdir -p build && cd build
    $ cmake -DCMAKE_INSTALL_PREFIX=$RISCV \
        -DCMAKE_BUILD_TYPE=Release -DLLVM_OPTIMIZED_TABLEGEN=On \
        -DLLVM_ENABLE_PROJECTS="clang;compiler-rt;lld" \
        -DLLVM_LINK_LLVM_DYLIB=On ../llvm
    $ make -j`nproc` && make install
    $ cd ../..

compile and install gnu toolchain

    $ cd riscv-gnu-toolchain
    $ git submodule update --init --recursive
    $ mkdir -p build && cd build && ../configure --prefix=$RISCV
    $ make -j`nproc`
    $ cd ../..

compile and install spike ISS.

    $ cd riscv-isa-sim
    $ mkdir -p build && cd build && ../configure --prefix=$RISCV
    $ make -j && make install
    $ cd ../..

run riscv-dnn on spike for dnn operator and resnet-50 tests.

    $ cd riscv-dnn
    $ pip3 install -r requirement.txt

    $ cd test/ops/
    $ make -j8
    $ cd ../..

    $ cd test/models/resnet50/
    $ python3 gen_weight_bin.py
    $ python3 gen_input_bin-fp16.py
    $ python3 gen_header.py
    $ python3 test_resnet50.py

## TODO

* update modules to support latest version of proposal.
* add chipyard simulation & verification & prototype documents.
