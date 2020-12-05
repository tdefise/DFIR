# Memory

## CobaltStrikeScan

[CobaltStrikeScan](https://github.com/Apr4h/CobaltStrikeScan) scans Windows process memory for evidence of DLL injection (classic or reflective injection) and performs a YARA scan on the target process' memory for Cobalt Strike v3 and v4 beacon signatures.

### Installation

```bash
git clone --recursive https://github.com/Apr4h/CobaltStrikeScan.git
```

### Analysis

```bash
CobaltStrikeScan.exe -f PROCESS_DUMP.dmp # Scan a file/process dump for CobaltStrike beacons
```

## Get-InjectedThread (On PC)

[Get-InjectedThread](https://gist.github.com/jaredcatkinson/23905d34537ce4b5b1818c3e6405c1d2#file-get-injectedthread-ps1) searches for common memory injection techniques that *can* be caught using this method include:

- Classic Injection (OpenProcess, VirtualAllocEx, WriteProcessMemory, CreateRemoteThread)
- Reflective DLL Injection
- Memory Module

```bash
PS > Get-InjectedThread
```