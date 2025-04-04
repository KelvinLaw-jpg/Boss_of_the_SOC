# Ransomware

## Scenario
After the excitement of yesterday, Alice has started to settle into her new job. Sadly, she realizes her new colleagues may not be the crack cybersecurity team that she was led
to believe before she joined. Looking through her incident ticketing queueshe notices a “critical” ticket that was never addressed. Shaking her head, she begins to investigate.
Apparently on August 24th Bob Smith (using a Windows 10 workstation named we8105desk) came back to his desk after working-out and found his speakers blaring (click below to listen), 
his desktop image changed (see below) and his files inaccessible.

Alice has seen this before... ransomware. After a quick conversation with Bob, Alice determines that Bob found a USB drive in the parking lot earlier in the day, 
plugged it into his desktop, and opened up a word document on the USB drive called "Miranda_Tate_unveiled.dotm". With a resigned sigh she begins to dig into the problem...


## Write Up

1. check sourcetype as usual: | metadata type=sourcetypes | fields sourcetype
2. 

### Q.1 What was the most likely IPv4 address of we8105desk on 24AUG2016?

### Q.2 Amongst the Suricata signatures that detected the Cerber malware, which one alerted the fewest number of times? Submit ONLY the signature ID value as the answer.
### Q.3 What fully qualified domain name (FQDN) does the Cerber ransomware attempt to direct the user to at the end of its encryption phase?
### Q.4 What was the first suspicious domain visited by we8105desk on 24AUG2016?
### Q.5 During the initial Cerber infection a VB script is run. The entire script from this execution, pre-pended by the name of the launching .exe, can be found in a field in Splunk. What is the length of the value of this field?
### Q.6 What is the name of the USB key inserted by Bob Smith?
### Q.7 Bob Smith's workstation (we8105desk) was connected to a file server during the ransomware outbreak. What is the IPv4 address of the file server?
### Q.8 How many distinct PDFs did the ransomware encrypt on the remote file server?
### Q.9 The VBscript found in question 204 launches 121214.tmp. What is the ParentProcessId of this initial launch?
### Q.10 The Cerber ransomware encrypts files located in Bob Smith's Windows profile. How many .txt files does it encrypt?
### Q.11 The malware downloads a file that contains the Cerber ransomware cryptor code. What is the name of that file?
### Q.12 Now that you know the name of the ransomware's encryptor file, what obfuscation technique does it likely use?
