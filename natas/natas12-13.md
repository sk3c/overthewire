# Natas 12 - 13

## Details
**Username:** natas13
**Password:** trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC
**URL:** http://natas13.natas.labs.overthewire.org
**Flag/Next Password:** z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ

## TL;DR
As with the previous machine, we have a file upload vulnerability here.
However, this time, the application has implemented sanitization methods to validate the file type.
To bypass these checks, we need to upload a payload that is recognized as an image but still functions as a PHP file.

We can achieve this by using exiftool to craft a polymorphic payload.
This tool allows us to manipulate the file's metadata in such a way that it appears as a valid image while retaining the functionality of a PHP script.
Once the payload is successfully uploaded, it will execute on the server, revealing the flag for the next level.


## Solution

### **Step 1: Identifying the Validation Mechanism**
In this machine, we again encounter a file upload form similar to the previous one.
However, this time the source code contains the `exif_imagetype` function, which is used to verify whether the uploaded file is a valid image.
To test this, we upload a random file, and the server returns a message indicating that the file is not recognized as an image.

We attempt the trick used in the last machine — changing the file extension in the request — but this does not bypass the check.
To exploit the vulnerability in this case, we need to craft a custom payload that behaves like an image while still being a functional PHP script.

### **Step 2: Crafting the Polymorphic Payload**
To create a valid payload, we use `exiftool`, a popular tool for crafting polymorphic payloads.
This tool allows us to embed PHP code inside an image file, making it appear as a legitimate image while retaining the functionality of a PHP script.

For our payload, we write a simple PHP script that uses the `file_get_contents()` function to retrieve the contents of the `natas15` password file.
We then use `exiftool` to inject this PHP code into the image metadata.
This way, the file will be validated as an image by the `exif_imagetype` function but will actually execute the PHP code when accessed.

### **Step 3: Uploading and Retrieving the Flag**
As in the previous machine, we upload the crafted payload and manipulate the file extension in the request to change it to `.php`.
After the upload, we visit the path where the file is stored.

Upon inspecting the page source, we find a random string of characters.
In the middle of this gibberish, after the `Comment` tag, we find the password for `natas15`. This is our flag for the next level.

