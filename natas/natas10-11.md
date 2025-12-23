# Natas 10 - 11

## Details
**Username:** natas11
**Password:** UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk
**URL:** http://natas11.natas.labs.overthewire.org
**Flag/Next Password:** yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB

## TL;DR
On the webpage, we have a simple form to enter a color code. In the source code, there is an implementation for cookie encryption.
We will exploit a simple XOR vulnerability, where `a ^ b = c` and `c ^ a = b`. Here, `a` represents the data, `b` is the key, and `c` is the encrypted cookie.
To retrieve the key, we need to modify the `xor_encrypt` function to expose the key used for encryption.

## Solution

### **Step 1:**
In the page source code, we identify the encryption logic for the cookie. By analyzing this code, we deduce that the flag can be retrieved by setting the `showpassword` value in the cookie to `yes`. However, in order to create our own encrypted cookie, we first need to obtain the `<censored>` encryption key.

### **Step 2:**
To extract the encryption key, we can use a simple XOR operation. Let’s define the variables:
- `A` represents our basic data, which will be an array containing the `showpassword` and `bgcolor` values.
- `B` is the encryption key (which we don’t yet have).
- `C` is the encrypted cookie, which we can retrieve from the cookie storage.

First, we need to decrypt the cookie using the `base64_decode` function. Then, modify the `key` in the `xor_encrypt` function to use a known value.

Example:
```php
json_encode(array("showpassword" => "no", "bgcolor" => "#ffffff"))
```
By executing this script, we can reveal the encryption key.

### **Step 3:**
With the encryption key in hand, we can now create our own encrypted cookie using the same code found in the page source.
Replace `<censored>` with the key we obtained. Finally, replace the original cookie in our browser with the newly created one.
Once this is done, we should have access to the password for Natas 12.
