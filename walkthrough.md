so we are given an ip-a per usual and first and foremost we are going to perform an nmap scan of ports

Tool used: Nmap

<img width="778" height="255" alt="image" src="https://github.com/user-attachments/assets/b81c93c6-0d17-4633-9090-f2d892f3d026" />



We found an open 80 port meaning we can access its web http browser shown below.Whereby under inspection of the webpage we find a username

<img width="1100" height="530" alt="image" src="https://github.com/user-attachments/assets/22f34493-6d46-4bd5-9f45-e3476da76dc5" />


Now we need to find the password,using gobuster we find that we have a hidden /robots.txt shown below

Tool used:Gobuster

<img width="890" height="565" alt="image" src="https://github.com/user-attachments/assets/5a98c562-a4b0-44b5-b005-0510d8100d16" />


Trying to access it from the web browser we meet with a base64 encoded password
<img width="519" height="122" alt="image" src="https://github.com/user-attachments/assets/0cbf130e-7350-4f55-80d4-79a4851b1476" />


And now using the famous cyberchef tool(https://gchq.github.io/CyberChef/#input=V1VCIEtERk4gUEg) we decoded the password in which we got

Tool used:Cyberchef

<img width="1100" height="458" alt="image" src="https://github.com/user-attachments/assets/dbb551b0-18d4-4b18-b679-18c7d2e69d90" />


Moving forward we login in the ssh openport using username@<ip-a> and when prompted to enter the password we enter the one we got from above cyberchef tool

<img width="552" height="364" alt="image" src="https://github.com/user-attachments/assets/fc3e9b52-06db-4f16-8088-21c2763aa49d" />

Then we successfully got our first flag from local.txt file.

Subscribe to the Medium newsletter
Moving forward to attaining our second flag i tried seeing the sudo privileges we have in the current user using sudo -l

<img width="466" height="199" alt="image" src="https://github.com/user-attachments/assets/0ae1b723-2dbf-4700-8f93-cb24bdb6a4ce" />

But it was unsucessful since we dont have the sudo password for the current user.So we tried using the comman uname -a to see the systems kernel and OS and we got

<img width="974" height="208" alt="image" src="https://github.com/user-attachments/assets/c8e8a88e-798d-4ccd-bcb8-93aeaa16679a" />


Now we went directly to Exploit-DB and searched for vulnerability found in Linux 3.13.0 version and we got it

<img width="1100" height="241" alt="image" src="https://github.com/user-attachments/assets/1ef475ae-164e-4e02-81f3-24224cabe5f1" />


Now reading into it we found that to get root privileges we have to first, download in our machine the exploitcode for above vulnerability which we can get from( https://www.exploit-db.com/download/37292 -O ofs-root.c)using tool wget shown below

<img width="1100" height="241" alt="image" src="https://github.com/user-attachments/assets/5dcf22cd-6fe8-4457-99e4-730de636c47a" />


Then we copy the file from our machine to the ssh shell /tmp directory and we will be prompted for the password again which we shall enter the one we got from above

<img width="646" height="112" alt="image" src="https://github.com/user-attachments/assets/e7592294-c96c-461d-ae93-ef32ccbbc6a8" />

Then back to our ssh shell we move to our /tmp directory using (cd /tmp) then use ls to see whether our ofs-root.c file is available.Then we run the command (gcc ofs-root.c -o ofs-root) which compiles the ofs-root.c source code (the overlayfs exploit) and produces an executable file named ofs-root.We then make it executable by runnig command (chmod +x ofs-root) then run the exploit file itself using (./ofs-root) in which we will see a # meaning we have successfully gained root privileges proven by running (whoami) command getting root as shown below
<img width="437" height="276" alt="image" src="https://github.com/user-attachments/assets/1828865e-687b-4fa8-b0d4-6cf11dbea0ab" />


Now running ls /root/ command we see we finally have the proof.txt file and on running cat /root/proof.txt we finally get our last flag

<img width="466" height="199" alt="image" src="https://github.com/user-attachments/assets/df74888c-bea8-4c29-b20b-1f1b270ef4ea" />

In this CyberSploit1‑offsec challenge, we started by enumerating the target with Nmap and discovered an open HTTP service. By inspecting the site and using Gobuster, we found a hidden robots.txt containing a Base64‑encoded password. We decoded it using CyberChef, used the credentials to SSH into the server, and captured the first flag (local.txt).
To obtain root privileges, we checked the kernel version (uname -a), identified it as a vulnerable version, and leveraged the OverlayFS exploit from Exploit‑DB. After compiling and executing the payload, we escalated to root and retrieved the final proof flag (proof.txt).

This challenge was an insightful exercise in enumeration, basic cryptography, and privilege escalation — reinforcing critical penetration testing skills.
