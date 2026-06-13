# 🔐 Encryption Tool — Interactive GUI

A single-file browser-based encryption tool with a dark UI. Supports Caesar cipher, XOR, and AES-256-CBC. No server, no dependencies, no installation — just open the HTML file.

---

## 🚀 Quick Start

```bash
# Just open the file in any modern browser
open encryption_gui.html          # macOS
start encryption_gui.html         # Windows
xdg-open encryption_gui.html      # Linux
```

That's it. No npm, no pip, no build step.

---

## 🔍 How to Use Each Tab

### Caesar Cipher

1. Type your message in the input box
2. Set a shift value (1–25). Default is **13** (ROT13)
3. Click **Encrypt** or **Decrypt**
4. Copy the result

> To decrypt, use the **same shift** you used to encrypt and click Decrypt.

**Example:**
```
Input:  Hello World
Shift:  3
Output: Khoor Zruog
```

---

### XOR Cipher

**To encrypt:**
1. Type your message
2. Enter a key (any string, e.g. `mysecretkey`)
3. Click **Encrypt → base64**
4. Copy the base64 output

**To decrypt:**
1. Paste the base64 string into the input
2. Enter the **same key**
3. Click **Decrypt from base64**

> The key must be identical for encryption and decryption.

---

### AES-256 (Recommended for real use)

**To encrypt:**
1. Type your message
2. Enter a strong password
3. Click **Encrypt**
4. Copy the full JSON output — you need all three fields (`salt`, `iv`, `ciphertext`) to decrypt

**To decrypt:**
1. Paste the **complete JSON** into the input box
2. Enter the **same password**
3. Click **Decrypt (paste JSON above)**

**Example JSON output:**
```json
{
  "salt": "abc123...==",
  "iv": "xyz789...==",
  "ciphertext": "def456...=="
}
```

> Keep the JSON and your password safe. Losing either means losing the data permanently.

---

## 🔒 Technical Details

| Feature | Detail |
|---|---|
| Encryption API | Browser-native `window.crypto.subtle` (Web Crypto API) |
| AES key size | 256-bit |
| AES mode | CBC (Cipher Block Chaining) |
| Key derivation | PBKDF2-HMAC-SHA256, 100,000 iterations |
| IV | 16 bytes, randomly generated per encryption |
| Salt | 16 bytes, randomly generated per encryption |
| Encoding | Base64 for all binary output |
| Server calls | None — runs 100% in browser |

---

## 🧠 Understanding the Three Ciphers

### Caesar — Classical (1st century BC)
Shifts every letter by a fixed amount. Only 25 possible keys, broken instantly by brute force or frequency analysis. **Educational use only.**

### XOR — Intermediate
XOR's each plaintext byte against a repeating key byte. Fast and simple, but vulnerable to known-plaintext attacks and catastrophically insecure with reused keys. **Do not reuse keys. Not for production.**

### AES-256 — Modern Standard
The encryption standard used in HTTPS, file storage, and password managers. With a random salt and IV per message and 100k PBKDF2 iterations, brute-forcing even a modest password is computationally infeasible. **Safe for real use.**

---

## 🌐 Browser Compatibility

| Browser | Supported |
|---|---|
| Chrome 60+ | ✅ |
| Firefox 57+ | ✅ |
| Safari 11+ | ✅ |
| Edge 79+ | ✅ |
| Internet Explorer | ❌ (no Web Crypto API) |

---

## 📂 File Structure

```
encryption_gui/
├── encryption_gui.html   # The entire tool — HTML + CSS + JS in one file
└── README.md             # This file
```

---

## 💡 Security Notes

- The AES implementation uses the browser's native Web Crypto API — the same engine used by your browser for HTTPS. It is not a custom implementation.
- A fresh random **salt** and **IV** are generated on every encryption, meaning the same message + password will produce different ciphertext each time. This is correct and expected behavior.
- For new projects, prefer **AES-GCM** over CBC — GCM provides authenticated encryption (detects tampering). CBC does not.
- Never share your password alongside the ciphertext.

---

## 🔧 Extending the Tool

The JavaScript is intentionally readable. To add a new cipher:

1. Add a new tab button in the `.tab-bar` div
2. Add a new `.panel` section with your inputs
3. Write an `async function myRun(mode)` that reads inputs, processes, and calls `setOut()` + `setStatus()`

The `deriveKey()`, `b64()`, and `unb64()` helpers are reusable for any Web Crypto operation.
