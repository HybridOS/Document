## Introduction

The UEFI 2.2 specification adds a protocol known as secure boot, which can secure the boot process by preventing the loading of drivers or OS loaders that are not signed with an acceptable digital signature. 

> ###### Standard UEFI Implement
> When secure boot is enabled, it is initially placed in "setup" mode, which allows a public key known as the "Platform key" (PK) to be written to the firmware. Once the key is written, secure boot enters "User" mode, where only drivers and loaders signed with the platform key can be loaded by the firmware. Additional "Key Exchange Keys" (KEK) can be added to a database stored in memory to allow other certificates to be used, but they must still have a connection to the private portion of the Platform key. Secure boot can also be placed in "Custom" mode, where additional public keys can be added to the system that do not match the private key.

## Existing Secure Boot Project 

- Shim
- Preloader

## Feature
The principle is very similar to PreLoader. The difference is that we will be based on the localization algorithm.  In addition, we also redefine some of the details of the validation process.
- TCM or other cryptographic device

## Development
