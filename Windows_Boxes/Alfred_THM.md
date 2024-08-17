Alfred is a THM box 

![image](https://github.com/user-attachments/assets/a13253ae-f2f2-4067-beab-119b4877f6a3)

**Reconnaissance**

Run map scan 

We get back the following result.

![image](https://github.com/user-attachments/assets/962f85c2-e284-4a72-a29c-8e9ab3bc8aea)


we have three open ports 

* port 80 Microsoft IIS httpd 7.5
* 3389/tcp open  ssl/ms-wbt-server
* 8080/tcp open  http Jetty  


![image](https://github.com/user-attachments/assets/f34d690d-d1b3-4409-a439-38055eb37d61)

**Enumeration**

Visit the Web application 

![image](https://github.com/user-attachments/assets/a3fa0f46-856d-4932-9cb3-045f0e76a5a0)

While enumerating port 8080, a login page powered by Jenkins was discovered. Different default login credentials were attempted, and a successful login was achieved with admin: admin.

After successfully logging in, navigation through various options was initiated. Upon reaching “Manage Jenkins,” various options were displayed, and when “Script Console” was selected, a location for executing Groovy scripts was found.

![image](https://github.com/user-attachments/assets/8ba03aae-04e1-4b29-9954-2d4e275d1ddb)

![image](https://github.com/user-attachments/assets/809f477d-1b64-437a-958d-73b1f98c6c08)

A Groovy script reverse shell script was searched for on Google and subsequently located. It was employed to obtain a reverse shell.

![image](https://github.com/user-attachments/assets/8f5f713e-5875-4e1c-bcad-413c050d6a08)

![image](https://github.com/user-attachments/assets/da967f3d-e12b-4c2f-b249-7d615a5202e7)


![image](https://github.com/user-attachments/assets/41355bdf-3451-4df2-874c-4e512942d595)


![image](https://github.com/user-attachments/assets/90e237ef-9b31-4c8e-b253-8cd13c3a7a60)

The Groovy reverse shell script was used to establish a reverse shell, and the LPORT was monitored on my attacker machine. A shell of the user “Bruce” was obtained.

![image](https://github.com/user-attachments/assets/c1b7c5c0-f552-4add-986d-617019f7fd9c)

![image](https://github.com/user-attachments/assets/ab06e2f7-39c9-45d9-b937-dba5fb412f12)

The user’s home directory in Windows is the user’s Desktop. Navigating to the Desktop of the user, I was able to find the user flag.

**Privielge escalation**
To make privilege escalation easy, a decision was made to switch to a Meterpreter shell. The process of creating a Meterpreter reverse shell was initiated in order to obtain a reverse shell.

![image](https://github.com/user-attachments/assets/05542ece-76a4-4190-98d9-4e1d5b78f7c5)

After the payload had been generated, the process of transferring the reverse shell from the attacker machine was initiated.

A Python server was created for file transfer from the location of the rev-shell.exe in the attacker machine.
![image](https://github.com/user-attachments/assets/92afa088-ddde-4ce5-837d-83fb451dcf72)

![image](https://github.com/user-attachments/assets/2997b86c-730f-4f53-8031-91162a28b500)

The tshell-name.exe was downloaded from the attacker's machine.

![image](https://github.com/user-attachments/assets/de9727e0-90fd-43b2-9e28-4f14e0595ec9)

The msfconsole was set up to listen for the reverse shell.

![image](https://github.com/user-attachments/assets/2b74841b-2799-4861-aec8-7998512e5d74)

![image](https://github.com/user-attachments/assets/3bd507ba-128d-4dd1-8ef8-bd8790c29e65)

The rev_shell.exe file was executed, resulting in the successful acquisition of a Meterpreter shell.

![image](https://github.com/user-attachments/assets/bf75f2fc-7136-4fa6-b504-82992a9a96c2)

Two privileges, SeDebugPrivilege and SeImpersonatePrivilege, can be observed as being enabled. The Incognito module will be utilized to exploit this vulnerability.

![image](https://github.com/user-attachments/assets/517f3437-e3a2-48a5-ab61-98e4c452ad75)

The “load incognito” command is used to load the Incognito module in Metasploit. It should be noted that the “use incognito” command may need to be employed if the previous command does not work. Additionally, it is important to ensure that your Metasploit is up to date.

![image](https://github.com/user-attachments/assets/22cb7b40-4256-4d55-81f8-494b53192639)

To check the available tokens, the “list_tokens -g” command is entered. It can be observed that the BUILTIN\Administrators token is available.

![image](https://github.com/user-attachments/assets/0967793b-4bac-4ced-bb8b-ab99262ddd3c)




The impersonate_token “BUILTIN\Administrators” command is utilized to impersonate the Administrators’ token. The output of the “getuid” command is sought after.

![image](https://github.com/user-attachments/assets/6878dad0-16c5-467a-b091-0480b91bfdf0)

![image](https://github.com/user-attachments/assets/83a73da8-d2f4-436d-b9ae-76923a419122)


Despite possessing a higher privileged token, the permissions of a privileged user may not be available (this is attributed to the way Windows manages permissions, relying on the process’s Primary Token rather than the impersonated token to establish its capabilities and limitations).

To address this, it is important to migrate to a process with the correct permissions, as indicated in the previous answer. The most secure choice is the services.exe process. Initially, employ the “ps” command to list processes and identify the PID of the services.exe process. Subsequently, migrate to this process using the “migrate PID-OF-PROCESS” command.

![image](https://github.com/user-attachments/assets/6483b2ec-297a-40cf-843e-d4be30ddf0d7)

The root.txt file was located at C:\Windows\System32\config.

