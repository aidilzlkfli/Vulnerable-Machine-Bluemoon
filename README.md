
<h1>Report: Vulnerable Machine - Bluemoon</h1>

<table border="0" cellpadding="10">
  <thead>
    <tr>
      <th width="5%">Step</th>
      <th width="35%">Activity / Findings</th>
      <th width="20%">Command / Details</th>
      <th width="40%">Screenshot</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Perform network reconnaissance to discover live hosts on the local network using ARP requests</td>
      <td><code>netdiscover</code></td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/311a775d-586b-468b-b006-a45d6604ca39" /></td>
    </tr>
    <tr>
      <td>2</td>
      <td>Check the attacker's own network interface configuration to identify the active network segment and IP address</td>
      <td><code>ifconfig</code></td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/ce05f5b6-e168-4ac3-89b5-1122caf526d5" /></td>
    </tr>
    <tr>
      <td>3</td>
      <td>Perform a ping sweep to identify live hosts on the network without scanning ports</td>
      <td><code>nmap -sn</code></td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/1fe728d6-d972-4acf-8788-539ce1992f27" /></td>
    </tr>
    <tr>
      <td>4</td>
      <td>Run a detailed port scan on the target IP (172.20.10.4) to discover open ports, running services, versions, and OS information</td>
      <td><code>nmap -sC -sV -Pn -vv 172.20.10.4</code></td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/4870be9e-8507-4261-81fb-2bf56384eec2" /></td>
    </tr>
    <tr>
      <td>5</td>
      <td>Perform directory brute-forcing to discover hidden web directories and files on the target web server</td>
      <td><code>dirb http://192.168.115.129 ~/Desktop/wordlist.txt -X .php,.html,.txt</code></td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/07b8b6fe-0d50-4f27-9452-dbebe1401fc2" /></td>
    </tr>
    <tr>
      <td>6</td>
      <td>Access the main index page of the web server to check for any visible information or entry point</td>
      <td>Browse to <code>172.20.10.4/index.html</code> → "Welcome, Are You Ready To Play With Me"</td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/63054783-8ad9-456d-8067-396e4db5fd45" /></td>
    </tr>
    <tr>
      <td>7</td>
      <td>Access a discovered hidden directory called "hidden_text" to find concealed content or clues</td>
      <td>Browse to <code>172.20.10.4/hidden_text</code> → Found hidden content</td>
      <td>
        <img width="100%" src="https://github.com/user-attachments/assets/377f9d74-182f-4950-8183-97b975cd97b8" /><br />
        <img width="100%" src="https://github.com/user-attachments/assets/0b35368e-12a4-473e-8e53-14f4183691bf" />
      </td>
    </tr>
    <tr>
      <td>8</td>
      <td>View the HTML source code of the hidden_text page to uncover any comments, scripts, or embedded elements not visible in the browser</td>
      <td>View page source → Discovered a QR code</td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/067491c7-6f5b-45f4-a340-2be6425462da" /></td>
    </tr>
    <tr>
      <td>9</td>
      <td>Decode the QR code to extract hidden credentials or configuration data</td>
      <td>QR code contains FTP credentials script</td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/7924db35-2fda-4265-85ef-4d4bc1bdf81c" /></td>
    </tr>
    <tr>
      <td>10</td>
      <td>Use the extracted FTP credentials to log into the target's FTP service and explore available files</td>
      <td>Logged into FTP using credentials from QR code</td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/b4657b8b-b22d-46f5-a916-0f1a59c24d6d" /></td>
    </tr>
    <tr>
      <td>11</td>
      <td>Download and extract interesting files from the FTP server for further analysis</td>
      <td>Retrieved <code>information.txt</code> and <code>p_list.txt</code></td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/d95ff1a7-b66b-48c7-a805-0b7553607934" /></td>
    </tr>
    <tr>
      <td>12</td>
      <td>Read the contents of the extracted files to find potential passwords or sensitive information</td>
      <td><code>cat</code> → Found password list</td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/35c2994c-50a2-4e2e-9b2e-105c649bec47" /></td>
    </tr>
    <tr>
      <td>13</td>
      <td>Use Hydra to perform a password brute-force attack against the SSH service using the extracted password list</td>
      <td><code>hydra</code> targeting SSH login</td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/42c322b9-b111-47b3-af48-c2579fd1298e" /></td>
    </tr>
    <tr>
      <td>14</td>
      <td>Successfully log into the target machine via SSH using the discovered credentials and get the first flag</td>
      <td><code>ssh robin@172.20.10.4</code></td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/916cc28b-6f13-4c90-b0d0-8ad643694504" /></td>
    </tr>
    <tr>
      <td>15</td>
      <td>Check sudo privileges, list project directory, and read first flag</td>
      <td>
        <code>sudo -l</code> → (jerry) NOPASSWD:/home/robin/project/feedback.sh<br />
        <code>ls project</code> → <code>user1.txt</code>
      </td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/8ea7aec8-4635-44ac-987f-3fed3205aaad" /></td>
    </tr>
    <tr>
      <td>16</td>
      <td>Switch to the jerry user by exploiting the sudo permission on the feedback.sh script</td>
      <td><code>sudo -u jerry /home/robin/project/feedback.sh</code></td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/acb0f6c5-f800-45bd-a734-7399fd4328f1" /></td>
    </tr>
    <tr>
      <td>17</td>
      <td>Capture the second flag after gaining access to the jerry user account</td>
      <td>Second flag obtained</td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/d34879ed-2cce-4c55-8dcb-625959cc70d7" /></td>
    </tr>
    <tr>
      <td>18</td>
      <td>Upgrade the restricted shell to a fully interactive TTY for better command execution and stability</td>
      <td><code>python -c 'import pty; pty.spawn("/bin/bash")'</code></td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/d0873c4a-dded-4452-b1f6-2a138af85073" /></td>
    </tr>
    <tr>
      <td>19</td>
      <td>Search for SUID binaries to find privilege escalation vectors (files with setuid permission)</td>
      <td><code>find / -perm -u=s -type f 2>/dev/null</code></td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/2702bb6d-4fd6-42e4-a3af-772d925d0c9e" /></td>
    </tr>
    <tr>
      <td>20</td>
      <td>Use Docker to escape the container and mount the host filesystem, gaining root access, then navigate to the root directory and capture the final root flag</td>
      <td>
        <code>docker run -v /:/mnt --rm -it alpine chroot /mnt sh</code><br />
        <code>whoami</code> → <code>root</code><br />
        <code>cd /root</code> → <code>ls</code> → <code>root.txt</code><br />
        <code>cat root.txt</code> → <code>Fl4g{r00t-H4ckTh3P14n3t0nc34g41n}</code>
      </td>
      <td><img width="100%" src="https://github.com/user-attachments/assets/8a9412ae-dd6e-4c6a-b8b7-d4d2867fca6d" /></td>
    </tr>
  </tbody>
</table>
