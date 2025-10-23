Here’s a **complete detailed answer set (16-mark style)** for **Set–18**, covering all sub-questions clearly and technically:

---

## **1. Generate a PGP key pair, export the public key, and encrypt a sample message.** (4 Marks)

### **Steps:**

1. **Generate PGP key pair**

   ```bash
   gpg --gen-key
   ```

   * Choose key type (default RSA and RSA).
   * Select key size (2048 or 4096 bits recommended).
   * Provide name, email, and passphrase.
   * GPG generates both public and private keys.

2. **List generated keys**

   ```bash
   gpg --list-keys
   ```

3. **Export public key**

   ```bash
   gpg --export -a "username" > public_key.asc
   ```

   * The `-a` option exports in ASCII-armored format.
   * The file `public_key.asc` can be shared publicly.

4. **Encrypt a sample message**

   ```bash
   echo "Hello, this is a test message." > message.txt
   gpg --encrypt --recipient "username" message.txt
   ```

   * Output: `message.txt.gpg` (encrypted file)

5. **Decrypt the message**

   ```bash
   gpg --decrypt message.txt.gpg
   ```

---

### **(a) Difference between Public Key and Private Key**

| Feature       | Public Key                                       | Private Key                                      |
| ------------- | ------------------------------------------------ | ------------------------------------------------ |
| **Purpose**   | Used for encrypting data or verifying signatures | Used for decrypting data or creating signatures  |
| **Sharing**   | Publicly distributed to others                   | Kept secret and secure by the owner              |
| **Ownership** | Identifies the user publicly                     | Provides proof of identity and confidentiality   |
| **Security**  | Compromise affects only confidentiality          | Compromise affects integrity and non-repudiation |

---

### **(b) Command used to generate PGP keys in Linux**

```bash
gpg --gen-key
```

This command launches the interactive GPG key generation process.

---

## **2. Implement the Blowfish algorithm logic in Java to encrypt the string “Cyber Security”.** (8 Marks)

### **Java Code Example**

```java
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
import java.util.Base64;

public class BlowfishExample {
    public static void main(String[] args) throws Exception {
        String data = "Cyber Security";
        String key = "MySecretKey"; // 4–56 bytes key size

        SecretKeySpec secretKey = new SecretKeySpec(key.getBytes(), "Blowfish");
        Cipher cipher = Cipher.getInstance("Blowfish");

        cipher.init(Cipher.ENCRYPT_MODE, secretKey);
        byte[] encrypted = cipher.doFinal(data.getBytes());

        System.out.println("Encrypted Text: " + Base64.getEncoder().encodeToString(encrypted));
    }
}
```

**Explanation:**

* `SecretKeySpec` creates a key specification for Blowfish.
* `Cipher.getInstance("Blowfish")` loads the Blowfish algorithm.
* The data is encrypted using the key and displayed in Base64 form.

---

### **(a) What is the block size of Blowfish?**

* **Blowfish has a fixed block size of 64 bits (8 bytes).**
* It supports variable key sizes ranging from **32 bits (4 bytes)** to **448 bits (56 bytes)**.

---

### **(b) How does Blowfish handle key expansion?**

Blowfish uses a **key expansion process** to generate multiple subkeys from the user key.

**Process:**

1. The user-provided key is divided and cyclically XORed into the **P-array (18 subkeys)**.
2. Then Blowfish encrypts an all-zero string using the current subkeys.
3. The output replaces the first two entries of the P-array.
4. The process continues to update the P-array and **four 32-bit S-boxes (each with 256 entries)**.
5. Final P-array and S-boxes are used in encryption rounds.

**Purpose:**
This key expansion ensures the key thoroughly affects all encryption rounds, enhancing security and diffusion.

---

✅ **Summary Table**

| Question | Concept                 | Marks | Key Points                                          |
| -------- | ----------------------- | ----- | --------------------------------------------------- |
| Q1       | PGP Key Pair + Commands | 4     | gpg commands, encryption, public/private difference |
| Q2       | Blowfish Implementation | 8     | Java code, 64-bit block, key expansion steps        |

---

Would you like me to turn this answer into a **formatted PDF or DOCX** (for submission or print)?
