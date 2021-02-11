# RBC Documentation

## Introduction

The RBC project introduces the concept of Remote Backend Compiler where the compilation of a user-provided source code to machine instructions to be executed is split in between two physically distant computer systems - a client and a server - using the following workflow:

* In a client, the user provides the source code of a function to the RBC software that will compile it to LLVM IR string
* The LLVM IR string together with the function properties is sent to a server where it will be registered and made available for execution in the server. 

