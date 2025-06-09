# File-Upload Vulnerabilities:

It occurs when a server allows to upload a file without validating the name, file type, size. If we put/upload a server-side script files it’ll cause remote code execution.

In some cases, the act of uploading the file is in itself enough to cause damage. Other attacks may involve a follow-up HTTP request for the file, typically to trigger its execution by the server.

Let’s say the file type isn’t validated and the server allows to upload file types with any extensions.(.php, .jsp) to be executed as code. In this case, an attacker could potentially upload a server-side code file that functions as a web shell, effectively granting them full control over the server.

## **How do file upload vulnerabilities arise?**

Given the fairly obvious dangers, it's rare for websites in the wild to have no restrictions whatsoever on which files users are allowed to upload. More commonly, developers implement what they believe to be robust validation that is either inherently flawed or can be easily bypassed.

For example, they may attempt to blacklist dangerous file types, but fail to account for parsing discrepancies when checking the file extensions. As with any blacklist, it's also easy to accidentally omit more obscure file types that may still be dangerous.

In other cases, the website may attempt to check the file type by verifying properties that can be easily manipulated by an attacker using tools like Burp Proxy or Repeater.

Ultimately, even robust validation measures may be applied inconsistently across the network of hosts and directories that form the website, resulting in discrepancies that can be exploited.

## How web servers deals with static files?

Initially all files are static in webserver and the url is mapped 1:1 with filesystem. But as websites become more dynamic, they removed this technic, but still they are using static files for stylesheets and some other purposes.

- If this file type is non-executable, such as an image or a static HTML page, the server may just send the file's contents to the client in an HTTP response.
- If the file type is executable, such as a PHP file, **and** the server is configured to execute files of this type, it will assign variables based on the headers and parameters in the HTTP request before running the script. The resulting output may then be sent to the client in an HTTP response.
- If the file type is executable, but the server **is not** configured to execute files of this type, it will generally respond with an error.

> Web Shell:
A malicious script that enables you to execute dangerous command in the web server by sending http requests to proper endpoints.
> 

For reading files from the server

```sql
<?php echo file_get_contents('/path/to/target/file'); ?>
```

For executing a command : 

```sql
<?php echo system($_GET['command']); ?>
```

## How to properly intercept the request and modify the file type

- First upload a proper fileformat and handle the proxy in burp.
- Now send the post request in burp to repeater, modify file name and extension.
- Remove the content and add a php or any server-side scripting one-liner in the content.
- And post/send the post-request to same path.
- Again send get-request to the file path with filename along with extension.

If you can find a way to upload a script to a different directory that's not supposed to contain user-supplied files, the server may execute your script after all.

**Tip**
Web servers often use the `filename` field in `multipart/form-data` requests to determine the name and location where the file should be saved.

You should also note that even though you may send all of your requests to the same domain name, this often points to a reverse proxy server of some kind, such as a load balancer. Your requests will often be handled by additional servers behind the scenes, which may also be configured differently.

### What if server doesn’t allow to execute a file in the directory

- We initially try to upload the file in previous directory like file name as ../filename
- But some servers will reject the ../ and just put filename for more security, which means the file will be stored in same directory.
- For that we can bypass this by using some commands and i used %2f in portswigger.
