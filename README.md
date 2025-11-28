# Encrypter – Browser-Only Text Encryption Tool

Encrypter is a tiny browser-only tool for encrypting and decrypting text with a password.  
All encryption and decryption happen **entirely in the browser** using the Web Crypto API – no backend, no database, no account, and no tracking.

- 🔐 Client-side AES-GCM encryption  
- 🧠 Password-based key derivation (no keys stored anywhere)  
- 🔗 Share ciphertext via links  
- 📷 Decrypt from QR codes  
- 🧾 Pure static site: HTML, CSS, and JavaScript only  

---

## Live Demo

👉 https://www.encrypter.site

You can use the live site directly in your browser.  
Nothing you type is sent to any server – everything stays in memory on your device.

---

## Features

- **Encrypt tab**  
  - Type or paste any text you want to protect.  
  - Choose a password.  
  - Encrypt to a compact ciphertext string.  
  - Optionally turn the ciphertext into a shareable link or a QR code.

- **Decrypt tab**  
  - Paste ciphertext or open a link that pre-fills the ciphertext.  
  - Enter the password.  
  - Decrypt locally to recover the original text.  
  - A separate “Decrypt from QR code” flow lets you upload a QR image that contains ciphertext and decrypt it with a password.

- **Browser-only crypto**  
  - Uses the Web Crypto API (AES-GCM).  
  - No custom crypto algorithms.  
  - No server-side encryption API.

- **Pure static site**  
  - No backend, no database, no user accounts.  
  - Easy to host on static platforms like Vercel, Netlify, GitHub Pages, etc.

---

## How It Works (High Level)

> Do not roll your own crypto.

Encrypter uses standard primitives provided by the browser:

- **Password → Key**  
  The password is converted into a cryptographic key using a key-derivation function (e.g. PBKDF2 with SHA-256, salt, and multiple iterations).

- **Encryption**  
  The plaintext is encrypted using **AES-GCM** with:  
  - a randomly generated **salt** (for key derivation)  
  - a randomly generated **IV** (initialization vector)  

  The output is `(salt, iv, ciphertext)` encoded into a single string (for example Base64 or a custom encoding).

- **Decryption**  
  To decrypt, Encrypter:  
  1. Parses the encoded package.  
  2. Re-derives the key from the password + salt.  
  3. Calls `crypto.subtle.decrypt` with AES-GCM and the IV.  
  4. Converts the decrypted bytes back into text.

The password and plaintext never leave the browser.  
Only the encoded ciphertext (plus salt + IV) is meant to be shared.

---

## Getting Started (Local Development)

You can run Encrypter locally as a simple static site.

### 1. Clone the repository

    git clone https://github.com/YC010101/www.encrypter.site.git
    cd www.encrypter.site

### 2. Run it locally

Because this is just a static site, you have options:

**Option A: Open `index.html` directly**

- Open `index.html` in your browser (double-click or drag into a tab).  
- For most modern browsers this is enough.

**Option B: Use a simple static server**

If you prefer running via HTTP (recommended for some browser APIs), you can use any simple static server, for example:

    # Using Node.js and npx
    npx serve .

Or any other static file server you like.  
Then open the printed `http://localhost:...` URL in your browser.

---

## Deployment

Encrypter is designed to work well with static hosting providers such as:

- Vercel  
- Netlify  
- GitHub Pages  
- Cloudflare Pages  
- Any other static web server  

Basic idea:

1. Build step: **none** (this is a pre-built static site).  
2. Output directory: project root (or `/public` if you structure it that way).  
3. Point your host to serve `index.html`, `style.css`, `main.js`, and any assets (e.g. logo SVG).

For example, on Vercel you can:

- Create a new project from this GitHub repo.  
- Framework preset: `Other` or `Static Site`.  
- Build command: empty.  
- Output directory: `.`  

---

## Security Notes

- This project is intended as a **practical, browser-only encryption helper**, not a replacement for fully audited cryptographic tools.  
- If you forget your password, your data is effectively unrecoverable. There is no “reset password” feature.  
- If your device is compromised (keyloggers, malicious extensions, etc.), Encrypter cannot protect you.  
- You are trusting:  
  - the static files you load (HTML/JS/CSS)  
  - the hosting and DNS setup that serves them  

  If you are extremely paranoid, you can:  
  - clone this repo  
  - review the code  
  - run it locally or host it yourself  

---

## Roadmap / Ideas

Some possible future improvements:

- A “Pro mode” that shows more technical details (salt, IV, algorithm, iterations).  
- Better mobile UX, especially for QR code workflows.  
- A downloadable “offline bundle” that you can run as a single HTML file.  
- More documentation on threat models, key management, and best practices.

---

## Feedback & Contributions

Feedback is very welcome:

- Suggestions for UX improvements.  
- Security reviews and threat-model feedback.  
- Bug reports.  
- Ideas for additional small, focused features.  

If you want to contribute:

1. Open an issue to discuss your idea or bug.  
2. Fork the repo.  
3. Create a feature branch.  
4. Open a pull request.  

---

## License

This project is licensed under the **MIT License**.  
