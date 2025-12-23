# Natas 11 - 12

## Details
**Username:** natas12
**Password:** yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB
**URL:** http://natas12.natas.labs.overthewire.org
**Flag/Next Password:** trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC

## TL;DR
On the webpage, there is a file upload form that allows users to upload files.
Upon reviewing the source code, we found that the file names are altered after the upload, but there are no checks or sanitization mechanisms in place.

We will exploit this vulnerability by uploading a PHP payload to the server, which we can use to retrieve the password for natas13.
To do this, we’ll manipulate the file name in the upload request.
Once the upload is successful, we can visit the file’s path to access and execute the payload, retrieving the flag for the next machine.

## Solution

### **Step 1: Identifying the Vulnerability**

When visiting the webpage, we notice a file upload form that accepts `jpeg` files. By inspecting the source code, we can clearly see how the uploaded files are handled on the server. However, upon closer inspection, we realize that there is no validation or sanitization of the uploaded files, allowing any file type to be uploaded.

To confirm this, we upload a simple PHP payload and check the server's response. Upon successful upload, the server returns a path with a random name and the `.jpeg` extension. We then visit the upload path, and the file either shows a blank image or a single pixel. This confirms that we can upload files with any extension, including PHP files.

### **Step 2: Crafting the Payload**

Next, we prepare a PHP payload designed to read the password for `natas13`. This can easily be found with a quick search for common PHP payloads used in CTF challenges or penetration tests. After crafting the payload, we upload it to the server as we did previously.

The server responds with a file path containing a random string followed by the `.jpeg` extension. When we visit this path, we see an image (or a blank image), which confirms that the file extension is being altered after the upload. This behavior is likely part of the server’s file handling process.

### **Step 3: Exploiting the Extension Change**

To further investigate, we inspect the request and notice that the uploaded file’s extension is being changed to `.jpeg` automatically, regardless of the original file extension. Since the file’s name is randomized upon upload, we can exploit this by changing the extension in our request.

To do this, we modify the file name in the upload request from `.jpeg` to `.php`. After uploading the payload, the file path will now end with `.php`, and we can visit this path directly.

Upon visiting the modified file path, the server executes the PHP payload, which reveals the flag for the next level of the challenge.
