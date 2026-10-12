# Web security — Interview

Answers are filled for the published interview set.

[← Back to hub](./7_special.md)

### Related docs

- [Web security](./7_special_security.md) · [Web security — Interview](./7_special_security_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/).

### Web Security Fundamentals

<details>
<summary>What is web security, and why is it important?</summary>

### What is web security, and why is it important?

**Definition:**

**Web Security** encompasses strategies and technologies aimed at protecting internet-connected systems, including web applications and services from various threats. It's a paramount consideration for businesses to safeguard data and maintain user trust.

**How it works:**

Fundamental Security Principles

- **Confidentiality**: Ensuring that sensitive information is accessible only to authorized entities.
- **Integrity**: Preserving the accuracy and trustworthiness of data.
- **Availability**: Making resources and services accessible when needed.

Web Security Components

Transport Layer Security (TLS)

**TLS** serves as the foundation for secure internet communication, ensuring encryption and data integrity through mechanisms like symmetric and asymmetric encryption.

Access Control

- **Authentication**: Verifies the identity of users through credentials or multi-factor methods.
- **Authorization**: Governs user access to resources and services based on their permissions.

Security Headers

**HTTP Security Headers** are HTTP response headers designed to enhance web application security. They provide strict web-security policies, protect against specific attacks, and help detect and mitigate potential security vulnerabilities.

- **X-Content-Type-Options**: Prevents content type sniffing.
- **X-Frame-Options**: Protects against clickjacking.
- **Content-Security-Policy**:  Mitigates cross-site scripting attacks and other code injection attacks.
- **X-XSS-Protection**: Activates the Cross-site scripting (XSS) filter in web browsers.

Data Validity and Sanitation

Properly validating and sanitizing input data from users is crucial in preventing injection and manipulation attacks.

- **Cross-Site Scripting (XSS)**: Attacks involving the execution of malicious scripts in a user's browser.
- **SQL Injection**: Exploits database handling code to execute unauthorized SQL commands.

Anti-CSRF Tokens

**Cross-Site Request Forgery (CSRF)** tokens mitigate unauthorized requests sent by trusted authenticated users.

Session Management

For maintaining user sessions securely, it's essential to consider session token generation, expiration, and storage best practices.

- **Secure Cookie Flags**: Additional flags like "Secure" and "HttpOnly" help protect against certain types of attacks like session hijacking and cross-site scripting.
- **Session Regeneration**: Regularly changing session tokens minimizes the window of opportunity for attackers.

**Examples:**

Code Example: Setting HTTP Security Headers

Here is the Python code:

```python
from flask import Flask

app = Flask(__name__)

# Example: Setting Content-Security-Policy Header
@app.after_request
def add_security_headers(response):
    response.headers.add('Content-Security-Policy', 
                          "default-src 'self'; script-src 'self' 'unsafe-inline';")
    return response

if __name__ == '__main__':
    app.run()
```

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>Can you explain what HTTPS is and how it differs from HTTP?</summary>

### Can you explain what HTTPS is and how it differs from HTTP?

**Definition:**

**HTTPS** (HyperText Transfer Protocol Secure) is an extension of HTTP that integrates security protocols for more secure data communication. It's essential for **maintaining privacy** and protecting sensitive data during online activities such as financial transactions, form submissions, and user logins.

**How it works:**

Key Features

- **Data Encryption**: Uses cryptographic protocols like SSL/TLS to encode messages.
- **Certificate Validation**: Authenticates servers and, in some cases, clients.
- **Data Integrity**: Employs hash functions to confirm that the message content has not been tampered with.

How HTTP and HTTPS Differ

- **Security Mechanisms**: While HTTP utilizes plaintext for data transfer, HTTPS adds security protocols like SSL/TLS.
- **Port Number**: HTTP typically uses port 80, while HTTPS uses port 443.
- **Transfer Speed**: HTTPS might be slightly slower due to encryption.
- **Default Browser Behavior**: Modern browsers can mark HTTP websites as "Not Secure."
- **SEO & Search Engine Strategy**: Secure websites rank higher.

The SSL/TLS Handshake

When a `Client` connects to a `Server`:

1. **ClientHello**: The `Client` initiates the communication, stating its requirements and capabilities.
2. **ServerHello**: The `Server` selects the best cipher suite and informs the `Client`. It also provides its public key if using asymmetric encryption.
3. **Certificate**: The `Server` sends its SSL certificate along with additional certificates, including the Certificate Authority's (CA) public key for validation.
4. **Key Exchange**: The `Client` and `Server` compute a shared session key, necessary for symmetric encryption.
5. **Finished**: Both parties send a hash-based verification message, ensuring secure channel establishment.
6. **Secure Data Transmission**: The shared secret key is used for symmetric encryption.

Common Security Terminologies in HTTP(S)

- **SSL/TLS**: The protocols responsible for secure connections.

- **Certificate Authority (CA)**: A trusted entity that issues SSL certificates.

- **Cipher Suite**: A collection of data encryption, authentication, and key exchange mechanisms used for the secure connection.

- **Handshake Protocols**: The client and server agree on a suitable security configuration during handshake.

- **Public Key Infrastructure (PKI)**: The framework that controls the use and distribution of digital certificates.

**Examples:**

Code Example: Basic HTTP Server

Here is the Python code:

```python
from http.server import HTTPServer, BaseHTTPRequestHandler

class SimpleHTTPRequestHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.end_headers()
        self.wfile.write(b'Hello, world!')

httpd = HTTPServer(('localhost', 8000), SimpleHTTPRequestHandler)
httpd.serve_forever()
```
Use Command `openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365` to create self-signed SSL certificate.

Code Example: Basic HTTPS Server

Here is the Python code:

```python
from http.server import HTTPServer, BaseHTTPRequestHandler
import ssl

class SimpleHTTPRequestHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.end_headers()
        self.wfile.write(b'Hello, secure world!')

httpd = HTTPServer(('localhost', 4443), SimpleHTTPRequestHandler)
httpd.socket = ssl.wrap_socket(httpd.socket, certfile='cert.pem', keyfile='key.pem', server_side=True)
httpd.serve_forever()
```

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>What are SSL and TLS, and what role do they play in web security?</summary>

### What are SSL and TLS, and what role do they play in web security?

**Definition:**

**SSL** (Secure Sockets Layer) and **TLS** (Transport Layer Security) are cryptographic protocols that create a secure, encrypted connection between a client and a server. This ensures that the data being transmitted across a network is secure and can't be easily intercepted or tampered with. This is especially important for sensitive data such as login credentials, personal information, and financial transactions.

**How it works:**

Key Features

- **Authentication**: Certificates are used to verify the identity of the server, ensuring that the client is communicating with the intended server and not an impersonator.

- **Confidentiality**: Data transferred between the client and the server is encrypted, obscuring its contents from unauthorized parties.

- **Integrity**: By using digital signatures, SSL/TLS ensures that data remains unchanged during transmission. If any tampering is detected, the connection is terminated.

- **Forward Secrecy**: Every SSL/TLS session uses a fresh, unique encryption key, so even if one key is compromised, it won't affect previous or future communication.

- **Compatibility**: SSL and TLS are designed to be backward and forward compatible, allowing older and newer systems to communicate securely.

- **Protection against known vulnerabilities**: Modern versions of TLS incorporate the latest security practices and can mitigate risks like session hijacking, man-in-the-middle attacks, and more.

Encryption Mechanisms

SSL/TLS utilizes different encryption algorithms. In the handshake process, they are used for key exchange and to establish a secure communication channel, categorically:

- **Symmetric Key Encryption**: A single key is shared between the client and the server and is used for both encryption and decryption. Common algorithms include AES and RC4.

- **Asymmetric Key Encryption**: Also called public-key cryptography, it uses a key pair - one public, one private. Data encrypted with one can only be decrypted with the other. This type is generally used for the secure exchange of symmetric keys and for initial authentication. Common algorithms include RSA, Diffie-Hellman, and Elliptic Curve Cryptography (ECC).

- **Hash Functions**: Ensure data integrity and are used to create a message digest or checksum. Common algorithms include SHA (Secure Hash Algorithm) variations like SHA-256.

SSL/TLS Handshake

Before data exchange begins, the client and server establish a secure connection through a **handshake**, which includes the following main steps:

1. **Hello Messages**: The client and server exchange messages to initiate the handshake.

2. **Certificate Exchange**: The server presents its SSL/TLS certificate, which contains its public key and is signed by a recognized Certificate Authority (CA).

3. **Key Exchange**: If needed, a secure key for the session is exchanged using asymmetric encryption.

4. **Symmetric Key Session Creation**: A unique session key is generated (such as the session key for the symmetric key cipher) and is securely shared between the client and server.

5. **Establish Cipher Suite**: A set of encryption, hash, and handshake algorithms to be used throughout the session is agreed upon.

6. **Handshake Completion**: Both parties confirm the completion of the handshake process.

Evolution: SSL to TLS

While SSL was foundational for the development of secure web communication, it has been largely **deprecated** due to well-documented security vulnerabilities.
This led to the creation of its successor, TLS, which has undergone several versions, each improving upon its predecessor in terms of security and functionality.

SSL and TLS in the Browser

To check a website's SSL/TLS status in Google Chrome, for instance, you could click on the padlock icon and view the certificate. A more holistic picture of a website's security can be viewed by clicking on the 'Not secure' or the padlock icon. This shows additional security details, such as whether the connection is secure or not and if there are any security issues.

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>How do SSL certificates work, and what is the purpose of a Certificate Authority (CA)?</summary>

### How do SSL certificates work, and what is the purpose of a Certificate Authority (CA)?

**Definition:**

Let's walk through the purpose and mechanisms of **SSL certificates** and the role of **Certificate Authorities**.

**How it works:**

Secure Sockets Layer & Transport Layer Security

**SSL/TLS** protocols provide a secure channel between two communicating machines, ensuring privacy, integrity, and authentication.

These are the main components:

1. **Digital handshake**: Establishes the session, negotiates encryption algorithm, and generates session keys.
2. **Key Exchange**: Public-key cryptography to share a secret key for further symmetric encryption, ensuring speed and security.
3. **Encryption**: The shared secret key is used for symmetric encryption of data.
4. **Verification**: Certificates provide the means for authenticating parties.

Certificate Authority (CA)

A **Certificate Authority** is a trusted entity, responsible for validating entities' identities before issuing certificates vouching for their authenticity.

The primary functions of a CA are:

1. **Authentication**: Verifying the identity of entities requesting a certificate.
2. **Issuance**: Creating and digitally signing certificates for validated entities.
3. **Revocation**: Providing mechanisms to invalidate certificates in case they're compromised.

Certificate Components

A standard SSL certificate comprises the following sections:

1. **Issuer Information**: Details of the authority issuing the certificate.
2. **Validity Period**: Start and end dates for when the certificate is considered valid.
3. **Subject Information**: Identifying information for the entity the certificate is issued to, such as the domain name.
4. **Public Key**: The vital piece of encryption enabling secure communication.
5. **Signature**: A hash of the certificate data signed with the private key of the issuing authority.

Certificate Formats

SSL certificates primarily exist in two formats:

1. **Base64-encoded** \( \rightarrow \) Useful for communication between systems or for human readability.
2. **DER (Distinguished Encoding Rules)** \( \rightarrow \) A binary format with a more concise footprint, suited for machine use.

Obtaining SSL Certificates

SSL certificates can be obtained in multiple ways. The primary methods are:

1. **Self-signed Certificates**: Suitable for individual or local testing, where a CA's involvement might not be necessary. Not typically used in production due to trust issues.
2. **Publicly-trusted CA Certificates**: Issued by established CAs and widely accepted by browsers and systems. These require thorough identity verification ensuring the certificate owner is legitimate.

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>What is the difference between encryption and hashing?</summary>

### What is the difference between encryption and hashing?

**Definition:**

Both **encryption** and **hashing** are cryptographic methods to enhance data security. However, they serve different purposes, and these roles come with distinct functionalities and characteristics.

**How it works:**

Core Differences and Use Cases

- **Encryption** secures data in a reversible manner using keys, making it understandable only with the right key. This is useful to keep data confidential.

- **Hashing**, on the other hand, is one-way—it converts input data into a unique, fixed-length string, called a **hash value**, which represents the original data. This is useful to verify data integrity and authenticate identities, especially for passwords where the original data should never be needed or known.

One-Way Process (Hashing)

Data goes through a one-way transformation process:

- **Input**: Original data like a message or password.
- **Hash Function**: A mathematical algorithm that processes the input into a fixed-length output known as the hash value.

Use Case: Password Management

Instead of storing plain-text passwords, it's safer to store their hashed equivalents. When a user submits a password for comparison, the system hashes the input and checks if the generated hash matches the stored one. This strengthens data security, ensuring that even if the stored has value is compromised, the original password remains protected.

Two-Way Process (Encryption)

- **Input**: Data
- **Encryption Algorithm**: Transforms the input into ciphertext.
- **Key**: Essential for encryption. Without the right key, it's nearly impossible to decrypt the ciphertext back into its original form.

**Examples:**

Computing the Encrypted Value

Here is the Python code:

```python
from cryptography.fernet import Fernet

# Generate a key to be used for encryption/decryption
key = Fernet.generate_key()

# Initialize the Fernet cipher using the generated key
cipher_suite = Fernet(key)

# Text to encrypt
text = b"Hello, this is a secret message!"

# Encrypt the text using the cipher
cipher_text = cipher_suite.encrypt(text)

print(f"Cipher Text: {cipher_text}")

# Decrypt the text back to its original form
original_text = cipher_suite.decrypt(cipher_text)
print(f"Original Text: {original_text.decode()}")
```

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>Define the concept of a secure session and explain how it is established.</summary>

### Define the concept of a secure session and explain how it is established.

**Definition:**

A **secure session** refers to the safeguarded interactive exchange between a web server and user-through a consistent, encrypted connection. This mutual trust is established via **session key management**, subsequently encrypting and decrypting all data transmissions.

**How it works:**

Session Key Management

The **session key** is the cryptographic linchpin of secure sessions. It's a relatively short-lived symmetric key used for encrypting the data during the session. Both parties—the web server and the user's browser—obtain the session key to sustain the confidential data exchange.

      - The web server generates the session key and maintains sole ownership.

      - The session key is sent to the user's browser using asymmetric encryption (public-key cryptography). The user's public key is employed to encrypt the session key.

      - Subsequent communications during the session are enveloped with the session key.

Key Generation

- **Randomness**: The session key's security hinges on its unpredictability. Suitable cryptographic algorithms and high-entropy sources, such as hardware RNGs or truly random user-generated input, are essential for the key's randomness. Consistent hashing can provide a more streamlined key distribution.

  - **Key Size**: Longer key lengths bolster the session's security. 128-bit keys, for example, are generally deemed secure against common attacks.

Session Key Establishment:

A web server and a user together ensure that they each **have the session key**. This method's objective is for both parties to possess the key without compromising it at any time.

Diffie-Hellman Key Exchange

The Diffie-Hellman algorithm enables two parties to formulate a shared secret without directly transmitting cryptographic keys. Each party devises a private key and an auxiliary public key, and following a series of exchanges, they both compute the shared secret. This secret then functions as the session key.

```plaintext
  Here is the Key Exchange
  
  User:
  Private Key: a
  Public Key: g^a mod p
  
  Server:
  Private Key: b
  Public Key: g^b mod p
  Shared Secret: (g^a)^b mod p = g^(ab) mod p
```

Perfect Forward Secrecy (PFS)

Perfect Forward Secrecy ensures that mechanical developments or bargained session keys at a later stage won't compromise the entire session or any earlier interchanges. Instead, it uses a freshly minted session key for each new session.

PFS is particularly beneficial for long-term security, making certain that past sessions won't be breached, even if the current session becomes compromised.

Additional Validation

To further bolster the security and confidence in the session establishment process, a series of mechanisms are often added:

  - **Certificates**: Verify the web server's identity, as well as establishing trust.

  - **Token-Based Systems**: Convert temporary tokens into sessions to keep user data secure even further.

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>What are some common web security vulnerabilities?</summary>

### What are some common web security vulnerabilities?

**Definition:**

Understanding common web security vulnerabilities is crucial in developing robust and secure web applications. Common security vulnerabilities include:

**How it works:**

Injection

**Description**: Unsanitized user input gets interpreted as commands by the backend database, opening the door to attackers for executing arbitrary SQL commands.

**Example**: A user enters `1; DROP TABLE users--` leading the database to execute unintended commands.

Mitigation

- **Avoid Dynamic Queries**: Prefer parametrized queries or ORM tools.
- **Input Sanitization**: Use prepared statements, stored procedures, or input validation.

Cross-Site Scripting (XSS)

**Description**: Vulnerability where web applications accept and display user-provided content (typically in HTML or JavaScript form) without escaping/sanitizing. This allows attackers to inject scripts, potentially compromising end-user systems or stealing data.

**Example**: Attacker inputs `<script>malicious_code()</script>` in a comment field. When a user views the comment, the script gets executed.

Mitigation

- **Sanitize User-Input**: Encode/escape user-provided content before rendering.
- **HTTP Headers & CSP**: Set proper `Content-Security-Policy` headers to restrict resource load from external sources.

Broken Authentication

**Description**: Weaknesses in the mechanisms used to authenticate the application's users, often arising from poor password management, insecure storage, or unprotected authentication tokens.

**Example**: Default or weak admin credentials, insufficient session management, or **Cross-Site Request Forgery** (CSRF) tokens being predictable or repetitive.

Mitigation

- **Strong Password Policies**: Enforce complexity and suggest periodic changes.
- **Secure Session Handling**: Use cookies with `HTTPOnly` and `Secure` attributes and non-predictable session IDs.

Security Misconfigurations

**Description**: Inaccurate or incomplete server, application, or database configurations that may give attackers unauthorized access or provide sensitive data.

**Example**: Default settings for admin interfaces, verbose error messages with stack traces in production, or storage buckets set to public instead of private.

Mitigation

- **Review and Monitor Settings**: Regularly audit configurations.
- **Principle of Least Privilege**: Restrict access to resources based on necessity.

Insecure Direct Object References (IDOR)

**Description**: When an application exposes internal implementation objects to users, potentially allowing malicious users to manipulate or access unauthorized data.

**Example**: An attacker changes a URL parameter meant to display their profile to access someone else's profile.

Mitigation

- **Use Indirect References**: Employ mappings or unique identifiers not directly linked to internal references.
- **Access Control Lists (ACLs)**: Implementing fine-grained access control mechanisms.

Cross-Site Request Forgery (CSRF)

**Description**: Occurs when an attacker tricks a victim into performing actions on a website where the victim is authenticated. This could be anything from clicking on a URL to a website's form submission, via another website, without the victim's consent.

