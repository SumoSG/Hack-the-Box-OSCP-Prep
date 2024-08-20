
![image](https://github.com/user-attachments/assets/09cf167b-ac16-45f5-9afc-5a66232cba1d)

**Reconnaissance**

I run four quad nmap scans 
*nmap -sS -v $IP --top-ports 1000
**rustscan -a $IP --ulimit 5000
***nmap -sU -v --top-ports 5 10.10.10.95
nmap -p- --min-rate 10000 -T4 10.10.10.95

![image](https://github.com/user-attachments/assets/5a46ac1f-83d8-45c7-967a-9d3ea37adf4a)

<div style="position: relative; margin-bottom: 1em;">
    <pre style="background-color: #f6f8fa; padding: 1em; border-radius: 5px;">
        <code id="codeSnippet">
          nmap -sS -v $IP --top-ports 1000 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-20 21:00 +08
Initiating Ping Scan at 21:00
Scanning 10.10.10.95 [4 ports]
Completed Ping Scan at 21:00, 0.06s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 21:00
Completed Parallel DNS resolution of 1 host. at 21:00, 0.01s elapsed
Initiating SYN Stealth Scan at 21:00
Scanning 10.10.10.95 [1000 ports]
Discovered open port 8080/tcp on 10.10.10.95                                                                        
Completed SYN Stealth Scan at 21:00, 6.03s elapsed (1000 total ports)                                               
Nmap scan report for 10.10.10.95                                                                                    
Host is up (0.045s latency).                                                                                        
Not shown: 999 filtered tcp ports (no-response)                                                                     
PORT     STATE SERVICE                                                                                              
8080/tcp open  http-proxy                                                                                           
                                                             
        </code>
    </pre>
    <button onclick="copyToClipboard()" style="position: absolute; top: 10px; right: 10px; padding: 0.5em 1em; border: none; border-radius: 5px; background-color: #28a745; color: white; cursor: pointer;">
        Copy
    </button>
</div>

<script>
    function copyToClipboard() {
        var code = document.getElementById("codeSnippet").innerText;
        var textarea = document.createElement("textarea");
        textarea.value = code;
        document.body.appendChild(textarea);
        textarea.select();
        document.execCommand("copy");
        document.body.removeChild(textarea);
        alert("Copied to clipboard!");
    }
</script>

