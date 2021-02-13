# RBC Documentation

[Gitbook View](https://xnd-project.gitbook.io/rbc-doc), [Documentation Sources](https://github.com/xnd-project/rbc-doc), [Project homepage](https://github.com/xnd-project/rbc). 

## Overview

The purpose of the [RBC project](https://github.com/xnd-project/rbc) is to implement the concept of Remote Backend Compiler \(RBC\). The concept of RBC is about splitting the compilation of a user-provided program source code to machine-executable instructions in between two different computer systems - an RBC client and a JIT server - using the following workflow:

* In the RBC client, the user-provided source code of a program is compiled to a LLVM IR string.
* The LLVM IR string together with the program metadata is sent to a server where it will be registered and made available for execution in the JIT server.

The RBC concept can be applied in various situations. For example, the RBC enables executing client programs for analyzing or processing [Big data](https://en.wikipedia.org/wiki/Big_data) stored in a remote server when retrieving the data over the network would not be feasible due to the large size or be too inefficient.

* _LLVM IR_ is an intermediate representation of a compiled program used in the [LLVM compiler toolchain](https://llvm.org/). The low-level [LLVM IR language](https://llvm.org/docs/LangRef.html) is based on [Static Single Assignment \(SSA\)](https://en.wikipedia.org/wiki/Static_single_assignment_form) representation that many high-level languages can be compiled into. The LLVM IR can be an input to a [Just-in-time \(JIT\)](https://en.wikipedia.org/wiki/Just-in-time_compilation) compiler which will complete the compilation process resulting in a machine-executable program.

In the RBC project, the client software is implemented in Python and uses [Numba](https://github.com/numba/numba) for compiling Python functions into LLVM IR. In addition, the RBC client software can use [Clang compilers](https://clang.llvm.org/) for compiling C/C++ functions into LLVM IR as well. The RBC project provides a Python/Numba based JIT server as a prototype of the RBC concept.

As an application, the RBC client software can be used in connection with [OmniSciDB - an analytical database and SQL engine](https://www.omnisci.com/platform/omniscidb) - for run-time registration of custom SQL functions: User-Defined Functions \(UDFs\) and User-Defined Table Functions \(UDTFs\). OmniSciDB uses JIT technology that enables compiling SQL queries into machine-executable programs to be run on modern CPU and GPU hardware.

## Structure of RBC documentation

* [Getting Started](getting-started/getting-started/)
* [Developers Corner](developers-corner/developing-rbc-and-omniscidb/)