**Example**: An attacker embeds a hidden form in a malicious website that submits a request to a banking site that the victim is authenticated to.

Mitigation

- **Use Anti-CSRF Tokens**: Use tokens unique to authenticated users, or  employ CORS (Cross-Origin Resource Sharing) headers.

Unvalidated Redirects and Forwards

**Description**: Websites featuring links or buttons that redirect users can be manipulated by attackers to forward users to malicious sites through **URL Redirection** or **Open Redirect** vulnerabilities.

**Example**: A website has a feature for login-free access to a partner site, and an attacker constructs a link to the affected site disguised as a link to the intended site.

Mitigation

- **Whitelisting or Direct Mapping**: Validate user input and check links against a whitelist of known URLs or enforce direct mappings.

**Examples:**

Code Example: SQL Injection Vulnerable Code

Here is the Python code:

```python
username = request.POST['username']
password = request.POST['password']

# Bad practice: Using string formatting
cursor.execute("SELECT * FROM users WHERE user='%s' AND password='%s'" % (username, password))
```

Code Example: XSS-Prone Code

Here is the HTML code:

```html
<form action="/comments" method="POST">
    <textarea name="comment" placeholder="Enter your comment"></textarea>
    <button type="submit">Post Comment</button>
</form>
```

Code Example: Vulnerable CSRF Code

Here is the HTML code:

```html
<form action="/make-transfer" method="POST">
    <input type="text" name="amount" />
    <input type="text" name="toAccount" />
    <button type="submit">Transfer</button>
</form>
```

Code Example: Redirect Prone to Attack

Here is the Python code:

```python
redirect_url = request.GET['redirect']
# Bad practice: Directly redirecting based on user input
return redirect(redirect_url)
```

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>Can you explain the Cross-Site Scripting (XSS) attack and how to prevent it?</summary>

### Can you explain the Cross-Site Scripting (XSS) attack and how to prevent it?

**Definition:**

In the context of web applications, **Cross-Site Scripting** (XSS) refers to the injection of malicious scripts into otherwise benign and trusted websites or web applications.

**How it works:**

What an XSS Attack Does

By executing **arbitrary code** within a victim's browser, an XSS attack can:

- Steal Sensitive Information: Obtain cookies, session tokens, or even keystrokes.
- Deface Websites
- Redirect Users: Forwards them to malicious sites.
- Spread Worms: Perpetuates the attack by infecting other users.

Types of XSS Attacks

1. **Stored XSS**: Scripts are stored in a database or other data source and then displayed to users on web pages.
2. **Reflected XSS**: The injected script is reflected off a web server, such as in an error message, and then executed within the user's browser.
3. **DOM-based XSS**: The attack occurs solely within the client-side HTML, bypassing typical server-based security measures.

Key Sources of Vulnerability

1. **User Input**: All user inputs, including form fields and query parameters in URLs, are potential XSS sources.
2. **Client-Side Data**: Data from GET requests, the document URL, and anchor elements (`<a>`) can also be vectors for XSS.

Key Defensive Measures

- **Output Encoding**: Convert special characters into their corresponding HTML entities, so they are displayed as text rather than executed.
- **HTTP Headers**: Utilize Content Security Policy (CSP) and the `X-XSS-Protection` header to bolster defense.
- **Input Validation**: Use strict input criteria and sanitize the input to prevent any script injection.
- **Session Cookies**: Implement the `HttpOnly` and `Secure` attributes to secure cookies.

Mitigation and Strong Input Validation

Key Anti-XSS PHP Functions:

- **htmlspecialchars()**: Converts special characters to HTML entities. The second parameter is essential to utilize, as it specifies the encoding mechanism.

  Example:

  ```php
  $output = htmlspecialchars($_GET['input'], ENT_QUOTES, 'UTF-8');
  ```

- **filter_var()**: particularly with `FILTER_SANITIZE_STRING`.

  Here is the PHP code:

  ```php
  $clean_input = filter_var($_GET['input'], FILTER_SANITIZE_STRING);
  ```

  However, be mindful that `FILTER_SANITIZE_STRING` might remove characters Unicode characters that are not within the 00-7F range.

- **ctype**: Functions such as `ctype_alpha()` can help verify if only alphabetic characters are present in a string.

  Example:

  ```php
  if (ctype_alpha($_GET['username'])) {
      // Username contains only alphabetic characters
  }
  ```

Best Practices for Prevention

- **Code Review**: Regularly scrutinize code for potential vulnerabilities.
- **Security Modules**: Consider integrating third-party security modules.
- **User Education**: Ensure users are aware of the risks and assist them in recognizing secure communication practices.
- **Sandboxed Environments**: Test or execute untrusted code in secure, isolated environments.

**Examples:**

Vulnerable Code Example

Here is the PHP code:

```php
echo $_GET['greeting'];
```

A potential attack URL:

```url
example.com?greeting=<script>alert('Hello, world!')</script>
```

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>What is SQL Injection and how can you defend against it?</summary>

### What is SQL Injection and how can you defend against it?

**Definition:**

**SQL Injection** is one of the most dangerous and prevalent attack vectors. It involves maliciously altering SQL statements through web applications. This tactic can lead to data theft, data manipulation, and even unauthorized system access.

**How it works:**

Common Methods for SQL Injection Attackers

- **Identification of Vulnerable Input Points**: This often involves forms or URL parameters.
- **Injection Through User Input**: Attackers provide manipulated input, such as altering SQL queries with additional conditions or as commands.
- **Data Exfiltration or Manipulation**: The goal could be anything from obtaining sensitive data to tampering with the system.

Guarding Against SQL Injection

- **Parameterized Statements**: Use placeholder parameters in SQL statements and pass the user-provided values separately.
- **Stored Procedures**: Centralize SQL logic, and call specific procedure names instead of constructing ad hoc SQL queries.
- **ORMs and Data Access Libraries**: These tools often provide a way to interact with the database without directly writing SQL.
- **Input Sanitization**: Validate and sanitize user input to mitigate injection risks.

**Examples:**

Code Example: Parameterized Query in Python

Here is the Python code:

  ```python
import sqlite3

conn = sqlite3.connect('example.db')
cursor = conn.cursor()

# Unsafe, vulnerable to SQL injection
query = "SELECT * FROM users WHERE username='%s' AND password='%s'" % (username, password)
cursor.execute(query)

# Safe, using parameterized query
query = "SELECT * FROM users WHERE username=? AND password=?"
cursor.execute(query, (username, password))

# Close
conn.close()
  ```

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>Describe what Cross-Site Request Forgery (CSRF) is and how to prevent it.</summary>

### Describe what Cross-Site Request Forgery (CSRF) is and how to prevent it.

**Definition:**

**Cross-Site Request Forgery (CSRF)**, often called session riding or one-click attack, tricks users into executing unwanted actions while authenticated on another site.

**How it works:**

Mechanism

- A **victim** accessing a malicious site remains authenticated on a trusted site in a separate tab or browser session.

- The **attacker** loads an image, an iframe, or a script tag with a request to the trusted site, which, due to the victim's active session, gets executed without the user's consent.

This breach occurs because web applications **fail to validate** the source of incoming requests.

Prevention Strategies

1. **Same-Origin Policy**: Modern browsers restrict how a web page can interact with resources from other domains, mitigating many CSRF scenarios. While generally effective, this approach has some limitations.

2. **Synchronizer Token Pattern**: The web application embeds a unique, random token in each HTML form and/or HTTP request. Upon submission, the server checks for this token's presence and its consistency with the user's session. This technique, known as a **CSRF token**, provides more robust defense.

3. **Double-Sided Cookies**: In addition to the user's session cookie, the web server generates a second cookie, often accessible only via HTTP headers. This additional layer of security becomes invaluable if the primary session cookie is compromised.

4. **User Confirmation**: Triggering critical actions only through a direct user interaction, such as a button click and not an automated request, deters common CSRF exploits.

5. **Always Use HTTPS**: HTTPS incorporates encryption and digital certificates to provide a secure channel for data communication, strengthening the defense against CSRF and numerous other attack vectors.

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>Explain the Same-Origin Policy and its importance in web security.</summary>

### Explain the Same-Origin Policy and its importance in web security.

**Definition:**

The **Same-Origin Policy** (SOP) is a foundational web security concept that guards against **Cross-Origin Attacks** and data exposure, ensuring a safer and more private browsing experience.

**How it works:**

Core Principle

**SOP** mandates that a web browser should restrict interactions between documents from different origins to prevent potential security breaches.

Identifying 'Origin'

An **origin** refers to the combination of :

- **Protocol**
- **Domain**
- **Port**

For two webpages to share a common origin and thereby avoid SOP restrictions, all these components must match.

Management of Cross-Origin Interactions

Browsers enable controlled cross-origin access using mechanisms such as:

- **Cross-Origin Resource Sharing (CORS)**: Servers can specify web resources that are accessible to various origins, relaxing the SOP's strictures.

- **Iframes**: These are windows embedded within a web page, useful for integrating third-party content, such as social media feeds or ads. Modern iframes are configurable, enabling or restricting cross-origin behavior.

Importance for Web Security

The SOP is a **cornerstone of web security** because it:

- **Safeguards User Data**: SOP prevents malicious websites from arbitrarily accessing sensitive user information present on legitimate websites in the same browser session.

- **Mitigates Cross-Site Scripting (XSS)**: By limiting scripts and other dynamic content to their origin, SOP thwarts numerous XSS attack vectors.

- **Curbs Cross-Site Request Forgery (CSRF)**: SOP helps in eliminating unauthorized, cross-site requests that can manipulate a user's session on a different origin.

- **Averts Cookie Theft**: Data like user session tokens stored in cookies can't be pilfered across origins, bolstering user authentication.

- **Sustains Web Application Confidentiality**: Supports the confidentiality of web applications and assists in the prevention of information leakage.

SOP Limitations

While SOP is a powerful and fundamental security tool, it has some limitations, such as:

- **Potential Misperception**: A website with numerous subdomains might confuse users on what exactly constitutes the 'same origin,' leading to accidental data exposure.
- **Loopholes in Older Browsers**: SOP's efficacy can vary across browsers. Earlier versions might not enforce it uniformly, enabling potential security breaches.
- **Incompatibility with Decentralized Systems**: Certain dispersed systems, like those using blockchain and decentralized file sharing, struggle with SOP's constraints, impeding their seamless operation.

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>What is Clickjacking, and what measures can prevent it?</summary>

### What is Clickjacking, and what measures can prevent it?

**Definition:**

**Clickjacking**, also known as a UI redress attack, is a form of web security threat in which a user is **tricked into clicking on something different from what they perceive, often leading to unintended actions**. This issue usually arises from cross-site scripting (XSS) vulnerabilities on a website.

**How it works:**

Common Clickjacking Techniques

- **Iframe Overlays**: Attackers place a transparent, positioned iframe on top of an innocuous element to intercept clicks.
- **CSS-Based Overlays**: By manipulating z-index and div positioning, attackers can make legitimate buttons appear beneath seemingly innocuous elements.
- **Drag & Drop**: Simulates user actions to trick users into dragging sensitive information onto a malicious target.

Defense Mechanisms Against Clickjacking

X-Frame-Options

The **`X-Frame-Options`** HTTP response header mitigates clickjacking by controlling frame embedding. It has three possible settings:

1. **DENY**: Disallows any form of framing, ensuring sites load in stand-alone mode.
2. **SAMEORIGIN**: Limits framing to the same origin (domain), shielding against external embedding.
3. **ALLOW-FROM uri**: Permits limited framing from a specific URI, allowing conditional embedding.

Implementation:

- **HTTP Header**:

  ```http
  X-Frame-Options: SAMEORIGIN
  ```

- **Meta Tag**:

  This meta tag should be placed in the `<head>` section of HTML pages.

  ```html
  <meta http-equiv="X-Frame-Options" content="SAMEORIGIN">
  ```

Content Security Policy (CSP)

**CSP** is a powerful header that defines approved sources for different types of content, offering granular control over what can be loaded on a page.

- **frame-ancestors Directive**: Limits frame embedding. Setting it to `'self'` or a specific domain can help prevent clickjacking.

  ```http
  Content-Security-Policy: frame-ancestors 'self'
  ```

JavaScript-Based Defense

- **Frame-Busting Scripts**: Adopts JavaScript to ensure a page doesn't load in a frame, thereby preventing clickjacking.

  For instance, incorporate the below code into the `<head>` section:

  ```html
  <script>
    if (self !== top) {
      top.location = self.location;
    }
  </script>
  ```

- **Pointer Lock API**: More elaborate, this API can be leveraged to confine mouse movements to a specified region on a web page.

   Using the Pointer Lock API might not be a general solution; however, the lock will prevent an iframe from moving its related elements during clickjacking attempts.

  startPointerLock()

Ensure Safe Browsing Experience with Clickjacking Protection

When an application or website deploys robust defense mechanisms like **X-Frame-Options**, **Content Security Policy**, and **JavaScript-based tactics**, it substantially reduces the risk of clickjacking, ensuring a secure and trustworthy browsing experience for users.

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>How can web cookies compromise security, and how do you safeguard against these risks?</summary>

### How can web cookies compromise security, and how do you safeguard against these risks?

**Definition:**

**Cookies** can pose security risks like **session hijacking**, **cross-site scripting**, and **data exposure**. But these can be mitigated with specific approaches.

**How it works:**

Risks and Mitigations

Session Hijacking

- **Risk**: Unauthorized access if a session ID is stolen.
- **Safeguard**: Use HTTP-only and Secure flags. Regularly change session IDs.

Cross-Site Scripting (XSS)

- **Risk**: Attackers inject malicious scripts, stealing user data or sessions.
- **Safeguard**: Sanitize inputs; utilize `HttpOnly` and `Secure` flags; employ cookie prefixes, e.g., `__Host-` and `__Secure-`.

Data Exposure

- **Risk**: Sensitive data sent in cookies can be intercepted.
- **Safeguard**: Use cookies **only** for essential session data.

Best Practices

- **Limit Cookie Data**: Avoid transmitting sensitive user information.
- **Use Secure and HTTP-Only**: Set these attributes to protect against unauthorized data access and transmission over unsecured channels.
- **Regularly Update Session Identifiers**: For added security, update session identifiers periodically.
- **Encrypt Cookies**: If essential data has to be stored, ensure it's encrypted.
- **Cookie Prefixes**: Use `__Host-` and `__Secure-` to provide additional security.

**Examples:**

Implementation Example: Setting up Secure Cookies

Here is the Python code:

```python
from flask import Flask, request, make_response

app = Flask(__name__)

@app.route('/')
def index():
    resp = make_response(render_template('index.html'))
    resp.set_cookie('user_id', '1234', secure=True, httponly=True, samesite='Strict')
    return resp

if __name__ == '__main__':
    app.run()
```

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>What is a Man-in-the-Middle (MitM) attack and how can it be prevented?</summary>

### What is a Man-in-the-Middle (MitM) attack and how can it be prevented?

**Definition:**

**Man-in-the-Middle (MitM)** attacks occur when a malicious actor intercepts communication between two parties. This breach can lead to data theft or falsification of information.

**How it works:**

Common MitM Scenarios

- **Wi-Fi Networks**: Unsecured public Wi-Fi networks are prime targets for MitM attacks.
- **Spoofed Sites**: Attackers create fake websites to capture login credentials.
- **Email**: Encrypted emails can be intercepted, read, and modified.

Preventive Measures

1. **Digital Certificates**: Use SSL/TLS on servers and ensure proper certificate validation on clients.
2. **Encryption**: Apply end-to-end encryption to communication channels.
3. **Public Key Infrastructure (PKI)**: Employ validated digital certificates for secure identification and data integrity.
4. **Multi-Factor Authentication (MFA)**: Add layers of identity checks beyond passwords.
5. **Strong Password Policies**: Mandate complex passwords and encourage regular updates.
6. **Awareness Programs**: Train users to spot suspicious behavior and potential MitM indicators.
7. **Secure Applications**: Use authenticated, verified, and updated software. Ensure regular security audits.

Additional Layers with Transport Layer Security (TLS)

- **Certificate Pinning**: Associating a specific certificate with an application to prevent unauthorized replacements.
- **HTTP Strict Transport Security (HSTS)**: Directing web browsers to interact with the server using only HTTPS.

Network Mechanisms to Counter MitM Threats

- **Ethernet Authentication**: IEEE 802.1X allows for network access control, reducing MitM risks.
- **Wi-Fi Protected Access 2 (WPA2)**: Enhanced security protocols over WEP and WPA, bolstering wireless security.
- **Intrusion Detection Systems (IDS) and Intrusion Prevention Systems (IPS)**: Active monitoring for suspicious network activity.

**Examples:**

Code Example: SSL/TLS in Node.js

Here is the Node.js code:

```javascript
const https = require('https');
const fs = require('fs');

const options = {
  key: fs.readFileSync('key.pem'),
  cert: fs.readFileSync('cert.pem')
};

https.createServer(options, (req, res) => {
  // Handle secure requests
}).listen(443);
```

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>

<details>
<summary>Describe the concept of session management in web security.</summary>

### Describe the concept of session management in web security.

**Definition:**

**Session management** is a pivotal mechanism in web security. It maintains state between web pages and securely manages users' identities and permissions. It employs a combination of cookies, tokens, and server-side data to monitor user activities across a single session.

**How it works:**

Importance of Session Management for Web Security

- **Data Protection**: Sensitive user details are less exposed, mitigating the risk of unauthorized access.
- **Access Control**: It ensures that only authenticated users access protected resources.
- **User Privacy**: By limiting tracking to the current session, it enhances user privacy.

Common Session Management Mechanisms

- **Cookies**: Text files stored on the user's device that include session identification.
- **URL Rewriting**: Append session IDs to URLs.
- **Hidden Form Fields**: Store session IDs in web forms.
- **HTTP Headers**: Utilize custom headers for session tracking.
- **Sessions**: Server-side storage, maintained between server and client via a unique session ID.

The Risks Associated with Poor Session Management

- **Session Hijacking**: Unauthenticated entities assume control over a session.
- **Session Fixation**: Attackers manipulate the session to gain unauthorized access.
- **Cross-Site Request Forgery (CSRF)**: Malevolent sites instigate user action on another site where the user is authenticated.

Best Practices for Secure Session Management

- **SSL/TLS Encrypted Connections**: Deploy HTTPS to secure data in transit.
- **Time-Out Sessions**: End inactive sessions after a predefined period.
- **Randomized Session IDs**: Generate unique, hard-to-guess session IDs.
- **Server-Side Validation**: Crosscheck session details on the server for added security.
- **Cookies with Secure and HTTPOnly Flags**: Cookies should only be sent over secure connections and remain inaccessible to JavaScript.

Common Web Vulnerabilities Related to Session Management

