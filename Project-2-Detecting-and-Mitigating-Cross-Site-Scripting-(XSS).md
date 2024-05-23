# Project 2: Detecting and Mitigating Cross-Site Scripting (XSS) 

## Introduction
Cross-Site Scripting (XSS) is a common web security vulnerability that allows attackers to inject malicious scripts into web pages viewed by other users. These scripts can hijack user sessions, deface websites, or redirect users to malicious sites. In this project, you will learn how to identify and exploit XSS vulnerabilities using Burp Suite and bWAPP (Bee-box Web Application), and understand how to mitigate them.

## Pre-requisites
- Basic understanding of web application concepts and the HTTP protocol.
- Familiarity with HTML and JavaScript.
- A working installation of bWAPP and Burp Suite Community Edition.

## Lab Set-up

### Setting up bWAPP
1. **Download and Install XAMPP**:
   - Download XAMPP from [Apache Friends](https://www.apachefriends.org/index.html) and install it.
   - Start the Apache and MySQL services from the XAMPP control panel.

2. **Download bWAPP**:
   - Download bWAPP from [bWAPP's official website](http://www.itsecgames.com/).

3. **Install bWAPP**:
   - Extract the bWAPP files into the `htdocs` directory of your XAMPP installation (e.g., `C:\xampp\htdocs\bWAPP`).

4. **Configure bWAPP Database**:
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

### Exercise 1: Identifying Reflected XSS

1. **Set bWAPP Security Level to Low**:
   - Log in to bWAPP and go to the "Choose your bug" section.
   - Set the security level to "low" and select "Reflected XSS" from the list of bugs.
   - Click "Hack".

2. **Navigate to Reflected XSS Page**:
   - You will be presented with a form to input a message.

3. **Intercept the Request with Burp Suite**:
   - Enter a message (e.g., `Hello World!`) and click "Go!".
   - Burp Suite will intercept the request. Forward the request to see the result.

4. **Modify the Request to Exploit XSS**:
   - Intercept the request again and modify the `message` parameter to an XSS payload (e.g., `<script>alert('XSS');</script>`).
   - Forward the modified request and observe the response.

### Commands and Expected Output

1. **Initial Request Interception**:
   - Request intercepted by Burp Suite:
     ```
     GET /bWAPP/xss_r.php?message=Hello+World%21&action=Go%21 HTTP/1.1
     Host: localhost
     ```

2. **Modified Request with XSS**:
   - Modify the intercepted request:
     ```
     GET /bWAPP/xss_r.php?message=<script>alert('XSS');</script>&action=Go%21 HTTP/1.1
     Host: localhost
     ```
   - Expected output: The response should execute the alert script, displaying a pop-up with the message `XSS`.

### Exercise 2: Identifying Stored XSS

1. **Set bWAPP Security Level to Low**:
   - In bWAPP, select "Stored XSS" from the list of bugs.
   - Click "Hack".

2. **Navigate to Stored XSS Page**:
   - You will be presented with a form to post a message.

3. **Intercept the Request with Burp Suite**:
   - Enter a message (e.g., `Hello again!`) and click "Sign Guestbook".
   - Burp Suite will intercept the request. Forward the request to see the result.

4. **Modify the Request to Exploit Stored XSS**:
   - Intercept the request again and modify the `message` parameter to a stored XSS payload (e.g., `<script>alert('Stored XSS');</script>`).
   - Forward the modified request and observe the response.

### Commands and Expected Output

1. **Initial Request Interception**:
   - Request intercepted by Burp Suite:
     ```
     POST /bWAPP/xss_s.php HTTP/1.1
     Host: localhost
     Content-Type: application/x-www-form-urlencoded
     Content-Length: 38

     message=Hello+again%21&btnSign=Sign+Guestbook
     ```

2. **Modified Request with XSS**:
   - Modify the intercepted request:
     ```
     POST /bWAPP/xss_s.php HTTP/1.1
     Host: localhost
     Content-Type: application/x-www-form-urlencoded
     Content-Length: 63

     message=<script>alert('Stored XSS');</script>&btnSign=Sign+Guestbook
     ```
   - Expected output: The stored XSS payload will be executed whenever the guestbook is viewed, displaying an alert pop-up with the message `Stored XSS`.

### Exercise 3: Mitigating XSS

1. **Implementing Input Validation and Output Encoding**:
   - Modify the application code (if you have access) to properly sanitize user inputs and encode outputs.

2. **Testing the Mitigation**:
   - After applying the mitigation, repeat the XSS attempts and ensure they are no longer successful.

## Conclusion
By completing these exercises, you will gain hands-on experience in identifying and exploiting XSS vulnerabilities using Burp Suite and bWAPP. Additionally, you will understand the importance of proper input validation and output encoding in preventing XSS attacks.
