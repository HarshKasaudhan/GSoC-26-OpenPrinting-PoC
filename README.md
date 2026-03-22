# GSoC 2026 - OpenPrinting Preliminary Evaluation & PoC

**Candidate:** Harsh Kasaudhan  
**Target Project:** System-Level Fuzzing for Parsing Features in OpenPrinting Projects  
**Mentors:** Till Kamppeter, Jiongchi Yu, Günther Noack  

This repository contains the Proof of Concept (PoC) and screenshots for the preliminary evaluation tasks assigned for the GSoC 2026 OpenPrinting project. All tasks were successfully executed locally on an Ubuntu WSL2 environment.

---

## Task 1: Running `fuzz_ipp` in OSS-Fuzz Locally

I successfully set up the Google OSS-Fuzz Docker infrastructure and compiled the `cups` project to run the existing `fuzz_ipp` harness.

**Command Executed:**
```bash
python3 infra/helper.py run_fuzzer cups fuzz_ipp
```
Result: The libFuzzer successfully initialized and started discovering new paths.
(See Task1_OSSFuzz.png for the live execution output).


## Task 2: Fuzzing CUPS with AFL++
To demonstrate manual instrumentation and system-level fuzzing, I compiled the OpenPrinting/cups repository from source using afl-clang-fast. I then created a seed corpus and initiated a fuzzing campaign on the network-based ipptool utility.

**Compilation:**
```bash
Bash
CC=afl-clang-fast CXX=afl-clang-fast++ ./configure --disable-shared --disable-systemd
make -j$(nproc)
```
**Fuzzer Execution:**
```bash
Bash
AFL_I_DONT_CARE_ABOUT_MISSING_CRASHES=1 afl-fuzz -i inputs/ -o outputs/ ./tools/ipptool ipp://127.0.0.1/ @@
```
Result: The AFL++ dashboard successfully launched, tracking map coverage, cycle progress, and execution speed.
(See Task2_AFL_CUPS.png for the AFL++ dashboard output).

I am actively working on expanding this infrastructure to fuzz full filter pipelines (like pdftops) by directly feeding raw mutated PDF files, as outlined in my GSoC proposal.
