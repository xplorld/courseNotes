# review of slide 01 - 09

## Control Hijacking Attacks

– Buffer overflow attacks
– Integer overflow attacks
– Format string vulnerabilities

## runtime checking

### stackguard

add canary before locals

- random canary: gen at program begin
- \0 canary

### NX: OS help: forbid on-stack code run

### tainting

taint input data to check control flow

## CFI

build CFG at compile time, instrument: add tag at jmp

context sensitivity: add shadow call stack

### model checking



- buffer overflow
- web attack

1. Run-time Security Enforcement Topics
2. Security Analysis of Software
 3. Software Security Architectures 
 4. Language-based Security