- **Session Prediction/Inference**: Predictable session IDs increase the vulnerability to attacks.
- **Misconfigured Cross-Origin Resource Sharing (CORS) Headers**: Poorly configured headers might allow unauthorized websites to access a user's session data.
- **Transport Layer Security (TLS) Misconfigurations**: Weak or outdated TLS settings may compromise session security.

**Examples:**

Code Example: Setting a Secure Cookie

Here is the Java code:

```java
Cookie sessionCookie = new Cookie("sessionID", UUID.randomUUID().toString());
sessionCookie.setSecure(true);  // Ensures the cookie is only sent over HTTPS
response.addCookie(sessionCookie);  // Add the cookie to the response
```

In a Java web application, the `response` object is used to send the cookie to the client.

**Sources:**

- [Devinterview.io — Web Security](https://devinterview.io/questions/web-and-mobile-development/web-security-interview-questions/)

</details>


### Authentication and Authorization

<details>
<summary>What is the difference between authentication and authorization?</summary>

### What is the difference between authentication and authorization?

**Definition:**

**Authentication** verifies that an entity is who it claims to be, using one or more authenticators such as a password, fingerprint, or security token. **Authorization** verifies that a requested action or service is approved for that entity. A person who has signed in is often still not allowed to perform every action the system can technically do.

**How it works:**

OWASP treats the two checks as separate. Authentication establishes identity. Authorization then decides which operations that identity may perform, and some public resources are authorized without any login.

On the HTTP authentication framework, a failed or missing credential challenge uses `401` (Unauthorized) with `WWW-Authenticate`. Valid credentials that are still inadequate for the resource use `403` (Forbidden). The server may instead return `404` (Not Found) when it should not reveal that the resource exists.

Defenses: deny by default, enforce least privilege, and validate the permission on every request on the server, gateway, or serverless function. Client-side checks are only for the interface. When a check fails, stop in a safe state and do not leak logs or debug detail in the error.

**When to use:**

Authenticate when you must know who the caller is. Authorize on every protected operation after that, including public reads that are intentionally allowed without a login. Use `401` when the caller should try again with credentials, and `403` when this already-known caller must not have the resource.

**Examples:**

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Basic realm="Access to the staging site"
```

**Sources:**

- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [MDN — HTTP authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Authentication)
</details>

<details>
<summary>Explain basic authentication and its weaknesses.</summary>

### Explain basic authentication and its weaknesses.

**Definition:**

HTTP **Basic** authentication, defined in RFC 7617, sends a user id and password as a base64-encoded pair in the `Authorization` request header. Base64 is a reversible encoding, not encryption. The server challenges the client with `401` and a `WWW-Authenticate` header that names the scheme and a realm.

**How it works:**

The browser usually prompts, then repeats the request with `Authorization: Basic` plus the encoded credentials. Those credentials are sent on later requests to that realm. MDN states that this is insecure unless the exchange is over HTTPS/TLS, because anyone who can read the bytes can recover the password.

Basic is also a weak fit for browser apps that change data: the user agent attaches the credentials regardless of which site triggered the request, so the site needs HTTPS, state-changing requests that are not simple cross-site GETs, and CSRF tokens. Without those controls, Basic should not protect sensitive or valuable information. Do not put the user id and password in the URL; modern browsers strip that form before the request is sent.

**When to use:**

Use Basic only for low-sensitivity access over TLS, or for a proxy challenge (`407`, `Proxy-Authenticate`, `Proxy-Authorization`). Prefer a modern authenticator, short-lived bearer or sender-constrained tokens, and server-side authorization for application data.

**Examples:**

```http
WWW-Authenticate: Basic realm="Access to the staging site"
Authorization: Basic <base64 user-id:password>
```

**Sources:**

- [MDN — HTTP authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Authentication)
</details>

<details>
<summary>What is OAuth, and how does it work?</summary>

### What is OAuth, and how does it work?

**Definition:**

**OAuth 2.0** lets a client application access a protected resource on behalf of a resource owner, with that owner's authorization. An authorization server issues an access token after authenticating the resource owner and obtaining consent. The resource server accepts that token instead of learning every authentication scheme the owner might use.

**How it works:**

The usual parties are the client, the resource owner (the end user when a person is granting access), the authorization server, and the resource server. Access tokens should be short-lived and audience-restricted, preferably to one resource server. Refresh tokens obtain new access tokens and should be sender-constrained (DPoP or mTLS) or rotated.

Current practice is the authorization code grant with PKCE (`response_type=code`) for every client type, including browser and native apps. The implicit grant (`response_type=token`) is deprecated by RFC 9700 and removed from OAuth 2.1 because it places access tokens in the URL fragment. Do not use the resource-owner password grant: it exposes the owner's credentials to the client. Clients must not forward the browser to an arbitrary URI from a query parameter. Authorization responses must not travel over unencrypted connections; `http` redirect URIs are limited to native loopback clients.

**When to use:**

Use OAuth when a client needs delegated access to an API. Use OpenID Connect when the client must verify who the end user is. Add DPoP or mTLS when a leaked bearer token must not be replayable by itself, especially for sensitive or long-lived tokens.

**Examples:**

```http
GET /authorize?response_type=code&client_id=app&code_challenge=<S256>&code_challenge_method=S256
```

**Sources:**

- [OWASP OAuth2 Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/OAuth2_Cheat_Sheet.html)
</details>

<details>
<summary>What is OpenID and how does it relate to web security?</summary>

### What is OpenID and how does it relate to web security?

**Definition:**

**OpenID Connect 1.0** is an identity layer on top of OAuth 2.0. It lets a relying party verify the end user's identity from an authentication performed by an authorization server, and obtain basic profile claims in an interoperable way. The ID token is a signed JWT. OWASP's guidance is to use OpenID Connect for authentication and single sign-on, and OAuth for authorization to APIs.

**How it works:**

The client (relying party) starts at the authorization server and receives an ID token whose signature, issuer, and audience it must check. Access tokens for APIs still come from the token endpoint, not from the front channel. In OpenID Connect flows the `nonce` parameter provides CSRF protection; otherwise use a one-time `state` value bound to the user agent. PKCE remains required for the authorization code.

Tokens are bearer credentials: whoever holds one can present it. Send them only over TLS. Do not put them in browser `localStorage`. Prefer secure, HTTP-only cookies or the platform's secure storage, keep access tokens short-lived, and rotate refresh tokens. Because OpenID Connect removes user-managed passwords from the application, the application no longer stores those passwords; it must protect the tokens instead.

**When to use:**

Choose OpenID Connect when the product needs login or SSO through an identity provider. Choose OAuth alone when the client is calling an API with a previously authorized grant and does not need an ID token.

**Examples:**

```http
GET /authorize?response_type=code&scope=openid&nonce=<transaction-specific>&code_challenge=<S256>&code_challenge_method=S256
```

**Sources:**

- [OWASP OAuth2 Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/OAuth2_Cheat_Sheet.html)
- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
</details>

<details>
<summary>Can you describe Multi-Factor Authentication (MFA) and where it's used?</summary>

### Can you describe Multi-Factor Authentication (MFA) and where it's used?

**Definition:**

**Multifactor authentication** requires evidence from at least two distinct factors: something you know, something you have, and something you are. Two passwords, or a password and a PIN, are the same factor and are not MFA. Location and other risk signals can inform a decision, but NIST guidance cited by OWASP says they do not substitute for a factor.

**How it works:**

OWASP treats MFA as the strongest practical defense against account compromise that starts from a weak, reused, or stolen password, and recommends it for all users, with TOTP as an option users can turn on and a hard requirement for administrative accounts. The factors must be independent so one compromise does not yield both.

Require MFA at login and again for sensitive actions: changing the password, changing the email address, disabling MFA, or raising a session to an administrator. Every login path, including a separate API or a companion mobile app, needs the same bar. One-time passwords need a short lifetime, single use, a strict attempt limit, and invalidation after success. Do not log them or store them as long-term plaintext. Provide a secure reset path; a casual bypass becomes the weak factor.

**When to use:**

Require MFA for every interactive login when users can enroll a second factor, and always for administrators and for the sensitive account changes listed above. Offer TOTP rather than treating SMS as equivalent; the cheat sheet lists SMS and phone calls as restricted.

**Examples:**

```python
# Distinct factors, not two secrets of the same kind.
know = password
have = totp  # short TTL, single use, attempt limit
```

**Sources:**

- [OWASP Multifactor Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html)
</details>

<details>
<summary>What is JWT (JSON Web Tokens) and how are they used in authentication?</summary>

### What is JWT (JSON Web Tokens) and how are they used in authentication?

**Definition:**

A **JSON Web Token** (RFC 7519) is a compact, URL-safe way to carry claims between two parties. The claims are a JSON object used as the payload of a JSON Web Signature or as the plaintext of a JSON Web Encryption, so they can be signed, integrity-protected with a MAC, and/or encrypted. OWASP describes a signed JWT as a protected identity card: the issuer's signature stops a client from forging claims such as an administrator role.

**How it works:**

A signed token is three base64url parts: protected header, claims, and signature. The signature is either a public-key signature or a MAC over a shared secret. Registered claims include `iss`, `sub`, `aud`, `exp`, `nbf`, `iat`, and `jti`.

In OpenID Connect the ID token is a JWT. In OAuth 2 an access token may be a JWT. A JWT is often suggested for stateless sessions; OWASP frowns on that use. The resource server must verify integrity and must not choose the algorithm from the token header, including `alg` of `none`. For API access tokens, require and check `iss`, `aud`, and `exp`. If the user logs out or an idle timeout ends the session before `exp`, place the server-issued `jti` (optionally with `aud`) on a denylist until the token expires. Prefer signatures over MACs when many services would otherwise share a key that can also mint tokens.

**When to use:**

Use a signed JWT as an ID token or as an access token when the resource server can validate issuer, audience, and expiry. Do not use a long-lived JWT as the only session record if you must revoke access on logout or idle timeout; keep server-side invalidation for that case.

**Examples:**

```text
# Signed JWT: base64url(header).base64url(claims).base64url(signature)
# Verify iss, aud, and exp with the key you configured — not with alg from the header.
```

**Sources:**

- [RFC 7519 — JSON Web Token (JWT)](https://www.rfc-editor.org/rfc/rfc7519.html)
- [OWASP JSON Web Token Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/JSON_Web_Token_Cheat_Sheet.html)
- [OWASP REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
</details>

<details>
<summary>How do you securely store user passwords?</summary>

### How do you securely store user passwords?

**Definition:**

Store passwords with a slow, adaptive password hash, not with reversible encryption and not with a fast hash such as SHA-256. OWASP's order is Argon2id, then scrypt if Argon2id is unavailable, bcrypt only on legacy systems, and PBKDF2-HMAC-SHA-256 when FIPS 140 validation is required. Each password gets a unique salt so precomputed tables cannot match many users at once.

**How it works:**

Argon2id minimums published on the cheat sheet include 19 MiB of memory, 2 iterations, and parallelism 1 (do not use that set with Argon2i). Bcrypt, when you must keep it, needs a work factor of at least 10 and a 72-byte password limit. PBKDF2-HMAC-SHA-256 needs at least 600,000 iterations. A pepper stored outside the password database, in a secrets vault or HSM, adds a hurdle when only the database is taken. Compare with the language's password verify function, or with a constant-time compare, and do not silently truncate.

You do not need to hide which modern algorithm you use. Re-hash on the next successful login when you raise the work factor. Transmit passwords only over TLS, on the login page and on later authenticated pages. Avoid periodic forced changes; rotate credentials when a leak is identified or the authenticator technology changes.

**When to use:**

Use Argon2id for new applications. Use scrypt where Argon2id is missing, PBKDF2 where a FIPS-validated implementation is mandatory, and bcrypt only while an older deployment cannot move yet.

**Examples:**

```java
# Legacy bcrypt only. Prefer Argon2id (19 MiB, t=2, p=1) when the library exists.
String hash = BCrypt.hashpw(password, BCrypt.gensalt(12));
```

**Sources:**

- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
</details>

<details>
<summary>What is Single Sign-On (SSO) and what are its benefits?</summary>

### What is Single Sign-On (SSO) and what are its benefits?

**Definition:**

**Single sign-on** lets a user authenticate once with an identity provider and then receive authentication and authorization assertions that relying applications accept. Two profiles OWASP documents for this are OpenID Connect (an identity layer on OAuth 2.0, with a signed ID token) and the SAML Web Browser SSO profile with Redirect/POST bindings.

**How it works:**

The benefit is one authentication service that can require MFA, short-lived tokens, and a consistent account policy, instead of every application storing its own passwords. OWASP notes that OpenID Connect reduces password-related risk by moving authentication to an identity provider.

The relying party still has to check the assertion. For SAML, use TLS 1.2 for confidentiality and integrity in transit, require a digital signature with a certified key, and require the profile's data elements: the authentication request's ID and service provider, `InResponseTo` on the response, and an assertion signed with the identity provider's private key that names the client, the identity provider, and the service provider. Cookie-authenticated SSO paths still need CSRF protection; token-only APIs do not replace that on the cookie paths. Do not let the application's front end share an internal directory that is also used for unsecured public access.

**When to use:**

Use SSO when several applications should share one identity provider. Prefer OpenID Connect for new web and API login. Use SAML where an existing enterprise identity provider already speaks that profile, and validate every required element before creating a local session.

**Examples:**

```text
# OpenID Connect: authenticate at the IdP, then accept a signed ID token.
# SAML: require InResponseTo and an assertion signed by the IdP.
```

**Sources:**

- [OWASP SAML Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SAML_Security_Cheat_Sheet.html)
- [OWASP OAuth2 Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/OAuth2_Cheat_Sheet.html)
</details>

<details>
<summary>Explain role-based access control (RBAC) and how it is implemented.</summary>

### Explain role-based access control (RBAC) and how it is implemented.

**Definition:**

**Role-based access control** grants or denies access from roles assigned to a user. Permissions attach to the role, and the user inherits the permissions of their roles. Users and roles are typically many-to-many, and roles may form a hierarchy. OWASP distinguishes this from attribute-based access control, which also considers subject, object, and environment attributes, and from relationship-based access control, which uses relationships between a user and an object.

**How it works:**

Implement checks on the server for every request, with a default of deny. Map, during design, which role may perform which operation on which resource, then test that map. Review permissions after deployment so they do not creep past the approved set. Do not let the client be the place that decides.

OWASP prefers attribute- or relationship-based decisions for most application development because a role check is coarse: it struggles with object-level rules, many roles create a role explosion, and a chain of `hasAnyRole` calls is easy to get wrong. A role can still be one attribute inside an attribute-based policy. Keep identifiers from becoming the only control: even a hard-to-guess id needs an authorization check for that specific object.

**When to use:**

Use a small RBAC model when a few stable job functions really do match the permissions. Move to attributes or relationships when access depends on the specific object, the time, the device, or who created the record.

**Examples:**

```python
if not user.has_role("ACCT_MANAGER"):
    return  # deny by default; check the object too, not only the role
```

**Sources:**

- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
</details>

<details>
<summary>What are the best practices for handling user session timeouts?</summary>

### What are the best practices for handling user session timeouts?

**Definition:**

Session timeout limits how long a server-side session stays valid. OWASP requires both an **idle timeout** (no HTTP request for that session id) and an **absolute timeout** (measured from when the application created the session, regardless of activity). After either limit, the user must authenticate again and receive a new session.

**How it works:**

Enforce both limits on the server. If the client stores the login time or the remaining minutes, that value can be changed to stretch the session. When the session expires or the user logs out, invalidate it on the server with the platform call (`HttpSession.invalidate()`, `Session.Abandon()`, or `session_destroy()` / `unset()`) and clear the client token. For a cookie, send an empty session id and an `Expires` (or `Max-Age`) date in the past.

Idle timeout also limits how much session state a quiet client can hold, which matters for availability as well as for account takeover. Absolute timeout caps a session that stays active. Do not rely on the browser closing to end the session.

**Examples:**

```http
Set-Cookie: id=; Expires=Friday, 17-May-03 18:45:00 GMT
```

**Sources:**

- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
</details>


### Network Security Concerns

<details>
<summary>What is a firewall, and how does it contribute to web security?</summary>

### What is a firewall, and how does it contribute to web security?

**Definition:**

A **firewall** is a policy enforcement point that allows or blocks traffic between network zones. In a segmented web architecture, crossing from the internet-facing front end into application or data zones means crossing a firewall. OWASP's Zero Trust sheet also lists firewalls, proxies, application gateways, and API gateways as the components that enforce access decisions, not only at the network edge.

**How it works:**

Place internet-facing services in a front-end zone that includes the load balancer, an application-layer firewall, the web server, and cache. Keep application logic in a middleware zone and databases, directories, and key storage in a back-end zone. Write the allowed paths down: a front end may reach its own application tier, and that tier may reach its own database. A front end must not reach another system's database directly.

A host firewall such as UFW does not automatically govern ports Docker publishes. Docker inserts its own packet rules, so a published port can be reachable even when a deny rule exists. Bind published ports to `127.0.0.1` when they should stay local, or integrate the firewall with Docker's rules. A firewall is one layer; application authorization still runs in server-side code.

**Examples:**

```bash
# Local only. Publishing 0.0.0.0 bypasses a typical UFW deny.
docker run -p 127.0.0.1:8000:8000 myimage
```

**Sources:**

- [OWASP Network Segmentation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Network_Segmentation_Cheat_Sheet.html)
- [OWASP Zero Trust Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Zero_Trust_Architecture_Cheat_Sheet.html)
- [OWASP Docker Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html)
</details>

<details>
<summary>Define a Virtual Private Network (VPN) and its significance in enhancing web security.</summary>

### Define a Virtual Private Network (VPN) and its significance in enhancing web security.

**Definition:**

In the guidance OWASP gives for remote access, a **VPN** is a network connection that must not, by itself, authorize access to other resources. Zero Trust treats a VPN that grants broad network access as the old model. The replacement is access to specific resources, with any remaining VPN restricted and monitored. TLS guidance adds that a VPN gateway and a public web server must not share one wildcard certificate.

**How it works:**

Prefer an identity-aware path to the specific application over a tunnel that drops the user onto the internal network. If a VPN is still required during a migration, restrict it and watch it. Device health is monitored continuously, and access is revoked when the device is no longer compliant. A VPN gateway can share a wildcard certificate with another VPN gateway; it must not share that certificate with a public web server or an internal server at a different trust level.

Privacy features and VPNs also reduce the accuracy of location checks used as a risk signal. Do not treat a VPN exit address as proof of who the user is.

**When to use:**

Use a tightly scoped VPN only while a specific system still cannot sit behind per-resource checks. For web applications, prefer an identity-aware proxy that authorizes the user and device for that app.

**Examples:**

```text
# Do not reuse one wildcard certificate across trust levels.
# A VPN gateway and a public web server need separate certificates.
```

**Sources:**

- [OWASP Zero Trust Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Zero_Trust_Architecture_Cheat_Sheet.html)
- [OWASP Transport Layer Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html)
</details>

<details>
<summary>What is an Intrusion Detection System (IDS) vs. an Intrusion Prevention System (IPS)?</summary>

### What is an Intrusion Detection System (IDS) vs. an Intrusion Prevention System (IPS)?

**Definition:**

An **intrusion detection system** records suspicious activity so people and tooling can investigate it. OWASP's logging guidance lists network and host intrusion detection systems (NIDS and HIDS) among the places security events come from. An **intrusion prevention system** is an inline enforcement layer: the virtual-patching guidance names an IPS appliance, next to a WAF, as a device that can block matching attempts against a known vulnerability without changing the vulnerable code.

**How it works:**

Detection feeds the log stream: authentication failures, policy violations, and other security events that a central store or SIEM can correlate. Prevention sits on the path and can block traffic the rule recognizes. A virtual patch, whether on an IPS or a WAF, only covers traffic that device sees and variants its rules recognize. It does not remove the vulnerability. Test legitimate workflows before blocking, keep an audit trail of rule id and outcome, and continue the code or vendor fix.

Do not treat either box as a substitute for server-side authorization, parameterized queries, or patching.

**When to use:**

Use detection when you need visibility and an investigation trail. Add inline prevention when you must reduce exposure to a known issue before the code change ships, and remove or retire that rule after the real fix is in place.

**Examples:**

```text
# Detective: NIDS/HIDS contribute events to the application log stream.
# Preventive: an IPS or WAF can block a known pattern while the code fix is prepared.
```

**Sources:**

- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [OWASP Virtual Patching Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Virtual_Patching_Cheat_Sheet.html)
</details>

<details>
<summary>How does a Distributed Denial-of-Service (DDoS) attack work, and how can websites be protected from it?</summary>

### How does a Distributed Denial-of-Service (DDoS) attack work, and how can websites be protected from it?

**Definition:**

A denial-of-service condition removes availability, one part of the CIA triad. OWASP groups the surfaces as application (the app becomes unusable or runs out of resources without filling the network), session or protocol (server or middlebox resources, including firewalls and load balancers), and network (bandwidth saturation). A distributed attack uses many sources against those same surfaces.

**How it works:**

There is no single switch. Inventory components, find single points of failure, and design for scaling, redundancy, and bulkheads so one failure does not spread. Degrade gracefully. Do cheap validation before expensive work. Cap upload size, total request size, and work that grows with input. Set an idle session timeout and an absolute timeout, and keep little data on each session. Authenticate before expensive functions, but do not let account lockout itself become the outage.

For slow requests, define a minimum ingress rate with a read timeout, an absolute connection timeout, and a maximum rate and bandwidth, tuned from real traffic so legitimate slow clients are not rejected wholesale. Cache and serve static files from another host so the application receives fewer requests. Ask the ISP about capacity and filtering, and consider a filtering service for large volumetric events. Rate limits can also return `429 Too Many Requests`.

**Examples:**

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 30
```

**Sources:**

- [OWASP Denial of Service Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Denial_of_Service_Cheat_Sheet.html)
- [OWASP REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
</details>

<details>
<summary>Explain the concept of a Content Delivery Network (CDN) and its importance in web security.</summary>

### Explain the concept of a Content Delivery Network (CDN) and its importance in web security.

**Definition:**

Sites sometimes host scripts and styles on a third party, including a **content delivery network**, instead of only on their own origin. That reduces load on the application origin. OWASP's availability guidance says caching and hosting static resources on a different domain make the application more resilient to bandwidth exhaustion. The same split creates a supply-chain risk: if the third-party host is changed, the browser would otherwise run whatever bytes it receives.

**How it works:**

Subresource Integrity is the browser control for that risk. Put an `integrity` attribute on `<script>` and on `<link rel="stylesheet">` (also `preload` and `modulepreload`) with a `sha256`, `sha384`, or `sha512` hash. The browser uses the strongest algorithm present and refuses the resource on mismatch. Cross-origin integrity requires CORS, so set `crossorigin="anonymous"`. A typical CDN sends `Access-Control-Allow-Origin: *`.

SRI checks bytes. It does not decide whether you should execute that third party's code at all. Limit tags to what you need, and prefer self-hosting when you can version the file yourself. `Integrity-Policy` can require integrity metadata for scripts or styles instead of hoping every tag includes it.

**Examples:**

```html
<script
  src="https://cdn.example.com/script.js"
  integrity="sha384-<base64-hash>"
  crossorigin="anonymous"></script>
```

**Sources:**

- [MDN — Subresource Integrity](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Subresource_Integrity)
- [OWASP Denial of Service Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Denial_of_Service_Cheat_Sheet.html)
</details>


### Secure Coding Practices

<details>
<summary>What are secure coding practices, and why are they needed?</summary>

### What are secure coding practices, and why are they needed?

**Definition:**

**Secure coding** is the quality bar on how a product's behavior is implemented, not only that the feature works. OWASP's secure-product guidance says products must meet the organization's security requirements and that security decisions must be explicit. The practices exist so a missing check is not discovered only after the system is in production.

**How it works:**

The sheet's baseline list is: validate input types, formats, lengths, and business constraints, and still use parameterized queries for SQL values; handle errors without disclosing sensitive detail; authenticate and authorize; use HTTPS and encryption at rest at the level the product requires; run code and hosts with least privilege; use memory-safe languages or explicit bounds; keep secrets out of source and in a secure store; test for vulnerabilities during development and before deployment; review code, including with automated tools; and keep dependencies and the platform patched.

Establish secure defaults, minimize what you expose, and fail closed. Repeat the checks at each tier. Threat modeling supplies the stories those checks must cover.

**Examples:**

```python
# SQL values are parameters. Validation does not replace that.
cursor.execute("SELECT balance FROM accounts WHERE id = ?", (account_id,))
```

**Sources:**

- [OWASP Secure Product Design Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Product_Design_Cheat_Sheet.html)
</details>

<details>
<summary>How can input validation prevent web security threats?</summary>

### How can input validation prevent web security threats?

**Definition:**

**Input validation** checks that data meets the application's requirements before the application uses it. It reduces malformed data, invalid business operations, and excessive resource use. OWASP separates it from the other defenses that are still required: parameterized queries for SQL, context-aware output encoding for cross-site scripting, and a separate authorization check. A well-formed account id does not mean the caller may read that account.

**How it works:**

Validate syntax and meaning. Fixed choices must be members of the allowed set, including values that came from a menu. Numbers and dates need type, format, and bounds. Strings need length and the structure the field requires. Objects need allowed and required fields. Arrays need size limits and a check of every item.

Define what you accept and reject the rest. Do not try to recognize every dangerous string. Apply size limits before parsing, use a maintained parser, and validate the decoded value you will actually use. Reject the request instead of continuing with a partial object. Client-side checks are for immediate feedback; repeat them on the server. Data from internal APIs, partners, queues, and stored records is still untrusted when it crosses a trust boundary.

**Examples:**

```python
import re
if re.fullmatch(r"[0-9]{1,6}", account_id) is None:
    raise ValueError("account id")
```

**Sources:**

- [OWASP Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
</details>

<details>
<summary>What are parameterized queries, and how do they prevent SQL injection?</summary>

### What are parameterized queries, and how do they prevent SQL injection?

**Definition:**

A **parameterized query** (a prepared statement with bound variables) forces the SQL text to be defined first and each data value to be passed later. The database then treats user input as data, not as part of the statement, so the statement's intent stays the one the developer wrote.

**How it works:**

OWASP lists this as the primary defense. Stop building queries by concatenating strings. Stored procedures prevent the same class of flaw only when they are themselves parameterized; dynamic SQL inside a procedure brings the risk back. If a table name, column name, or sort direction cannot be a bound parameter, map the user value onto a fixed allow-list in code and append only that chosen literal. Escaping user input is strongly discouraged and is database-specific.

Keep a second layer: allow-list validation, and database accounts with only the rights that feature needs. Do not give the application a DBA account. Different features can use different database users so a login path cannot write.

**Examples:**

```java
String query = "SELECT account_balance FROM user_data WHERE user_name = ?";
PreparedStatement pstmt = connection.prepareStatement(query);
pstmt.setString(1, custname);
ResultSet results = pstmt.executeQuery();
```

**Sources:**

- [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
</details>

<details>
<summary>Why should developers avoid security through obscurity?</summary>

### Why should developers avoid security through obscurity?

**Definition:**

**Security through obscurity** treats secrecy of an identifier, a header, or an algorithm name as if it were an access check. OWASP's authorization guidance says that making an object id hard to guess is not enough: the user must not gain an object merely by knowing its id. Access control has to authorize that object. The password-storage guidance likewise says you do not need to hide which modern password hash you use when it is configured properly.

**How it works:**

Hiding the `Server` or `X-Powered-By` header is still worth doing because those headers advertise software, but OWASP notes that other fingerprinting remains. Removing a version header is hygiene, not the control that stops a vulnerable parser.

Prefer designs that stay safe when the mechanism is known: parameterized queries, server-side authorization, slow password hashes with public parameters, and TLS. Deny by default so a newly added route is not exposed because nobody wrote a rule. Custom cryptographic algorithms are rejected outright by the cryptographic-storage guidance.

**Examples:**

```python
# Authorize the object. A random id is not the access check.
if not can_read(user, account_id):
    abort(404)
```

**Sources:**

- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- [OWASP HTTP Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)
</details>

<details>
<summary>How do you securely handle file uploads to prevent web security issues?</summary>

### How do you securely handle file uploads to prevent web security issues?

**Definition:**

File upload lets a user send a photo, a document, or similar content. OWASP's defensive baseline is an allow-list of extensions the business needs, validation of the file type rather than trust in the client `Content-Type`, a server-generated file name, limits on name length and size, authorization of the uploader, storage outside the web root or on another host, malware scanning where you have it, and CSRF protection on the upload.

**How it works:**

No single check is sufficient. Decode the file name before you judge the extension, and allow only the types you agreed, such as one image type or `docx` and `pdf` for a CV. Generate a UUID instead of keeping the client's name. Restrict any user-visible name to a short allowed character set and reject leading dots. Store files outside the web root with least-privilege permissions, and disable per-directory server configuration on the upload directory so a dropped configuration file cannot change how the server treats types.

When the browser must download rather than render, send `Content-Disposition: attachment`, `Content-Type: application/octet-stream` for unknown bytes, and `X-Content-Type-Options: nosniff`. Cap size before you expand archives. Only authenticated users with permission for that operation should upload.

**Examples:**

```http
Content-Disposition: attachment
Content-Type: application/octet-stream
X-Content-Type-Options: nosniff
```

**Sources:**

- [OWASP File Upload Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html)
- [OWASP HTTP Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)
</details>


### Data Protection and Privacy

<details>
<summary>Define Personally Identifiable Information (PII) and discuss how it should be protected.</summary>

### Define Personally Identifiable Information (PII) and discuss how it should be protected.

**Definition:**

**Personally identifiable information** is private information an application holds about a person. OWASP's mobile guidance tells developers to minimize it, replace it with less sensitive data where that still meets the need, keep less of it (for example fewer location updates), expire and delete it, and ask for consent before collection or use. The user-privacy sheet treats identities, private information, and communications as data that must be protected with strong cryptography.

**How it works:**

Encrypt communications in transit with TLS, verified certificates, protected private keys, and strong ciphers. Encrypt private data at rest with keys of sufficient length and strict access, technical and procedural. Hash credentials even when the store is also encrypted. Support HTTP Strict Transport Security so browsers keep using HTTPS.

Do not log full request bodies, secrets, or authentication material. Give users a way to see sessions and invalidate a device that was lost. If you cannot legally or technically prevent disclosure of logs, say so plainly. Block optional third-party content when loading it would reveal the user's address to another host.

**Examples:**

```http
Cache-Control: no-store
```

**Sources:**

- [OWASP Mobile Application Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Mobile_Application_Security_Cheat_Sheet.html)
- [OWASP User Privacy Protection Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/User_Privacy_Protection_Cheat_Sheet.html)
- [OWASP HTTP Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)
</details>

<details>
<summary>What is the General Data Protection Regulation (GDPR), and what are its key tenets?</summary>

### What is the General Data Protection Regulation (GDPR), and what are its key tenets?

**Definition:**

The **General Data Protection Regulation** is the European Union privacy regulation. OWASP's multifactor guidance notes that finance and healthcare rules, and GDPR, often require MFA. The Zero Trust cheat sheet maps GDPR-related controls to privacy-by-design, logging of data access, and breach detection. Those are the tenets the fetched OWASP pages actually name; they are not a reprint of the regulation's articles.

**How it works:**

Privacy-by-design, in the product guidance those pages sit next to, means collecting less personal data, encrypting it in transit and at rest, hashing passwords, and telling users honestly what you cannot protect. Access logging records who reached personal data and when, without storing the data itself in the log. Breach detection depends on that trail plus monitoring, so an incident is visible rather than silent.

MFA for accounts that can reach personal data is the control the authentication sheet ties to GDPR compliance. Pair it with the right to practical session invalidation when a device is lost, which the privacy sheet recommends.

**Examples:**

```text
# Privacy-by-design controls named alongside GDPR in the Zero Trust sheet:
# less collection, access logs, breach detection, and MFA for those accounts.
```

**Sources:**

- [OWASP Zero Trust Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Zero_Trust_Architecture_Cheat_Sheet.html)
- [OWASP Multifactor Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html)
</details>

<details>
<summary>What are data encryption best practices for sensitive information?</summary>

### What are data encryption best practices for sensitive information?

**Definition:**

**Encryption of stored data** should follow the threat model: who you are protecting against decides whether encryption belongs in the application, the database (for example SQL Server TDE), the filesystem (BitLocker or LUKS), or hardware. Hardware encryption can address physical theft and does nothing for an attacker who already runs code on the server. The strongest reduction is to not store the sensitive value, especially card data.

**How it works:**

For symmetric encryption, OWASP prefers AES with a key of at least 128 bits and ideally 256, in an authenticated mode: GCM or CCM. If those are unavailable, CTR or CBC plus encrypt-then-MAC. Do not use ECB outside very specific cases. Do not invent algorithms. For public-key encryption use a maintained hybrid scheme such as HPKE; if RSA is required, use at least 2048-bit keys and OAEP (RFC 8017). RSA and elliptic-curve public keys are not post-quantum secure; long-lived secrecy needs a plan such as ML-KEM (FIPS 203) in a hybrid construction. AES still encrypts the data.

Generate keys with a CSPRNG (`SecureRandom`, `secrets`, `crypto.randomBytes()`, not `Math.random()` or `rand()`). Store them in an HSM, a cloud KMS, or a secrets manager, not in source control. Keep keys apart from ciphertext. Rotate on compromise, on a cryptoperiod, on usage limits of the mode, or when the algorithm weakens. Passwords are hashed, not encrypted, except the rare case where another system still demands the original password and no OIDC-style grant exists.

**Examples:**

```javascript
# Node.js CSPRNG for a 256-bit data key. Do not use Math.random().
const key = crypto.randomBytes(32);
```

**Sources:**

- [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- [OWASP Post-Quantum Cryptography Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Post_Quantum_Cryptography_Cheat_Sheet.html)
</details>

<details>
<summary>Explain data masking and how it can protect sensitive data.</summary>

### Explain data masking and how it can protect sensitive data.

**Definition:**

**Data masking** hides sensitive values from people who can see a screen or a log but do not need the raw data. OWASP's mobile sheet calls out masking sensitive fields in the interface to reduce shoulder surfing. The logging sheet says not to record secrets, full bodies, or authentication material. The HTTP caching guidance says sensitive responses should use `Cache-Control: no-store` so shared caches do not keep them.

**How it works:**

Mask in the UI at the field that a bystander could read, and keep the full value only on the server path that is authorized to use it. In logs, store the fact of the event and a correlation id, not the password, the card number, or the request body. In object storage, prefer IAM access from the application over public objects, and use short-lived credentials rather than a long-lived access key.

Masking is a display and retention control. It does not replace encryption at rest or an authorization check. Randomizing an object id, which the authorization sheet discusses, is also not a substitute for authorizing that object.

**Examples:**

```http
Cache-Control: no-store
```

**Sources:**

- [OWASP Mobile Application Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Mobile_Application_Security_Cheat_Sheet.html)
- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [OWASP HTTP Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)
</details>

<details>
<summary>What steps can organizations take to ensure data privacy?</summary>

### What steps can organizations take to ensure data privacy?

**Definition:**

**User privacy protection**, in OWASP's sheet, is the set of mitigations that keep identities, private information, and communications from being exposed by the service itself. The steps are cryptographic, transport, session, and transparency controls, not a promise that the network is anonymous.

**How it works:**

Encrypt private data in transit and at rest. Hash passwords with a slow hash. Send HSTS so browsers refuse to fall back to cleartext HTTP. Let users review sessions and invalidate a lost or stolen device. If third-party images or tags would leak the user's address, let users block that content. If the service cannot prevent disclosure of logs to an outside party, tell users in plain language.

Minimize collection, ask consent, and delete on a schedule, as the mobile sheet requires for PII. Log security events without the personal payload. Zero Trust's GDPR row adds privacy-by-design, access logging, and breach detection as the operational loop.

**Examples:**

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

**Sources:**

- [OWASP User Privacy Protection Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/User_Privacy_Protection_Cheat_Sheet.html)
- [OWASP Zero Trust Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Zero_Trust_Architecture_Cheat_Sheet.html)
</details>


### Advanced Web Security Concepts

<details>
<summary>What are security headers, and why are they important?</summary>

### What are security headers, and why are they important?

**Definition:**

**Security headers** are HTTP response headers that tell the browser how to handle the page. OWASP calls them an easy boost against cross-site scripting, clickjacking, and information disclosure. They apply to responses the browser will render; a JSON API that is never rendered gains little from framing or script policy.

**How it works:**

Set `Content-Security-Policy` to limit script and style sources, and prefer `frame-ancestors` over `X-Frame-Options`. If you still send `X-Frame-Options`, `DENY` refuses framing. Set `X-Content-Type-Options: nosniff` and a correct `Content-Type`, including `charset=UTF-8` for HTML. Set `Referrer-Policy: strict-origin-when-cross-origin` explicitly. Use `Cache-Control: no-store` for sensitive responses. `Strict-Transport-Security` belongs on HTTPS responses only.

Turn `X-XSS-Protection` off (`0`); the cheat sheet warns it can create flaws and says to rely on CSP instead. Do not send `Expect-CT` or `Public-Key-Pins`. Remove `X-Powered-By` and non-informative `Server` values, knowing fingerprinting still exists. `Cross-Origin-Opener-Policy: same-origin`, `Cross-Origin-Embedder-Policy: require-corp`, and `Cross-Origin-Resource-Policy: same-site` isolate browsing contexts. `Permissions-Policy` can disable camera, microphone, and geolocation when the site does not need them. If you send `Access-Control-Allow-Origin`, name a specific origin instead of `*` unless the API is intentionally public.

**Examples:**

```http
Content-Security-Policy: default-src 'self'
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
X-Frame-Options: DENY
```

**Sources:**

- [OWASP HTTP Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)
</details>

<details>
<summary>What is Content Security Policy (CSP) and how does it improve web security?</summary>

### What is Content Security Policy (CSP) and how does it improve web security?

**Definition:**

**Content Security Policy** is the `Content-Security-Policy` response header (or a `<meta http-equiv>` element) that lists which origins and script endpoints a page may load. MDN states that this helps guard against cross-site scripting. Policies are directives separated by semicolons. Fetch directives such as `default-src`, `script-src`, and `style-src` name allowed sources; `default-src` is the fallback.

**How it works:**

If `script-src` or `default-src` is present and does not include `'unsafe-inline'` or `'unsafe-eval'`, inline scripts and string evaluation are blocked. Prefer nonces or hashes (`sha256`, `sha384`, `sha512`) for the few inline scripts you must keep. `'strict-dynamic'` extends trust from a nonce or hash to scripts that trusted script adds. `frame-ancestors` lists who may embed the page. `object-src 'none'` blocks plugins. `upgrade-insecure-requests` rewrites HTTP URLs to HTTPS.

Multiple policies only tighten: if one policy sets `connect-src 'none'`, a second policy cannot reopen it. While testing, `Content-Security-Policy-Report-Only` reports violations without blocking. OWASP says to turn off `X-XSS-Protection` and use CSP instead. A report-only policy does not block execution.

**Examples:**

```http
Content-Security-Policy: default-src https:; object-src 'none'
```

**Sources:**

- [MDN — Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy)
- [OWASP HTTP Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)
</details>

<details>
<summary>Can you explain the role of Subresource Integrity (SRI) in web security?</summary>

### Can you explain the role of Subresource Integrity (SRI) in web security?

**Definition:**

**Subresource Integrity** lets the browser check that a fetched script or stylesheet matches a cryptographic hash you supply. MDN describes it as a defense when a third-party host, such as a CDN, is changed and would otherwise serve different bytes. It applies to `<script>` and to `<link>` whose `rel` is `stylesheet`, `preload`, or `modulepreload`.

**How it works:**

The `integrity` value is a whitespace-separated list of hashes prefixed with `sha256-`, `sha384-`, or `sha512-`. The browser selects the strongest algorithm present, hashes the bytes, and loads the resource only on a match; otherwise it fails the load with a network error. Several hashes of the same algorithm allow alternate legitimate files.

Cross-origin SRI requires CORS, so the tag needs `crossorigin="anonymous"` and the host must send `Access-Control-Allow-Origin`. Without `crossorigin`, the request is `no-cors` and browsers refuse to apply integrity. `Integrity-Policy: blocked-destinations=(script)` can require integrity metadata. Generate the hash from the file you intend to serve, for example with OpenSSL `dgst -sha384 -binary` piped to `base64 -A`.

**Examples:**

```bash
cat FILENAME.js | openssl dgst -sha384 -binary | openssl base64 -A
```

**Sources:**

- [MDN — Subresource Integrity](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Subresource_Integrity)
</details>

<details>
<summary>What is Domain Name System Security Extensions (DNSSEC), and how does it work?</summary>

### What is Domain Name System Security Extensions (DNSSEC), and how does it work?

**Definition:**

**DNSSEC** (RFC 4033) adds data origin authentication and data integrity to the Domain Name System. The specification introduces the extensions, their capabilities, and their limitations. It states that these extensions do not provide confidentiality.

**How it works:**

Resolvers can authenticate that DNS data came from the zone that is authoritative for the name and that the data was not altered, including authenticated denial when a name or type does not exist. Operators publish the signed records and a chain of trust back to a trust anchor. Clients and resolvers that validate reject data that does not authenticate.

Because DNSSEC does not encrypt queries or answers, it does not hide which names were looked up. Confidentiality of web traffic remains the job of TLS. DNSSEC is still worth deploying so a resolver can detect unauthorized changes to the records that point users at your site.

**Examples:**

```text
# DNSSEC (RFC 4033): origin authentication and integrity.
# It does not provide confidentiality; use TLS for that.
```

**Sources:**

- [RFC 4033 — DNS Security Introduction and Requirements](https://www.rfc-editor.org/rfc/rfc4033.html)
</details>

<details>
<summary>What is HTTP Strict Transport Security (HSTS) and how can it be used?</summary>

### What is HTTP Strict Transport Security (HSTS) and how can it be used?

**Definition:**

**HTTP Strict Transport Security** is the `Strict-Transport-Security` response header. MDN says it tells browsers that the host should be contacted only with HTTPS, and that later HTTP attempts are upgraded. On later connections the browser will not let the user bypass certificate errors. The host is identified by domain name only, not by IP address. Browsers ignore the header if it arrives over cleartext HTTP.

**How it works:**

`max-age` is seconds. `includeSubDomains` covers subdomains of that host, not the parent. `preload` requires `max-age` of at least `31536000` (one year) and `includeSubDomains`. OWASP's header sheet shows `max-age=63072000; includeSubDomains; preload` and warns that a long policy plus a broken certificate can lock users out until the age expires. Send the header only on HTTPS. A first HTTP visit can still be redirected; MDN's example uses `301` with a `Location` of the HTTPS URL, and that redirect itself must not include HSTS. Preload lists close the gap before the first HTTPS response, and they are operational, not part of the HSTS specification. `max-age=0` clears the policy, but only over HTTPS.

**When to use:**

Enable HSTS on HTTPS responses after the whole host, including subdomains if you set that directive, serves valid certificates. Use a short `max-age` first. Add `preload` only when you can meet the one-year minimum and accept that clients will keep the policy.

**Examples:**

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

**Sources:**

- [MDN — Strict-Transport-Security](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Strict-Transport-Security)
- [OWASP HTTP Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)
</details>


### Security Testing and Audits

<details>
<summary>What is penetration testing, and how does it differ from vulnerability scanning?</summary>

### What is penetration testing, and how does it differ from vulnerability scanning?

**Definition:**

OWASP's virtual-patching guidance separates two proactive checks on a live application: **penetration tests**, performed by people, and **automated web assessment tools**. The authorization sheet adds that unit and integration tests catch low-hanging access-control mistakes and are not a substitute for a dedicated security test or penetration test. Automated tests miss more sophisticated issues.

**How it works:**

Scanning repeatedly applies known checks and produces a list of findings. A penetration test is an ethical assessment of the running application, including business logic and chained weaknesses that a scanner does not model. Both are ways to learn about a flaw before an incident. Record the finding, judge whether a virtual patch applies, and still fix the code. Run scanners in CI for the known classes; schedule human testing when the threat model or a major release says the automated result is not enough.

Do not treat a clean scan as proof the authorization logic is sound. The authorization sheet points at the OWASP Web Security Testing Guide for access-control testing beyond unit tests.

**When to use:**

Run automated assessment continuously. Commission a penetration test for a new or high-impact system, after major design changes, and when a scanner cannot see business rules. Use the results to prioritize fixes, not as a one-time certificate.

**Examples:**

```text
# Automated scan: known checks in CI.
# Penetration test: people exercise the running app, then you still patch the code.
```

**Sources:**

- [OWASP Virtual Patching Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Virtual_Patching_Cheat_Sheet.html)
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
</details>

<details>
<summary>How is fuzzing used in security testing?</summary>

### How is fuzzing used in security testing?

**Definition:**

OWASP's logging guidance says to confirm that logging stays enabled during application security, **fuzz**, penetration, and performance testing. In that setting, fuzzing is an input-generation test that throws varied data at the application while you watch for crashes, error leaks, and log injection. The input-validation sheet tells you what those runs must include: valid values, invalid values, and near-matches, with length limits applied before matching, and a timeout treated as validation failure.

**How it works:**

Bound request size first so the test does not become an availability problem of its own. Point fuzzing at parsers, regular expressions, file uploads, and query parameters. Fail closed on malformed input. Assert that logs record the failure and metadata, not secrets or the full body. The logging sheet also says to test that the logging path itself is not subject to injection and that losing the network does not have an unwanted side effect if logs are shipped remotely.

Fuzzing finds broken handling. It does not replace parameterized queries or an authorization matrix.

**Examples:**

```text
# Exercise valid, invalid, and near-matching values.
# A matcher timeout is a validation failure, not a pass.
```

**Sources:**

- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [OWASP Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
</details>

<details>
<summary>What are the main objectives of a security audit?</summary>

### What are the main objectives of a security audit?

**Definition:**

A **security audit** checks whether the controls you claim are actually in place and whether their logs can support that claim. OWASP's logging sheet lists compliance monitoring, audit trails of data addition, change, deletion, and export, and later requests for information as reasons application logs exist. The secure-code-review sheet adds a human pass over architecture, entry points, authentication, authorization, data flow, cryptography, error handling, and deployment configuration.

**How it works:**

Objectives that those pages support:

- Confirm server-side authorization, deny-by-default, and safe failure.
- Confirm passwords are slow-hashed, sessions expire on the server, and cookies use `HttpOnly`, `Secure`, and `SameSite`.
- Confirm TLS, security headers, and dependency updates.
- Confirm security events are logged in a consistent format, clocks agree, and sensitive values are excluded.
- Confirm third-party components are known and reviewed.

An audit is not only a scanner report. Automated findings guide the manual review; people still judge business rules and whether a control can be measured.

**Examples:**

```text
# Audit trail events: add, change, delete, export.
# Exclude passwords, tokens, and full request bodies.
```

**Sources:**

- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [OWASP Secure Code Review Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Code_Review_Cheat_Sheet.html)
</details>

<details>
<summary>Can you explain the concept of threat modeling?</summary>

### Can you explain the concept of threat modeling?

**Definition:**

**Threat modeling** is a structured, repeatable process that models a system from a security perspective, identifies applicable threats, and decides a response. OWASP says to do it early, in design, and to keep the model updated with the system rather than once. The Threat Modeling Manifesto questions are: What are we working on? What can go wrong? What are we going to do about it? Did we do a good enough job?

**How it works:**

Decompose the system with data-flow diagrams that show trust boundaries, flows, stores, processes, and external entities. Identify threats with a method that fits the question. STRIDE is the usual prompt for a technical design: spoofing against authentication, tampering against integrity, repudiation against accounting, information disclosure against confidentiality, denial of service against availability, and elevation of privilege against authorization. Other methods (attack trees, abuse cases, LINDDUN, PASTA, OCTAVE, VAST) are for privacy, business impact, or organization-wide risk; do not pile them on for appearance.

Each threat gets a response: mitigate, eliminate, transfer, or accept. Transfer does not by itself reduce likelihood or harm; record who still owns the controls. Review the model with stakeholders, not only developers, and turn mitigations into requirements you can test. Cloud models must include shared responsibility, IAM, managed services, and ephemeral infrastructure.

**Examples:**

```text
# Four questions: what is it, what can go wrong, what will we do, did we do enough?
# STRIDE on each trust boundary, then a recorded response per threat.
```

**Sources:**

- [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)
</details>

<details>
<summary>What is static code analysis and how it is beneficial in identifying security risks?</summary>

### What is static code analysis and how it is beneficial in identifying security risks?

**Definition:**

**Static analysis** (SAST) examines source or built artifacts without running the application. OWASP's secure-code-review sheet says automated tools complement manual review: they highlight likely issues, dependency risk, and anti-patterns, while people judge business logic, authorization, and cryptographic misuse. Docker's CI guidance lists static analysis next to lint and image scanning as a pipeline step.

**How it works:**

Run SAST before the human review so obvious findings are already visible. People filter false positives and spend time on gaps tools miss: workflow bypass, race conditions, and whether a role check covers the specific object. Focus the tools on query construction, file paths, `eval` and similar sinks, deserialization, and hardcoded secrets.

Static analysis does not see a live configuration or a missing firewall rule. Pair it with dependency review, tests, and a diff review on every change. Keep the rules current with the language, including `no-eval` for JavaScript.

**Examples:**

```text
# Pipeline order from the review sheet: scan, then manual review of what tools cannot judge.
# JavaScript: no-eval, no-implied-eval, no-new-func
```

**Sources:**

- [OWASP Secure Code Review Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Code_Review_Cheat_Sheet.html)
- [OWASP Docker Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html)
</details>


### Compliance and Standards

<details>
<summary>What are some common web security compliance standards?</summary>

### What are some common web security compliance standards?

**Definition:**

The OWASP pages fetched for this note name several standards that constrain web controls, without replacing the standard's own text. **ISO/IEC 27001** appears as information-security management and risk-based access decisions. **PCI DSS** appears as network segmentation, encrypted communications, access monitoring, and a ban on TLS 1.0 and 1.1. **HIPAA** appears as granular access control, encryption, and audit trails. **GDPR** appears as privacy-by-design, access logging, and breach detection. **FIPS 140** appears when cryptographic modules or password hashes such as PBKDF2 must be validated. **SOC 2** appears next to access control, monitoring, and audit logging.

**How it works:**

Use the standard that matches the data. Cardholder data pulls in PCI DSS and the TLS baseline (TLS 1.3, TLS 1.2 only if you still need it, no SSL). Health data pulls in the HIPAA row: tight access, encryption, trails. Personal data of people in the European Union pulls in the GDPR row and, often, MFA. An information-security management system pulls in ISO 27001's risk-based decisions. Ask SaaS vendors for attestation evidence, which the cloud sheet specifically ties to ISO 27001.

Meeting a named control in a cheat-sheet table is not a certification. Map each obligation to an owner, a technical control, and a log you can show.

**Examples:**

```text
# TLS baseline cited with PCI DSS: disable TLS 1.0, TLS 1.1, SSLv2, and SSLv3.
# Default to TLS 1.3; keep TLS 1.2 only when compatibility requires it.
```

**Sources:**

- [OWASP Zero Trust Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Zero_Trust_Architecture_Cheat_Sheet.html)
- [OWASP Transport Layer Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html)
- [OWASP Secure Cloud Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Cloud_Architecture_Cheat_Sheet.html)
</details>

<details>
<summary>Explain the purpose and key aspects of ISO/IEC 27001.</summary>

### Explain the purpose and key aspects of ISO/IEC 27001.

**Definition:**

**ISO/IEC 27001**, as OWASP's Zero Trust sheet summarizes it, is the standard associated with information security management and risk-based access decisions. The secure-cloud sheet says that when you assess a SaaS provider you should ask for attestation records and proof of compliance to standards like ISO 27001.

**How it works:**

In practice that means an information-security management system: decide risk, choose controls, operate them, and keep evidence. The Zero Trust table pairs the standard with risk-based access rather than with a fixed network perimeter. Decisions consider who is asking, the device, and the resource, and they are logged.

For a web system, the evidence is the same material other OWASP sheets already require: threat models, access reviews, encryption in transit and at rest, security logging, and vendor attestations. ISO 27001 does not replace those engineering controls; it is the management frame that shows they were chosen and checked.

**Examples:**

```text
# Ask a SaaS vendor for ISO 27001 attestation evidence.
# Pair it with risk-based access decisions and audit logs.
```

**Sources:**

- [OWASP Zero Trust Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Zero_Trust_Architecture_Cheat_Sheet.html)
- [OWASP Secure Cloud Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Cloud_Architecture_Cheat_Sheet.html)
</details>

<details>
<summary>What is PCI DSS and its relevance to web security?</summary>

### What is PCI DSS and its relevance to web security?

**Definition:**

**PCI DSS** is the payment-card security standard. OWASP's Zero Trust table associates it with network segmentation, encrypted communications, and access monitoring. The TLS cheat sheet says PCI DSS forbids TLS 1.0 and TLS 1.1. The cryptographic-storage sheet says card data is a prime candidate for not being stored at all, and that PCI DSS tightly constrains storage when you must keep it. FIPS-style limits on algorithms can also apply.

**How it works:**

For a web checkout: terminate TLS at 1.3 (1.2 only for a documented compatibility need), disable SSL and the deprecated TLS versions, segment the card environment from the rest of the network, encrypt what you truly must store, and log access without storing the card number in the log. Prefer a payment provider so the application never sees the full card. Database accounts that touch payment data get least privilege.

A scanner stamp is not the whole obligation. The virtual-patching and authorization sheets still require the application to authorize each operation and to parameterize queries.

**Examples:**

```text
# PCI DSS, via the TLS cheat sheet: disable TLS 1.0 and TLS 1.1.
# Do not store card data if a payment provider can hold it instead.
```

**Sources:**

- [OWASP Zero Trust Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Zero_Trust_Architecture_Cheat_Sheet.html)
- [OWASP Transport Layer Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html)
- [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
</details>

<details>
<summary>Can you discuss the importance of HIPAA compliance in web applications?</summary>

### Can you discuss the importance of HIPAA compliance in web applications?

**Definition:**

**HIPAA**, in OWASP's Zero Trust summary, lines up with granular access controls, data encryption, and audit trails for health-related data. The multifactor sheet adds that healthcare regulations commonly require MFA. A web application that handles that data has to implement those three controls in the application, not only in a policy document.

**How it works:**

Authorize each record on the server, not only by a coarse role: a clinician's session must not read every patient's file merely because the id is known. Encrypt in transit with the TLS baseline and at rest with AES-GCM or an equivalent authenticated mode, keys in a KMS or HSM. Write an audit trail of access, change, and export, and keep the health data itself out of the log line.

MFA belongs on the accounts that can open those records, and again on sensitive changes. Session idle and absolute timeouts limit how long a browser left open remains a door. HIPAA compliance is the obligation to show these controls; the engineering is the same least privilege, encryption, and logging OWASP already describes.

**Examples:**

```text
# HIPAA row: granular access, encryption, audit trails, plus MFA for those accounts.
```

**Sources:**

- [OWASP Zero Trust Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Zero_Trust_Architecture_Cheat_Sheet.html)
- [OWASP Multifactor Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html)
</details>


### Emerging Threats and Trends

<details>
<summary>How do you stay informed about the latest web security threats?</summary>

### How do you stay informed about the latest web security threats?

**Definition:**

OWASP's virtual-patching preparation step is to subscribe to vendor alert lists for the commercial software you run, so a disclosure arrives with patching data instead of as a surprise. The .NET sheet says to watch the security-announcement repositories for the platform. The vulnerable-dependency sheet says to analyze third-party components from the start of the project, not at the end.

**How it works:**

Keep an inventory of applications, libraries, container images, and the operating systems under the pipeline. Turn on dependency and image scanning in CI. Read vendor lists and the feeds those tools use. When a notice arrives, record the identifier, the affected versions, and whether your configuration can hit the issue, then patch or apply a temporary virtual patch while the code fix is prepared.

Threat models and the logging trail show which notices matter to this system. A newsletter with no inventory does not.

**Examples:**

```text
# Watch platform announcement repos and vendor lists.
# Scan dependencies in CI from the first commit, not the week before release.
```

**Sources:**

- [OWASP Virtual Patching Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Virtual_Patching_Cheat_Sheet.html)
- [OWASP Vulnerable Dependency Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Vulnerable_Dependency_Management_Cheat_Sheet.html)
- [OWASP DotNet Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DotNet_Security_Cheat_Sheet.html)
</details>

<details>
<summary>Can you discuss the implications of quantum computing on web security?</summary>

### Can you discuss the implications of quantum computing on web security?

**Definition:**

OWASP's post-quantum sheet says a sufficiently powerful quantum computer could break RSA, Diffie-Hellman, and elliptic-curve cryptography, which affects public-key encryption, key exchange, and digital signatures. AES is treated differently: prefer AES-256 for margin, while NIST's FAQ cited there still permits AES-128, AES-192, and AES-256. A hybrid TLS key exchange does not make classical certificate signatures quantum-resistant (RFC 9794).

**How it works:**

Inventory every use: transport, stored keys, signatures, HSMs, and backups. Prioritize data that must stay confidential for years and verifiers that are hard to update, because collecting ciphertext now can matter later. Adopt supported implementations, not a home-built combination: ML-KEM (FIPS 203) for key establishment and a separate signature migration such as ML-DSA (FIPS 204). Pilot a hybrid exchange such as X25519MLKEM768 and keep certificate and hostname checks on. Test real clients, proxies, and size limits. Roll out gradually, record negotiated algorithms without logging secrets, and give every classical-only exception an owner and an end date.

Start the inventory even when a dependency cannot migrate yet.

**Examples:**

```text
# Inventory purpose, algorithm, library, and owner. Store metadata, never key material.
# Prefer AES-256 for stored data; plan ML-KEM for key establishment.
```

**Sources:**

- [OWASP Post-Quantum Cryptography Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Post_Quantum_Cryptography_Cheat_Sheet.html)
</details>

<details>
<summary>What role does artificial intelligence play in web security?</summary>

### What role does artificial intelligence play in web security?

**Definition:**

OWASP's AI-agent sheet describes agents that reason, plan, call tools, keep memory, and take actions. That capability adds risks beyond a single prompt: tool abuse, data leaving through a tool call, memory that affects later sessions, actions without a person, and unbounded loops that run up cost. The defensive role of these controls is to keep an agent inside least privilege.

**How it works:**

Give the agent only the tools that task needs, with read versus write scope. Separate tool sets for internal and user-facing agents. Require an explicit authorization step before a sensitive or irreversible action. Validate tool arguments on the server the same way you validate any other request: schema, parameterized queries, and an authorization check that does not trust the model's claim.

Keep secrets and personal data out of the prompt, the memory store, and the logs. Cap iterations and spend. Treat documents and web pages the agent reads as untrusted input. The JavaScript sheet's rule still applies to any web UI in front of the agent: do not pass model output to `innerHTML`, `dangerouslySetInnerHTML`, `v-html`, or Angular's trust-bypass APIs.

**Examples:**

```text
# Least privilege: one tool, one scope. Authorize high-impact actions outside the model.
```

**Sources:**

- [OWASP AI Agent Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/AI_Agent_Security_Cheat_Sheet.html)
- [OWASP JavaScript and TypeScript Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/JavaScript_and_TypeScript_Security_Cheat_Sheet.html)
</details>

<details>
<summary>How can Internet of Things (IoT) devices create web security risks?</summary>

### How can Internet of Things (IoT) devices create web security risks?

**Definition:**

OWASP's Zero Trust sheet lists IoT devices with servers, databases, and user devices as resources that need their own controls. A device is not safe because it sits on an internal network. If a web application trusts that network location, a weak device becomes a path to the application.

**How it works:**

Treat every device session like any other client. Authenticate it, authorize the specific resource, and expire the session. Encrypt the connection regardless of whether the device is "inside." Monitor posture: a device that is not patched or no longer matches policy loses access. Do not reuse a VPN-style grant that opens the whole network once the device connects.

Web APIs that devices call need the same REST controls: HTTPS only, tokens with `iss`, `aud`, and `exp`, and rate limits. Store as little personal data from the device as the feature allows, and delete it on a schedule.

**Examples:**

```text
# Zero Trust: an IoT device is a resource. Network location is not authorization.
```

**Sources:**

- [OWASP Zero Trust Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Zero_Trust_Architecture_Cheat_Sheet.html)
</details>

<details>
<summary>What is Zero Trust security architecture?</summary>

### What is Zero Trust security architecture?

**Definition:**

**Zero Trust** means never trust by default, including callers already inside the network. OWASP's sheet takes the principles from NIST SP 800-207: every data source and service is a resource; communication is secured wherever it is; access to one resource does not grant another; policy is dynamic; asset posture is measured; authentication and authorization are enforced and rechecked; and telemetry improves later decisions, with privacy limits on that monitoring.

**How it works:**

Three roles cooperate. The policy engine decides. The policy administrator turns the decision into commands. Policy enforcement points — firewalls, proxies, application gateways, API gateways — allow or block. Enforcement is not only at the edge.

Prefer access to a specific application over a broad VPN. A WAF can filter requests as defense in depth and does not replace server-side authorization. Collect logs in a SIEM, exclude sensitive data, and revoke access when posture fails. The same sheet maps these controls onto ISO 27001, PCI DSS, HIPAA, and GDPR obligations.

**Examples:**

```text
# Never trust, always verify. One session authorizes one resource.
```

**Sources:**

- [OWASP Zero Trust Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Zero_Trust_Architecture_Cheat_Sheet.html)
</details>


### Incident Response and Handling

<details>
<summary>What is an incident response plan, and why is it critical?</summary>

### What is an incident response plan, and why is it critical?

**Definition:**

An **incident response plan** is the practiced sequence for what the organization does when a security event happens. OWASP's secure-product sheet lists a practiced plan as part of secure configuration, because it limits damage. The virtual-patching workflow copies the same phases used for IT incident response: preparation, identification, analysis, creating the mitigation, implementation and testing, then recovery and follow-up.

**How it works:**

Preparation is the part that cannot wait for the incident: know assets, have logging that redacts secrets, pre-authorize an emergency control such as a WAF rule, and name who decides. Identification is how you learn, from a vendor notice, a public disclosure, or a live event. Analysis records impact and affected versions. The mitigation might be a code fix, a key rotation, or a temporary virtual patch that is tested against real traffic before it blocks. Follow-up updates the ticket, the threat model, and the plan.

Without the plan, the first hours are spent choosing tools and owners. The product sheet treats that delay as part of the harm.

**Examples:**

```text
# Phases: prepare, identify, analyze, mitigate, implement and test, recover and follow up.
```

**Sources:**

- [OWASP Secure Product Design Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Product_Design_Cheat_Sheet.html)
- [OWASP Virtual Patching Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Virtual_Patching_Cheat_Sheet.html)
</details>

<details>
<summary>Explain the steps you would take after discovering a data breach.</summary>

### Explain the steps you would take after discovering a data breach.

**Definition:**

After a suspected breach, OWASP's materials point at containment that does not destroy evidence, then a durable fix. The secrets sheet's incident note, echoed by the webhook guidance, is to revoke a compromised secret immediately and replace it; accepting it during an overlap still allows forged use. Cryptographic-storage guidance says to rotate keys when compromise is known or suspected, including when someone who could use the key leaves.

**How it works:**

Preserve logs: copy them to a store the affected system cannot rewrite, which the segmentation sheet illustrates with append-only syslog. Record who accessed what, without collecting more personal data than the investigation needs. Invalidate sessions server-side and denylist stolen token identifiers until they expire. Force credential reset where passwords may be in the taken data, and re-hash with the current algorithm on the next login. Tell users what you cannot protect, which the privacy sheet requires when disclosure is real.

Patch the path that was used, or put a tested virtual patch in front while the code change is built. Update the threat model and the incident plan with what the timeline showed. GDPR-oriented monitoring in the Zero Trust sheet exists so this detection is not optional for personal data.

**Examples:**

```text
# Revoke the secret now. Overlap is for planned rotation, not for a known compromise.
```

**Sources:**

- [OWASP Webhook Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Webhook_Security_Cheat_Sheet.html)
- [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- [OWASP Network Segmentation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Network_Segmentation_Cheat_Sheet.html)
</details>

<details>
<summary>How important is user training in preventing web security incidents?</summary>

### How important is user training in preventing web security incidents?

**Definition:**

OWASP's threat-modeling sheet says many developers lack security experience, so they miss threats or rank them poorly, and that the remedy is regular training tailored to the team plus security people in the modeling sessions. The CI/CD sheet says education is required before a tool is used to deploy code: secure configuration does not appear by itself.

**How it works:**

Train people on the failures those sheets name: passwords and MFA, not pasting tokens into tickets, reporting a suspected phish, and not bypassing a certificate warning. Train developers on parameterized queries, server-side authorization, and secret storage. Invite security into threat-modeling sessions so the training is about this system.

The mobile sheet notes that security which people cannot use gets bypassed. Training works together with usable controls, not instead of them. A practiced incident plan tells everyone the first call to make.

**Examples:**

```text
# Training topic: report the event, do not "click through" a certificate warning.
```

**Sources:**

- [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)
- [OWASP CI CD Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/CI_CD_Security_Cheat_Sheet.html)
</details>

<details>
<summary>Can you discuss the role of a Security Operations Center (SOC)?</summary>

### Can you discuss the role of a Security Operations Center (SOC)?

**Definition:**

OWASP does not define a SOC org chart in the pages used here. It does define the work those teams perform: central logging, a SIEM, and response. The logging sheet says application logs should be consistent so a SIEM or SEM can correlate them. The authorization sheet says to consider sending access-control logs to a centralized log server or SIEM. Zero Trust says to invest in SIEM and analytics or you cannot detect, tune policy, or show compliance.

**How it works:**

The operational role is to collect authentication failures, authorization denials, and other security events; exclude passwords and full bodies; keep clocks aligned; alert on spikes such as webhook signature failures; and start the incident plan. Detection systems (NIDS, HIDS, WAF) are inputs, not a replacement for application logs.

Automated blocking, which Zero Trust lists next to the SIEM, still needs a tested rule and a person who can roll it back. Threat hunting looks for activity the first alert missed. The trail is also what an audit and a breach investigation read later.

**Examples:**

```text
# Ship application security events to a SIEM. Keep secrets out of the event.
```

**Sources:**

- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [OWASP Zero Trust Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Zero_Trust_Architecture_Cheat_Sheet.html)
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
</details>


### Cloud Security

<details>
<summary>How does web security change when applications are moved to the cloud?</summary>

### How does web security change when applications are moved to the cloud?

**Definition:**

Moving to the cloud changes who operates each layer and adds shared, elastic infrastructure. OWASP's threat-modeling sheet says cloud threat models must include the shared-responsibility split, IAM, managed services, multi-tenant isolation, and infrastructure that appears and disappears. The secure-cloud sheet says to spend assessment effort on threats, likelihood, attack surface, and business impact before buying controls.

**How it works:**

Identity becomes the main boundary. Use short-lived workload credentials instead of keys in images. Segment front end, application, and data. Put a WAF on the load balancer or API gateway as a first filter, and keep authorization in the application. Encrypt with the provider's KMS where the threat model says the application must not hold the key itself. Log to a store the application cannot rewrite.

You still write the same application controls: parameterized queries, session timeouts, security headers, and dependency updates. The provider does not do those for an application you deploy.

**Examples:**

```text
# Cloud change: shared responsibility, IAM, and ephemeral infrastructure belong in the threat model.
```

**Sources:**

- [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)
- [OWASP Secure Cloud Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Cloud_Architecture_Cheat_Sheet.html)
</details>

<details>
<summary>What are the shared responsibility models in cloud security?</summary>

### What are the shared responsibility models in cloud security?

**Definition:**

The **shared responsibility model** in OWASP's cloud sheet splits duties between the cloud provider and the customer by how much of the stack the provider operates. Physical machines and the data center stay with the provider. Depending on the service, the customer may own everything from the operating system upward, or only configuration and code.

**How it works:**

IaaS: the provider maintains infrastructure; the customer maintains authentication and authorization, data storage and access, some networking such as ports and network ACLs, and the application. PaaS: the customer still controls application authentication and authorization, application software, and external data storage, while the platform hosts the code. SaaS: the provider runs the stack; the customer configures the product, designates administrators, and connects other products. Ask SaaS vendors for attestation such as ISO 27001.

Write down which team patches the OS, rotates keys, and reviews IAM. A control with no owner is the customer's outage even when the provider runs the hardware.

**When to use:**

Choose IaaS when you must control the operating system and can staff it. Choose PaaS when the team should run application code and data access without owning the host. Choose SaaS when the product already exists and you can accept the vendor's stack in exchange for their attestation.

**Examples:**

```text
# IaaS: you own the OS upward. PaaS: you own the app and its data access. SaaS: you own configuration and admins.
```

**Sources:**

- [OWASP Secure Cloud Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Cloud_Architecture_Cheat_Sheet.html)
</details>

<details>
<summary>Explain how encryption is handled in the cloud.</summary>

### Explain how encryption is handled in the cloud.

**Definition:**

Cloud encryption uses the same algorithms as anywhere else, with provider key services as the preferred store. OWASP's cryptographic-storage sheet lists a physical or virtual HSM, Amazon KMS, Azure Key Vault, and external secrets managers. These give central management in container fleets, rotation, secure generation, and a harder path to export the key. The cloud architecture sheet says object access should use short-lived workload identity, not a long-lived key embedded in the application.

**How it works:**

Pick the layer from the threat model. Provider disk encryption helps against physical loss of a volume. It does not help if the attacker runs as the application. Application-level AES-GCM with a data key wrapped by a KMS key separates the people who can read ciphertext from the people who can use the key. Keep the wrapped data key with the data only if the wrapping key stays in KMS.

Turn on the provider's encryption for backups and snapshots as part of the same rotation plan. Do not put key material in images, environment variables, or source control. Passwords remain slow hashes, not KMS ciphertext, unless an external system truly requires the original password.

**Examples:**

```text
# Store the key in the provider KMS. The application asks to use the key; it does not embed it.
```

**Sources:**

- [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- [OWASP Secure Cloud Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Cloud_Architecture_Cheat_Sheet.html)
</details>

<details>
<summary>What special considerations are there for web security with cloud storage services?</summary>

### What special considerations are there for web security with cloud storage services?

**Definition:**

OWASP's cloud sheet gives object storage three access modes: IAM policies, cryptographically signed URLs, and public objects. The secure default for application data is IAM through a workload identity with short-lived credentials and permissions limited to the operations and resources required. Public storage exposes the object to anyone who has the URL. The authorization sheet calls out poorly secured cloud buckets as a standing problem and says static files belong in the same access policy as other resources.

**How it works:**

Do not ship a long-lived access key in the image or the repository. Separate keys from data: ciphertext in the bucket, wrapping keys in KMS. Classify objects so logs and exports are not world-readable. Turn off public listing. Sign URLs only for a specific object and a short lifetime when a browser must fetch directly. Log access. Apply the file-upload rules if users can write objects: allow-listed types, size limits, and names the server chooses.

A bucket policy is not a substitute for checking, in the application, that this user may read this object.

**Examples:**

```text
# Workload identity with short-lived credentials. Do not embed a persistent access key.
```

**Sources:**

- [OWASP Secure Cloud Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Cloud_Architecture_Cheat_Sheet.html)
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
</details>

<details>
<summary>How does a Web Application Firewall (WAF) protect cloud-hosted web applications?</summary>

### How does a Web Application Firewall (WAF) protect cloud-hosted web applications?

**Definition:**

A **web application firewall** monitors or blocks common attack payloads, or allows only specific request types and patterns. OWASP's cloud sheet says to attach it to entry points such as load balancers or API gateways as a first line of defense. Provider rule sets (AWS Managed Rules, GCP WAF rules, Azure core rule sets) cover generic cases and will not cover every issue an application has, so custom rules should match this application's routes and fields. Zero Trust adds that a WAF, including rules such as OWASP CRS, is defense in depth and does not replace authorization or business rules in server-side code.

**How it works:**

The virtual-patching sheet defines the temporary use: an enforcement layer that blocks attempts it recognizes against a known vulnerability while the code is fixed. Start in log-only mode, test real user flows, then block. Positive rules that describe valid input are preferred to a pattern copied from one report. Log the rule id and outcome, and redact bodies so the WAF log is not a second copy of passwords.

A WAF sees only traffic that passes through it. Direct paths to the origin bypass it. Keep parameterized queries and server-side checks anyway.

**Examples:**

```text
# Attach the WAF to the load balancer or API gateway.
# Log only, then block. Authorization stays in application code.
```

**Sources:**

- [OWASP Secure Cloud Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Cloud_Architecture_Cheat_Sheet.html)
- [OWASP Zero Trust Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Zero_Trust_Architecture_Cheat_Sheet.html)
- [OWASP Virtual Patching Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Virtual_Patching_Cheat_Sheet.html)
</details>


### Security in Web Application Frameworks

<details>
<summary>What built-in security features do modern web application frameworks typically include?</summary>

### What built-in security features do modern web application frameworks typically include?

**Definition:**

Modern web frameworks ship controls that are secure when left on and easy to turn off. The OWASP sheets for Rails, Django, and ASP.NET Core describe the same set: HTTPS redirection, CSRF protection on cookie-authenticated state changes, session and CSRF cookies marked secure, output encoding in templates, parameterized data access, and response headers such as HSTS and a content security policy.

**How it works:**

Turn the deployment checks on and read the warnings. Django's `check --deploy` reports an unset `SECURE_HSTS_SECONDS`, `SECURE_SSL_REDIRECT` not true, a weak `SECRET_KEY`, `SESSION_COOKIE_SECURE` and `CSRF_COOKIE_SECURE` not true, `DEBUG` still true, and an empty `ALLOWED_HOSTS`. Rails enables `protect_from_forgery` on the base controller and `config.force_ssl` for TLS, HSTS, and secure cookies. ASP.NET Core uses `UseHttpsRedirection`, HSTS, `HttpOnly` cookies, and anti-forgery validation.

Exceptions to CSRF, a debug page, or an empty host list undo those defaults. Review every exception. Repeat authorization on the server even when the framework has a login decorator.

**Examples:**

```bash
# Django deployment check. Fix every warning before production.
./manage.py check --deploy
```

**Sources:**

- [OWASP Django Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Django_Security_Cheat_Sheet.html)
- [OWASP Ruby on Rails Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Ruby_on_Rails_Cheat_Sheet.html)
- [OWASP .NET Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DotNet_Security_Cheat_Sheet.html)
</details>

<details>
<summary>How does Ruby on Rails handle web security, and what are its built-in protection mechanisms?</summary>

### How does Ruby on Rails handle web security, and what are its built-in protection mechanisms?

**Definition:**

Ruby on Rails includes CSRF tokens and forces TLS when production is configured that way. OWASP's Rails sheet says to keep `protect_from_forgery` on the base `ApplicationController`, and to set `config.force_ssl = true` so the app is served over SSL, sends Strict-Transport-Security, and uses secure cookies.

**How it works:**

CSRF exceptions exist and must be reviewed. The sheet notes that Rails does not apply CSRF protection to HTTP `GET` by default, so state-changing work should not be a GET. Token-only APIs do not need this controller protection; cookie-authenticated paths still do.

SQL values go through placeholders. A `LIKE` pattern is escaped with `sanitize_sql_like` and still bound as a parameter. Mass assignment is the Rails name for binding a request object straight onto a model; allow only the attributes the action may set.

**When to use:**

Keep the defaults on every cookie-authenticated controller. Add a `protect_from_forgery` exception only for a path that does not use cookies, and record why.

**Examples:**

```ruby
# config/environments/production.rb
config.force_ssl = true

class ApplicationController < ActionController::Base
  protect_from_forgery
end
```

**Sources:**

- [OWASP Ruby on Rails Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Ruby_on_Rails_Cheat_Sheet.html)
- [OWASP Mass Assignment Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Mass_Assignment_Cheat_Sheet.html)
</details>

<details>
<summary>What are the security features provided by Django for Python web applications?</summary>

### What are the security features provided by Django for Python web applications?

**Definition:**

Django aims to be secure by default and still lets a developer reconfigure a component unsafely. OWASP's Django sheet points at `manage.py check --deploy`, the CSRF middleware, secure session and CSRF cookies, and `@login_required` so an unauthenticated caller is redirected to the login page.

**How it works:**

Address every warning from `check --deploy`: set HSTS only when the whole site is HTTPS, set `SECURE_SSL_REDIRECT` or redirect at the proxy, use a long random `SECRET_KEY` that is not prefixed with `django-insecure-`, set `SESSION_COOKIE_SECURE` and `CSRF_COOKIE_SECURE`, turn `DEBUG` off, and set `ALLOWED_HOSTS`. Any custom cookie from `HttpResponse.set_cookie()` needs its secure parameter true. Hash passwords with `make_password` from `django.contrib.auth.hashers`, not with a reversible store.

`@login_required` checks that someone is signed in. It does not decide which object that person may read. Add that check in the view.

**Examples:**

```python
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
DEBUG = False
```

**Sources:**

- [OWASP Django Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Django_Security_Cheat_Sheet.html)
</details>

<details>
<summary>How does ASP.NET Core enforce web security?</summary>

### How does ASP.NET Core enforce web security?

**Definition:**

The OWASP .NET sheet configures ASP.NET Core to redirect to HTTPS, send HSTS, and set browser security headers. Cookie options set `HttpOnly` and an expiry. Anti-forgery validation is applied on handlers that must not run from another site.

**How it works:**

`app.UseHttpsRedirection()` sends browsers to HTTPS. `UseHsts` sets a max age; the sheet's example uses one year and `IncludeSubdomains`. NWebsec helpers in the same sheet set `X-Content-Type-Options`, `Referrer-Policy` of `NoReferrer`, `X-Frame-Options` deny, and a content security policy limited to `'self'` for scripts, styles, fonts, images, and form actions. `ConfigureApplicationCookie` sets `Cookie.HttpOnly = true` and `ExpireTimeSpan` of 60 minutes, with sliding expiration left off unless you have accepted that trade-off.

`[ValidateAntiForgeryToken]` checks the token, including on a GET handler when you apply it there. Remove `X-Powered-By` and the server version header so the response does not advertise the stack. These headers do not replace an authorization filter on the action.

**Examples:**

```csharp
app.UseHttpsRedirection();
app.UseHsts(hsts => hsts.MaxAge(365).IncludeSubdomains());
services.ConfigureApplicationCookie(options =>
{
    options.Cookie.HttpOnly = true;
    options.ExpireTimeSpan = TimeSpan.FromMinutes(60);
    options.SlidingExpiration = false;
});
```

**Sources:**

- [OWASP .NET Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DotNet_Security_Cheat_Sheet.html)
</details>

<details>
<summary>What security considerations must be taken into account when using client-side JavaScript frameworks like Angular, React, or Vue.js?</summary>

### What security considerations must be taken into account when using client-side JavaScript frameworks like Angular, React, or Vue.js?

**Definition:**

OWASP's cross-site scripting sheet says modern frameworks reduce XSS because templates auto-escape. Gaps remain at the escape hatches: React's `dangerouslySetInnerHTML` without a sanitizer, Angular's `bypassSecurityTrustAs*` functions, Lit's `unsafeHTML`, and Polymer's `inner-h-t-m-l` attribute. React also does not accept `javascript:` or `data:` URLs without extra validation.

**How it works:**

Keep data in text sinks. The DOM sheet's safe pattern is `textContent`, `insertAdjacentText`, `createTextNode`, and setting `value` on a form field. Use `innerHTML` only after a maintained sanitizer such as DOMPurify. The AJAX sheet says React, Vue, Angular, and Svelte standardize DOM updates and still require developers to avoid those unsafe APIs.

Know where your framework stops protecting you. Output encoding and HTML sanitization cover the gap. Keep framework packages current. A content security policy still limits which scripts the browser will run if a sink is misused.

**When to use:**

Use the framework's default template binding for text. Step outside it only to render HTML you have sanitized, and keep that path small and reviewed.

**Examples:**

```javascript
elem.textContent = untrusted;
```

**Sources:**

- [OWASP Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [OWASP DOM based XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html)
- [OWASP AJAX Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/AJAX_Security_Cheat_Sheet.html)
</details>


### Mobile Web Security

<details>
<summary>How do mobile web applications pose unique security challenges?</summary>

### How do mobile web applications pose unique security challenges?

**Definition:**

OWASP's mobile sheet says mobile development has security problems that web applications do not share in the same way. The app binary, the device store, backups, screenshots, and deep links sit on a device the developer does not control. A patch also reaches users late, because of store review and because people update on their own schedule.

**How it works:**

Do not trust the client. Authenticate and authorize on the server, and load sensitive data only after that succeeds. Do not store the user's password on the device; use a revocable token in platform storage (Keychain on iOS, Keystore on Android). Encrypt data at rest with platform APIs and hardware-backed keys when they exist. Assume the network can be read: use HTTPS, do not disable certificate checks, and consider pinning only after reading the pinning sheet's trade-offs.

Deep links, widgets, Shortcuts, and Siri can open a function while the device is locked. Require device authentication for those sensitive actions and still enforce the app session. Plan a forced update when a fix cannot wait for voluntary installs.

**Examples:**

```text
# Sensitive App Intents: require unlock. This is not a substitute for the server session.
authenticationPolicy = .requiresLocalDeviceAuthentication
```

**Sources:**

- [OWASP Mobile Application Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Mobile_Application_Security_Cheat_Sheet.html)
</details>

<details>
<summary>What steps can be taken to secure mobile APIs and services?</summary>

### What steps can be taken to secure mobile APIs and services?

**Definition:**

The mobile sheet's secure-API guidance is to communicate with backend services securely, use OAuth 2, JWT, or a similar mechanism, and rotate API keys and tokens. The same sheet says authentication and authorization decisions are made on the server.

**How it works:**

Use HTTPS for every call. Do not override certificate validation to accept an invalid certificate. Prefer certificates from a trusted CA and strong cipher suites. Do not hard-code credentials in the app. Store tokens in platform secure storage, handle expiry, and time out idle sessions. Offer remote logout. Require the user to authenticate again for a password change or a payment update.

An API key in the binary is not access control for sensitive data. The REST sheet says API keys can limit farming and still must not be the only control on high-value resources. Return `429 Too Many Requests` when calls arrive too quickly, and revoke a key that breaks the usage agreement.

**Examples:**

```http
Authorization: Bearer <access-token>
```

**Sources:**

- [OWASP Mobile Application Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Mobile_Application_Security_Cheat_Sheet.html)
- [OWASP REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
</details>

<details>
<summary>Discuss the security implications of third-party libraries in mobile web app development.</summary>

### Discuss the security implications of third-party libraries in mobile web app development.

**Definition:**

The mobile sheet treats third-party libraries as a supply-chain unknown. Its controls are to sign the app, use only trusted and validated components, control updates and releases, and watch those products for security incidents. The vulnerable-dependency sheet, which it links, is the follow-up when a notice arrives.

**How it works:**

Keep libraries current; the sheet lists that as its own testing item. Audit dependencies rather than waiting for a release week. Pin versions so a build does not silently move. Sign release artifacts so an update is the one you built.

A library runs with the app's permissions and can read what the app stores. Request only the device and backend permissions the feature needs. Do not add a component whose maintainer you cannot identify.

**Examples:**

```text
# Audit third-party components from the start of the project.
# Prefer a maintained library and pin the version you reviewed.
```

**Sources:**

- [OWASP Mobile Application Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Mobile_Application_Security_Cheat_Sheet.html)
- [OWASP Vulnerable Dependency Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Vulnerable_Dependency_Management_Cheat_Sheet.html)
</details>

<details>
<summary>What are common vulnerabilities found in mobile web applications?</summary>

### What are common vulnerabilities found in mobile web applications?

**Definition:**

The mobile sheet groups the failures it wants developers to close: trusting the client, storing passwords or hard-coded secrets on the device, sending data without HTTPS, turning off certificate checks, leaking data through logs, caches, and snapshots, and opening sensitive screens from a deep link or a lock-screen widget without an authentication check.

**How it works:**

Perform authentication and authorization on the server and repeat any client check there. Use platform secure storage. Do not put secrets in `plist` files or in Android `SharedPreferences`. Minimize PII, expire it, and ask consent. Mask sensitive fields on screen. Validate input and validate output.

Disable debugging on release builds, verify the app signature, and do not treat obfuscation as the access check. The sheet also says usable security matters: controls people cannot use get worked around. Give a fallback such as a PIN next to biometrics, and do not allow a 4-digit PIN.

**Examples:**

```text
# Do not store the account password on the device.
# Store a revocable token in Keychain or Keystore.
```

**Sources:**

- [OWASP Mobile Application Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Mobile_Application_Security_Cheat_Sheet.html)
</details>


### Secure Infrastructure

<details>
<summary>How does server hardening affect web security?</summary>

### How does server hardening affect web security?

**Definition:**

Hardening removes defaults that advertise or over-privilege the host. The TLS sheet says to disable SSL and TLS 1.0 and 1.1, default to TLS 1.3, keep TLS 1.2 only for compatibility, disable null, anonymous, and export ciphers, and patch the TLS library. The .NET sheet removes the server version header and `X-Powered-By`. The Docker sheet requires a `USER` directive and pinned base-image versions.

**How it works:**

After the configuration change, test it. The TLS sheet points at the OWASP Testing Guide chapter on SSL/TLS testing and at external checkers such as SSL Labs. Protect the private key with filesystem permissions. Certificates should hash with SHA-256, not MD5 or SHA-1, and the name belongs in `subjectAlternativeName`.

A hardened host still runs the application. Parameterized queries, authorization, and secure cookies remain application work. Network segmentation keeps that host from reaching zones it does not serve.

**Examples:**

```dockerfile
USER app
```

**Sources:**

- [OWASP Transport Layer Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html)
- [OWASP Docker Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html)
- [OWASP .NET Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DotNet_Security_Cheat_Sheet.html)
</details>

<details>
<summary>Explain the importance of patch management in maintaining web security.</summary>

### Explain the importance of patch management in maintaining web security.

**Definition:**

A missing patch leaves a known flaw in a library, a container image, or the TLS stack. The TLS sheet says SSL and TLS libraries must be kept up to date, citing historical library flaws such as Heartbleed. The mobile sheet adds that a released mobile patch still waits on store review and on the user to install it, so you need a way to require an update.

**How it works:**

Inventory dependencies from the start of the project and scan them in CI, which is the vulnerable-dependency and Docker guidance. Subscribe to vendor alerts so a notice arrives with patch data. When a fix is not in production yet, a virtual patch on a device that sees the traffic can block recognized attempts; it does not remove the flaw, and the code or image update still has to ship.

Record the identifier, the affected versions, and whether your configuration can hit the issue. Retire the temporary rule after the real fix is deployed.

**Examples:**

```text
# Scan the image in CI. Push it only when the scan is clean.
```

**Sources:**

- [OWASP Transport Layer Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html)
- [OWASP Vulnerable Dependency Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Vulnerable_Dependency_Management_Cheat_Sheet.html)
- [OWASP Virtual Patching Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Virtual_Patching_Cheat_Sheet.html)
</details>

<details>
<summary>What is network segmentation, and how can it improve security for web applications?</summary>

### What is network segmentation, and how can it improve security for web applications?

**Definition:**

OWASP's network-segmentation sheet describes a three-layer layout. The front-end zone holds the load balancer, an application-layer firewall, the web server, and the web cache. Middleware holds applications that talk to each other. The back-end zone holds databases, directory services, and log servers.

**How it works:**

An edge firewall can split a DMZ for Internet-facing services, protected by a WAF, from a segment that may call out but is not reachable from the Internet. Write the allowed paths: a front end reaches its application tier, and that tier reaches its own database. A front end does not reach another system's database.

Segmentation fails closed only while the firewall is the place a packet must cross. If many applications share one network and a balancer is the only door, the sheet says access control between those applications has moved to layer 7 on the balancer. Application authorization is still required on each request.

**Examples:**

```text
# FRONTEND: balancer, application-layer firewall, web server, cache.
# MIDDLEWARE: applications. BACKEND: databases, directory, logs.
```

**Sources:**

- [OWASP Network Segmentation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Network_Segmentation_Cheat_Sheet.html)
</details>

<details>
<summary>Describe how load balancers can be configured to improve web application security.</summary>

### Describe how load balancers can be configured to improve web application security.

**Definition:**

In the segmentation sheet the load balancer sits in the front-end zone with the application-layer firewall, the web server, and the cache. When several applications share one network, the sheet says you open one port to that network and balance, for example, on HTTP request parameters. The balancer is then the only incoming door.

**How it works:**

That design concentrates entry, which is useful, and it also means segmentation between those applications no longer happens in the firewall. The sheet says that control is then performed at layer 7 by the balancer. Put a WAF on that entry when the cloud sheet's guidance applies, and still authorize every request in application code.

Do not treat the balancer as the session or the permission system. REST endpoints remain stateless and check the caller. Health checks and management ports stay off the public interface.

**Examples:**

```text
# One incoming port to the network, then balance on HTTP request parameters.
# Authorization for each application still runs behind the balancer.
```

**Sources:**

- [OWASP Network Segmentation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Network_Segmentation_Cheat_Sheet.html)
- [OWASP Secure Cloud Architecture Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Cloud_Architecture_Cheat_Sheet.html)
</details>


### Cryptography in Web Security

<details>
<summary>What is the difference between symmetric and asymmetric encryption, and how are they used in web security?</summary>

### What is the difference between symmetric and asymmetric encryption, and how are they used in web security?

**Definition:**

OWASP's key-management sheet, citing NIST SP 800-57 Part 1, separates algorithms by how many keys they use. A symmetric algorithm uses the same secret key for an operation and its inverse, such as encryption and decryption. An asymmetric algorithm uses a key pair: a public key anyone may know, and a private key that stays with the owner. Knowledge of the public key does not reveal the private key.

**How it works:**

Symmetric keys provide confidentiality and also message authentication codes. The .NET and Java sheets show AES-GCM with a 256-bit key and a 12-byte nonce that must be unique for every encryption under that key. Asymmetric algorithms in the key-management sheet are used to compute digital signatures and to establish keying material. TLS uses both: the sheet's AEAD suites (AES-GCM or ChaCha20-Poly1305 on TLS 1.3) protect the records, and the certificate's public key lets the client authenticate the server.

Do not write your own cryptographic functions. The Java sheet says to prefer a maintained library or a cloud key service. Passwords are not encrypted with a reversible symmetric cipher; use a password hash.

**When to use:**

Use a symmetric AEAD cipher for bulk data once a secret key exists. Use the asymmetric key pair to sign, or to agree that secret key. Use a password hash for passwords.

**Examples:**

```csharp
// AES-GCM: 32-byte key, 12-byte nonce, a new nonce for every encryption.
var key = new byte[32];
var nonce = new byte[12];
```

**Sources:**

- [OWASP Key Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Key_Management_Cheat_Sheet.html)
- [OWASP .NET Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DotNet_Security_Cheat_Sheet.html)
- [OWASP Transport Layer Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html)
</details>

<details>
<summary>Explain the role of digital signatures in maintaining web security.</summary>

### Explain the role of digital signatures in maintaining web security.

**Definition:**

The key-management sheet says asymmetric algorithms are used to compute digital signatures, and that hash functions compress messages for signature generation and verification. It cites FIPS 186 for digital signature algorithms. A signature lets a verifier check integrity and origin with the public key. It does not hide the message.

**How it works:**

The private key stays with the owner. The public key may be known by anyone, so the verifier still needs a reason to believe that key belongs to the named party. That binding is the certificate, described in the TLS sheet: the name is in `subjectAlternativeName`, and the hash is SHA-256 rather than MD5 or SHA-1.

On an API, the REST sheet prefers a signature over a MAC for JWT integrity. With a MAC, every service that can check the token can also mint one with the same key. Verify `iss`, `aud`, and `exp`, and do not take the algorithm from the token header.

**Examples:**

```text
# Verify the JWT with the key you configured.
# Require iss, aud, and exp. Ignore alg from the token header.
```

**Sources:**

- [OWASP Key Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Key_Management_Cheat_Sheet.html)
- [OWASP REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
- [OWASP Transport Layer Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html)
</details>

<details>
<summary>What is a cryptographic hash function, and where is it used in web security?</summary>

### What is a cryptographic hash function, and where is it used in web security?

**Definition:**

The key-management sheet, citing NIST SP 800-57, says a cryptographic hash takes a possibly large input and produces a relatively small digest that is fundamentally hard to reverse: it is hard to find an input that yields a given output. Hash functions do not use a key. Approved hash functions are defined in FIPS 180. The same section says many cryptographic schemes use a hash as a component.

**How it works:**

The sheet lists the uses: data authentication and integrity when the hash is keyed into a MAC, compressing a message before a digital signature, deriving keys, and deterministic random numbers. TLS certificates should use SHA-256 rather than MD5 or SHA-1.

A password is a special case. The cryptographic-storage sheet says passwords must not be stored with reversible encryption; use a password hashing algorithm from the password-storage sheet. A fast hash such as the one inside a signature is the wrong tool for a password.

**When to use:**

Use a hash inside signatures, MACs, and certificate fingerprints. Use a slow password hash when the secret is a password a person chose.

**Examples:**

```text
# Certificate hashing algorithm: SHA-256.
# Passwords: a password hash, not this digest.
```

**Sources:**

- [OWASP Key Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Key_Management_Cheat_Sheet.html)
- [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- [OWASP Transport Layer Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html)
</details>

<details>
<summary>How can Public Key Infrastructure (PKI) be used to secure web applications?</summary>

### How can Public Key Infrastructure (PKI) be used to secure web applications?

**Definition:**

The TLS sheet says a correctly implemented TLS session lets the client confirm it is connected to the legitimate server. The client's identity is not verified unless client certificates are used. The key-management sheet lists trusted certificates and trust anchors as the keys that authenticate an end device. A certificate binds the public key to a name the verifier can check.

**How it works:**

Put the fully qualified name in `subjectAlternativeName`. Modern Chrome ignores `commonName` alone; keep the primary name in the CN as well for compatibility. Use RSA keys of at least 2048 bits, SHA-256, and protect the private key with filesystem permissions and administrative controls. Do not put internal names or IP addresses on a certificate that faces the Internet. Avoid a wildcard shared across different trust levels; the TLS sheet says a wildcard violates least privilege because one key is then valid for every subdomain.

For a highly privileged REST service, the REST sheet says to consider mutually authenticated client certificates in addition to HTTPS. Store keys in an HSM or a key vault when you have one, which is the cryptographic-storage guidance.

**Examples:**

```text
# SAN holds the FQDN. Hash algorithm: SHA-256. RSA key: at least 2048 bits.
```

**Sources:**

- [OWASP Transport Layer Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html)
- [OWASP Key Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Key_Management_Cheat_Sheet.html)
- [OWASP REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
</details>


### DevSecOps and Web Security

<details>
<summary>How does DevSecOps integrate security into the web development lifecycle?</summary>

### How does DevSecOps integrate security into the web development lifecycle?

**Definition:**

OWASP's guidance is to treat the pipeline as production. The GitHub Actions sheet says a CI/CD pipeline often holds sensitive credentials, so it needs threat modeling, secure review, security validation, and penetration testing, and it points at the CI/CD security cheat sheet. The Kubernetes sheet says the pipeline should vet code, scan the built image, and push to a private registry only when the assessment passes.

**How it works:**

Fail the build when the scan fails, so a vulnerable image does not enter the registry. The Docker sheet adds lint in that pipeline: a `USER` directive, pinned base-image and package versions, and `COPY` rather than `ADD`. Sign images and keep the registry private.

Threat modeling is not a one-time gate. The threat-modeling sheet says to answer what you are working on, what can go wrong, what you will do about it, and whether you did a good enough job, and to update that model with the system.

**Examples:**

```text
# Scan the image after the build. Push only if the assessment passes.
```

**Sources:**

- [OWASP CI CD Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/CI_CD_Security_Cheat_Sheet.html)
- [OWASP Kubernetes Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Kubernetes_Security_Cheat_Sheet.html)
- [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)
</details>

<details>
<summary>What are security Information and Event Management (SIEM) systems and their role in web security?</summary>

### What are security Information and Event Management (SIEM) systems and their role in web security?

**Definition:**

A SIEM, or a log aggregator, is where security logs become something you can act on. OWASP's software supply-chain sheet says logging is not sufficient by itself and prefers a centralized SIEM or similar tool so the data is actionable. The logging sheet says application logs must record when, where, who, and what for each event.

**How it works:**

Include the time, the application and its address, the user identity when it is known, the event type, the severity, and whether the action succeeded. Give related events one interaction identifier so a later reader does not have to reconstruct the request. Flag security events. Protect the logs from change.

Do not write secrets into that stream. Before you emit a log, remove fields such as passwords, API keys, and tokens. Authentication failures and authorization denials belong in the stream. A dashboard that nobody reviews is not monitoring.

**Examples:**

```text
# Each security event: when, where, who, what. No passwords or tokens.
```

**Sources:**

- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [OWASP Software Supply Chain Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Software_Supply_Chain_Security_Cheat_Sheet.html)
</details>

<details>
<summary>How can containerization improve web security?</summary>

### How can containerization improve web security?

**Definition:**

A container can run as a non-root user, from a pinned base image, in a private registry that accepts only scanned images. OWASP's Docker sheet says the pipeline should check that a `USER` is set, that the base image and OS packages are pinned, and that `COPY` is used instead of `ADD`. The Kubernetes sheet says to start from a minimal verified base image and to keep approved images in a private registry.

**How it works:**

Scan the image for known vulnerabilities, secrets, and misconfigurations before it is pushed. Sign it so the cluster can check what it runs. Publishing a port on all interfaces can skip a host firewall; bind to `127.0.0.1` when the port should stay local.

A container is not an authorization boundary for the application inside it. The process still needs least privilege, and the app still checks the caller.

**When to use:**

Use containers when you can pin the base image, scan it, and run it as a non-root user from a private registry. They do not replace application authorization.

**Examples:**

```dockerfile
USER app
```

**Sources:**

- [OWASP Docker Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html)
- [OWASP Kubernetes Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Kubernetes_Security_Cheat_Sheet.html)
</details>

<details>
<summary>Discuss the role of automated security pipelines in web application deployments.</summary>

### Discuss the role of automated security pipelines in web application deployments.

**Definition:**

An automated pipeline is the place OWASP expects security checks to run on every build: lint, static analysis, dependency scanning, and container scanning. The Docker sheet lists those checks and names scanners such as Trivy and Grype. The Kubernetes sheet says a failed assessment must fail the pipeline so the image is not pushed.

**How it works:**

Treat the pipeline definition as production code. It holds credentials, so protect it with the same review and threat model as the application, which is the GitHub Actions sheet's rule. Pin the tools. Do not print secrets into the job log.

A green pipeline shows the checks you encoded. It does not replace a human review of authorization and business rules, or a penetration test when the threat model says the automated result is not enough.

**Examples:**

```text
# Fail the job when the image scan reports a vulnerability you have not accepted.
```

**Sources:**

- [OWASP Docker Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html)
- [OWASP Kubernetes Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Kubernetes_Security_Cheat_Sheet.html)
- [OWASP CI CD Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/CI_CD_Security_Cheat_Sheet.html)
</details>


### API and Web Service Security

<details>
<summary>What are the primary concerns for REST API security?</summary>

### What are the primary concerns for REST API security?

**Definition:**

OWASP's REST sheet says a secure REST service provides only HTTPS endpoints, so credentials and tokens are protected in transit and the client can authenticate the service. Each non-public endpoint performs access control. The API is stateless: every call checks whether this caller may perform this operation. Passing session state from the client so the service can pretend to be stateless is called out as an anti-pattern.

**How it works:**

Prefer an identity provider that issues access tokens, and let the endpoint decide locally. For a JWT, require a signature rather than a MAC when services must not share a minting key, reject `alg` of `none`, and validate `iss`, `aud`, and `exp`. Allow only the HTTP methods you documented and reject others with `405 Method not allowed`. Validate length, range, format, and type. Reject an oversized body with `413`. Require a supported `Content-Type` and reject others with `415`.

Return generic errors without stack traces. Audit security events. For browser clients send `Cache-Control: no-store`, `Content-Type`, `X-Content-Type-Options: nosniff`, `Strict-Transport-Security`, and `Content-Security-Policy: frame-ancestors 'none'`. Keep management endpoints off the Internet or behind multi-factor authentication and a restricted network.

**Examples:**

```http
Cache-Control: no-store
Content-Security-Policy: frame-ancestors 'none'
X-Content-Type-Options: nosniff
```

**Sources:**

- [OWASP REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
</details>

<details>
<summary>How does OAuth 2.0 provide secure delegated access?</summary>

### How does OAuth 2.0 provide secure delegated access?

**Definition:**

The REST sheet centralizes user authentication in an identity provider that issues access tokens, so each resource endpoint does not invent its own login. OAuth 2.0 is that delegation: the client receives an access token after the resource owner authorizes it, and the resource server accepts the token. The OWASP OAuth sheet's current grant is authorization code with PKCE.

**How it works:**

Send the authorize request with `response_type=code`, a `code_challenge`, and `code_challenge_method=S256`. Do not use the implicit grant, which places the token in the URL fragment, or the resource-owner password grant, which gives the client the owner's password. Keep access tokens short-lived and audience-restricted. Prefer a sender-constrained token or rotate refresh tokens.

On the API, validate the token's integrity with your configured key, and check `iss`, `aud`, and `exp`. HTTPS is mandatory. The token is a bearer credential: protect it in storage and do not log it.

**When to use:**

Use OAuth 2.0 when a client needs access to an API on a user's behalf. Use OpenID Connect when that client must also learn who the user is.

**Examples:**

```http
GET /authorize?response_type=code&code_challenge=<S256>&code_challenge_method=S256
```

**Sources:**

- [OWASP REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
- [OWASP OAuth2 Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/OAuth2_Cheat_Sheet.html)
</details>

<details>
<summary>What are best practices for securing GraphQL APIs?</summary>

### What are best practices for securing GraphQL APIs?

**Definition:**

OWASP's GraphQL sheet says to validate all incoming data, limit expensive queries, enforce access control, and turn off insecure defaults such as introspection and GraphiQL on any public or production endpoint.

**How it works:**

Authorize both edges and nodes, not only one of them. Resolvers are a place to check permission. Use allow-lists, GraphQL scalars and enums, and schemas for mutation input. Pass values to SQL through parameterized APIs. Do not let user input choose the host of an outbound request.

Unbounded depth or amount can exhaust the service. Set depth and amount limits, paginate, set timeouts, and consider a maximum query cost. Rate-limit per IP or per user. Disable introspection with a validation rule, and enable GraphiQL only outside production.

**Examples:**

```javascript
app.use('/graphql', graphqlHTTP({
  schema: MySessionAwareGraphQLSchema,
  validationRules: [NoIntrospection],
  graphiql: process.env.NODE_ENV === 'development',
}));
```

**Sources:**

- [OWASP GraphQL Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/GraphQL_Cheat_Sheet.html)
</details>

<details>
<summary>How can webhooks be secured to ensure they are not exploited?</summary>

### How can webhooks be secured to ensure they are not exploited?

**Definition:**

A webhook is a server-to-server callback. OWASP's payment-gateway sheet says only those server-to-server callbacks should be trusted for a decision such as fulfilling an order, and that the callback's authenticity must be checked, for example with an HMAC or a secret token. The secrets sheet says never to send a secret in plaintext; use TLS.

**How it works:**

Verify the signature with the secret you stored, using a constant-time compare. Check a unique transaction id and reject an expired or already used id so a repeated callback does not fulfill twice. Confirm the amount, currency, and order id on your server with the provider's API before you fulfill. Do not trust fields that arrived only because the user's browser was redirected.

Log callback attempts without the secret. Idempotency means a second delivery of the same event does not repeat the side effect.

**Examples:**

```text
# Accept the callback only over TLS.
# Check HMAC(secret, body) and a one-time transaction id before fulfillment.
```

**Sources:**

- [OWASP Third Party Payment Gateway Integration Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Third_Party_Payment_Gateway_Integration_Cheat_Sheet.html)
- [OWASP Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)
</details>

<details>
<summary>Explain the significance of rate limiting in API security.</summary>

### Explain the significance of rate limiting in API security.

**Definition:**

Rate limiting caps how fast a client may call an API. The REST sheet says to return `429 Too Many Requests` when requests arrive too quickly, and that API keys can reduce the impact of denial of service without being the only control on sensitive data. The GraphQL sheet says to rate-limit per IP or per user so an expensive query cannot be repeated without bound.

**How it works:**

Apply the limit before the expensive work. Count by a stable identity: the API key, the user, or the tenant, not only by an address that many people share. When you refuse, the multi-tenant sheet's example sends `429` and a `Retry-After` value. Revoke a key that violates the usage agreement.

A limit protects availability. It does not authenticate the caller or authorize the object. Keep those checks.

**When to use:**

Rate-limit public and authenticated APIs that do real work. Use a tighter limit on login and on unauthenticated calls than on a normal signed-in read.

**Examples:**

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 30
```

**Sources:**

- [OWASP REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
- [OWASP GraphQL Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/GraphQL_Cheat_Sheet.html)
</details>


### Web Security Culture and Policy

<details>
<summary>How do you foster a culture of security within a web development team?</summary>

### How do you foster a culture of security within a web development team?

**Definition:**

The threat-modeling sheet says modeling should be a normal step in the lifecycle, not an add-on, and that the model is updated with the system. The secure-product sheet says security decisions must be explicit. The mobile sheet says to make security a focus of code review, and that controls people cannot use will be worked around.

**How it works:**

Give the team the four questions from the Threat Modeling Manifesto: what are we working on, what can go wrong, what will we do about it, and did we do a good enough job. Review permissions for privilege creep after release. Run `check --deploy` or the equivalent and treat warnings as work, not as noise.

Invite the people who operate the system into the review. Pair training with usable controls: MFA that can be enrolled, errors that do not dump secrets, and a place to report a suspected incident.

**Examples:**

```text
# In design review: what can go wrong, and what will we do about it?
```

**Sources:**

- [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)
- [OWASP Secure Product Design Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Product_Design_Cheat_Sheet.html)
</details>

<details>
<summary>What should a web security policy include?</summary>

### What should a web security policy include?

**Definition:**

A web security policy is the written set of decisions the product must meet. OWASP's secure-product sheet says products must meet the organization's security requirements and that those decisions must be explicit. The authorization sheet adds deny by default, least privilege, and a server-side check on every request.

**How it works:**

State who may perform which operation on which resource, and that new routes are denied until a rule allows them. Require HTTPS, secure cookies, and password hashing for accounts. Require parameterized queries and output encoding. Say where secrets live: a vault or HSM, not the repository. Name the log events (authentication failures, authorization denials) and what must never be logged.

Include session idle and absolute timeouts, MFA for administrators, and how a lost device is signed out. Name the dependency and image scanning the pipeline must pass. A policy that is not tested is a wish; the authorization sheet says to test the permission map.

**Examples:**

```text
# Deny by default. Every new route needs an explicit allow.
```

**Sources:**

- [OWASP Secure Product Design Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Product_Design_Cheat_Sheet.html)
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
</details>

<details>
<summary>Discuss the process of conducting a security risk assessment for web applications.</summary>

### Discuss the process of conducting a security risk assessment for web applications.

**Definition:**

OWASP's threat-modeling sheet defines a repeatable process: model the system from a security perspective, identify threats from that model, and decide the response. It should start in design and be updated as the system changes. The four questions are what you are working on, what can go wrong, what you will do about it, and whether the job was good enough.

**How it works:**

Map trust boundaries, the kinds of users, the resources, and the operations on those resources. The authorization sheet says to decide, for each combination, which operations are allowed, then test that map. Prefer deny by default so an unmatched request is refused.

Record the result as work: a control, an accepted risk with an owner, or a change to the design. Revisit after deployment for privilege creep. Automated scans and a penetration test feed the same list; they do not replace the model of how the business process can be misused.

**Examples:**

```text
# 1. What are we working on?
# 2. What can go wrong?
# 3. What will we do about it?
# 4. Did we do a good enough job?
```

**Sources:**

- [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
</details>

<details>
<summary>How does change management impact web security?</summary>

### How does change management impact web security?

**Definition:**

A change can add a route, a permission, or an image that nobody re-checked. The authorization sheet says to review permissions after deployment for privilege creep: current rights must not exceed the design plus formally approved changes. The Kubernetes sheet says only approved images are pushed to the private registry, and only after the pipeline's security assessment passes.

**How it works:**

Treat the pipeline and the infrastructure definition as code that is reviewed. Pin image and package versions so a rebuild does not change them silently. A framework default can change underneath you; the authorization sheet says not to rely on a library's deny-by-default behavior without an explicit configuration.

Roll back a change that fails its security check. Temporary virtual patches are changes too: record them and remove them when the code fix ships.

**Examples:**

```text
# Ship the image only after the pipeline assessment passes.
# Review permissions against the approved set after release.
```

**Sources:**

- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [OWASP Kubernetes Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Kubernetes_Security_Cheat_Sheet.html)
</details>

<details>
<summary>Why should a company have an established data breach response protocol?</summary>

### Why should a company have an established data breach response protocol?

**Definition:**

An incident that nobody has practiced becomes a scramble, and the logs that would explain it may not exist. The logging sheet says security events must be logged so they can be monitored and acted on. The mobile sheet lists an incident-response plan as its own operations item, next to monitoring. The supply-chain sheet says a centralized SIEM is preferred so those logs are actionable.

**How it works:**

Decide in advance who is called, which tokens and keys can be revoked, and how users are told. Remote logout and session invalidation, from the mobile and session sheets, are the controls that limit a lost device or a stolen token. Do not put passwords or tokens in the ticket or the log while you investigate.

The protocol also drives preparation: audit logs before and after security events, generic errors that do not leak internals, and a way to patch or apply a temporary virtual patch while the code fix is built. Write the plan before you need it, and practice the first calls.

**Examples:**

```text
# Before the incident: log security events, name the on-call, know how to revoke a token.
```

**Sources:**

- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [OWASP Mobile Application Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Mobile_Application_Security_Cheat_Sheet.html)
- [OWASP Software Supply Chain Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Software_Supply_Chain_Security_Cheat_Sheet.html)
</details>


### Miscellaneous Web Security

<details>
<summary>What is the Secure Development Lifecycle (SDL) and its relevance to web security?</summary>

### What is the Secure Development Lifecycle (SDL) and its relevance to web security?

**Definition:**

OWASP ASVS section V1.1 names secure software development lifecycle requirements and points at the threat-modeling cheat sheet. That sheet says threat modeling belongs early, in design, and again whenever the system changes. It is a standard step, not an extra. The authorization sheet says least privilege is addressed throughout the lifecycle, not only at deployment.

**How it works:**

In design, enumerate users, resources, and operations, and deny by default. In implementation, use the framework controls: parameterized queries, output encoding, CSRF on cookie paths, HTTPS, and password hashing. In the pipeline, scan dependencies and images and fail the build on a finding you have not accepted. In operation, log security events, review permissions for privilege creep, and keep the threat model current.

The four questions keep the loop short: what are we working on, what can go wrong, what will we do about it, and did we do a good enough job.

**Examples:**

```text
# SDL: model in design, build the controls, scan in CI, review permissions after release.
```

**Sources:**

- [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
</details>
