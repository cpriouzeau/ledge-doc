.. SPDX-License-Identifier: CC-BY-SA-4.0

*******************
About This Document
*******************

Introduction
============

The LEDGE documentation describes instructions to build, install
and use varios features included in LEDGE linux reference platfrom. 

Comments or change requests can be sent to team-ledge@linaro.org.

Guiding Principles
==================

This documetation describes how to build fully open source version of LEDGE 
reference platfrom and run it.

Scope
=====

LEDGE refernece platfrom is intended for IoT and EDGE devices. So that support
of hight level of security takes major place. Document only describes hight level
of features usage and impmenetation details. This document does not provide full
technical documentation about specific features.

Cross References
================
This document cross-references sources that are listed in the References
section by using the section sign §.

Examples:

UEFI § 6.1 - Reference to the UEFI specification [UEFI]_ section 6.1

Terms and abbreviations
=======================

This document uses the following terms and abbreviations.

.. glossary::

   A64
      The 64-bit Arm instruction set used in AArch64 state.
      All A64 instructions are 32 bits.

   AArch32
      Arm 32-bit architectures. AArch32 is a roll up term referring to all
      32-bit versions of the Arm architecture starting at ARMv4.

   AArch64 state
      The Arm 64-bit Execution state that uses 64-bit general purpose
      registers, and a 64-bit program counter (PC), Stack Pointer (SP), and
      exception link registers (ELR).

   AArch64
      Execution state provides a single instruction set, A64.

   EFI Loaded Image
      An executable image to be run under the UEFI environment,
      and which uses boot time services.

   EL0
      The lowest Exception level on AArch64. The Exception level that is used to execute
      user applications, in Non-secure state.

   EL1
      Privileged Exception level on AArch64. The Exception level that is used to execute
      Operating Systems, in Non-secure state.

   EL2
      Hypervisor Exception level on AArch64. The Exception level that is used to execute
      hypervisor code. EL2 is always in Non-secure state.

   EL3
      Secure Monitor Exception level on AArch64. The Exception level that is used to
      execute Secure Monitor code, which handles the transitions between
      Non-secure and Secure states.  EL3 is always in Secure state.

   Logical Unit (LU)
      A logical unit (LU) is an externally addressable, independent entity
      within a device. In the context of storage, a single device may use
      logical units to provide multiple independent storage areas.

   OEM
      Original Equipment Manufacturer. In this document, the final device
      manufacturer.

   SiP
      Silicon Partner. In this document, the silicon manufacturer.

   UEFI
      Unified Extensible Firmware Interface.

   UEFI Boot Services
      Functionality that is provided to UEFI Loaded Images during the UEFI boot
      process.

   UEFI Runtime Services
      Functionality that is provided to an Operating System after the
      ExitBootServices() call.
