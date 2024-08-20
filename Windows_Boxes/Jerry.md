
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
            echo "Hello, World!"
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

