# AWS Transfer Family [In progress]
AWS Transfer Family is a secure transfer service that enables you to transfer files into and out of AWS storage services like Amazon S3 and Amazon EFS.  
AWS Transfer Family offers fully managed support for the transfer of files over `SFTP`, `AS2`, `FTPS`, `FTP`, and `web browser-based` transfers directly into and out of AWS storage services.


## Explanation of protocols or each transfer method supported by AWS Transfer Family
1. **FTP (File Transfer Protocol)**
- Sending files from one computer to another without security.
- Data is sent in plain text, which can be intercepted.
- Commonly used for simple file transfers within trusted networks.
- Not recommended for sensitive data due to lack of encryption.
- Example use case: Transferring non-sensitive files within a secure internal network.

2. **FTPS (FTP Secure)**
- FTP, but with encryption added.
- Data is encrypted using SSL/TLS, making it more secure than FTP.
- Example use case: Transferring sensitive files over the internet securely for companies that already use FTP but need security.

3. **SFTP (SSH File Transfer Protocol)**: Most commonly used for secure file transfers over SSH.
- A secure way to transfer files using SSH.
- A partner uses an SFTP client (such as WinSCP) to connect to AWS Transfer Family server and upload/download files.
- All data is encrypted during transfer.
- Example use case: Securely exchanging files with external partners or clients. Common in enterprise systems.

4. **AS2 (Applicability Statement 2)**: Used when data security and proof of delivery are critical.
- A business-to-business secure file exchange standard with proof of delivery.
- Commonly used in industries like retail and healthcare for exchanging EDI (Electronic Data Interchange) documents. It is used in legal and compliance contexts where proof of delivery is important.
- Example use case: Exchanging purchase orders or invoices between business partners.

5. **Web browser-based transfers**: Simple way to upload/download files using a web interface.
- Users can upload/download files directly through a web page.
- Example use case: Allowing users to manually upload files to an S3 bucket via a web interface. Common for non-technical users.


**Summary:**

| Method  | Simple meaning       | Security | Typical user         |
|---------|----------------------|----------|----------------------|
| FTP     | Basic file sending   | ❌ None   | Legacy systems       |
| FTPS    | Encrypted FTP        | ✅ Yes    | Older secure systems |
| SFTP    | Secure file transfer | ✅ Yes    | Modern enterprises   |
| AS2     | Secure + legal proof | ✅ Yes    | B2B, EDI partners    |
| Browser | Upload via website   | ✅ Yes    | Non-technical users  |
