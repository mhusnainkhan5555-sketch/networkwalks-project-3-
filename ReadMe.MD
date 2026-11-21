PDF Password Auditing & Offline Dictionary Attacks (Week 3 Labs)

Hands-on lab documentation covering cryptographic hash extraction and offline dictionary attacks against encrypted PDF documents. Completed as part of the Networkwalks Cybersecurity & Ethical Hacking project modules (W3-PM1 & W3-PM2).

Objective

Extract the standard security handler hash from a password-protected PDF (My Locked PDF1.pdf) and recover the document-open password using two distinct toolchains:

Module 1 (W3-PM1): Local CLI/GUI auditing via John the Ripper (Jumbo) and Johnny.

Module 2 (W3-PM2): Client-side browser auditing via Networkwalks Hash Calculator and Password Cracker.

Cryptographic & Technical Breakdown

1. Hash Extraction & Metadata Analysis

Offline password recovery does not attack the PDF file directly; instead, it isolates the encryption dictionary metadata into a crackable digest (pdf2john / hashcat format):

$pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*32*...


Format Identifier ($pdf$4*4*128): Indicates PDF Version 4, Revision 4 security handler using a 128-bit key length.

Extracted Parameters: Captures the permissions integer (-1060), document ID/salt, and the user/owner password verification strings required to test candidate passwords offline.

2. Attack Execution

Method A — John the Ripper / Johnny (W3-PM1):

Configured the Johnny GUI wrapper to point to the john.exe (1.9.0-jumbo-1) binary.

Loaded the extracted $pdf$ digest (hash1.txt) and executed a wordlist attack using JtR's default ruleset.

Method B — Browser-Based Auditing (W3-PM2):

Parsed My Locked PDF1.pdf locally in the browser using the Networkwalks Hash Calculator to verify the $pdf$ string without server-side file uploads.

Ran a dictionary attack against the digest using the Networkwalks Password Cracker.

3. Results

Target File: My Locked PDF1.pdf

Used alternate files as wells

Recovered Password: password1

Verification: Unlocked the protected document to retrieve the target flag (networkwalks_flag1.pdf).

Operational Security & Defensive Takeaways

Absence of Rate-Limiting Offline: Once an encrypted file leaves a controlled environment, authentication throttling no longer applies. An attacker with local access to the file can test millions of candidates per second depending on hardware.

Entropy Over Algorithm Strength: Although the target PDF utilized 128-bit encryption, the underlying password (password1) resided in standard dictionaries, reducing time-to-crack to seconds. Strong encryption requires high-entropy passphrases (12+ mixed characters) to withstand offline dictionary and rule-based attacks.

OPSEC During Hash Extraction: In real-world engagements, sensitive documents should never be uploaded to third-party online extractors. Using local utilities (pdf2john.pl / pdf2john.py) or client-side Web Crypto tools prevents accidental data exposure.



Tools Used

John the Ripper (1.9.0-jumbo-1) & Johnny GUI (v2.2)

NetworkWalks Tools

Networkwalks Hash Calculator & Password Cracker

Adobe Acrobat Reader (Verification)
