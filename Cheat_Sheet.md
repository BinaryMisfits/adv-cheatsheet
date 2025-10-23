# Set-20

## **1. Digital Signatures**

### **Aim**
To sign a file using a private key and verify the signature using the sender’s public key.

---

### **Procedure (Using OpenSSL)**

```bash
# Step 1: Generate a private key
openssl genpkey -algorithm RSA -out private.pem

# Step 2: Extract the public key
openssl rsa -pubout -in private.pem -out public.pem

# Step 3: Create a sample message file
echo "This is a confidential message" > message.txt

# Step 4: Generate a digital signature
openssl dgst -sha256 -sign private.pem -out signature.bin message.txt

# Step 5: Verify the signature
openssl dgst -sha256 -verify public.pem -signature signature.bin message.txt
```

**Expected Output:**
```
Verified OK
```

---

### **(a) Purpose of Digital Signatures**
The purpose of digital signatures is to:
- **Authenticate** the identity of the sender.  
- **Ensure data integrity** — confirm that the message has not been altered.  
- **Provide non-repudiation** — the sender cannot later deny sending the message.  

---

### **(b) How Signature Verification Ensures Message Integrity**
- When a message is signed, a **hash** of the message is generated and **encrypted** using the sender’s **private key**.  
- During verification, the receiver decrypts the signature using the sender’s **public key** and compares it with a newly computed hash of the received message.  
- If both hashes match, it confirms that:
  - The message has not been modified.
  - The signature was created by the rightful owner of the private key.

Thus, message integrity and authenticity are ensured.

---

## **2. XOR Cipher Encryption and Decryption**

### **Aim**
To write a program to encrypt and decrypt text using the XOR cipher.

---

### **Algorithm**
1. Take input text and an integer key.  
2. For each character in the text:
   - Convert it to its ASCII value.
   - XOR it with the key.
   - Convert it back to a character.
3. Join all characters to form the ciphertext.  
4. Apply the same function again with the same key to decrypt.

---

### **Python Program**
```python
def xor_cipher(text, key):
    result = ""
    for char in text:
        result += chr(ord(char) ^ key)
    return result

# Example usage
plaintext = "Hello World"
key = 42  # Example key

encrypted = xor_cipher(plaintext, key)
print("Encrypted:", encrypted)

decrypted = xor_cipher(encrypted, key)
print("Decrypted:", decrypted)
```

---

### **Sample Output**
```
Encrypted: *garbled text*
Decrypted: Hello World
```

---

### **(a) Why XOR Symmetric Encryption is Reversible with the Same Key**
XOR encryption works because the XOR operation is **self-inverse**:
```
(A XOR B) XOR B = A
```
Applying the same key twice returns the original message.  
Hence, XOR is **symmetric**, meaning the same key is used for both encryption and decryption.

---

### **(b) Limitations of XOR Encryption**
1. **Weak Security:** Easily broken if the key is short or reused.  
2. **Predictable Patterns:** Repeating plaintext with small keys produces visible patterns.  
3. **No Key Management:** Not suitable for multi-user systems.  
4. **Not Secure for Real Use:** Only used for demonstrations or lightweight tasks.

---

### **Result**
- Successfully signed and verified a file using digital signatures.  
- Implemented XOR cipher encryption and decryption demonstrating symmetric reversibility.

---
