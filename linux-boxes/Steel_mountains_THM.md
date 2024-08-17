Steel Mountain is a THM machine 

![image](https://github.com/user-attachments/assets/c9a4dba0-4117-4d65-9e7f-3f8dd916f518)

First thing first, we run a quick initial nmap scan to see which ports are open and which services are running on those ports.

nmap -sC -sV 10.10.224.110 -oA initial  

-sC: run default nmap scripts
-sV: detect service version
-O: detect OS [not allowed to run on this machine- I got an error]
-oA: output all formats and store in file initial



