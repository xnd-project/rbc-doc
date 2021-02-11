# RBC Documentation

## Introduction

The purpose of the RBC project is to implement the concept of Remote
Backend Compiler. The concept of RBC is about splitting the
compilation of an user-provided program source code to machine
executable instructions in between two different computer systems - a
client and a server - using the following workflow:

* In a client, the user-provided source code of some function is
  compiled to a LLVM IR string.

* The LLVM IR string together with the function metadata is sent to a
  server where it will be registered and made available for execution
  in the server.
