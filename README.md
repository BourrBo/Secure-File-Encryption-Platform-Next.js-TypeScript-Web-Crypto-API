# 🔐 Secure File Encryption Platform

A client-side file encryption tool built with **Next.js**, **TypeScript**, and the browser-native **Web Crypto API**. Files are encrypted and decrypted entirely in the browser — nothing is ever sent to a server, so your files and passwords never leave your device unless you explicitly choose to upload them to the cloud.

---

## ✨ Features

- **AES-256-GCM Encryption** — Industry-standard authenticated encryption performed via the browser's native Web Crypto API.
- **PBKDF2 Key Derivation** — Passwords are run through 250,000 PBKDF2 (SHA-256) iterations with a random salt before being used as an encryption key, protecting against brute-force and rainbow table attacks.
- **Compression Before Encryption** — Files are compressed with [pako](https://github.com/nodeca/pako) (zlib) prior to encryption, reducing output size.
- **Password Strength Meter** — Real-time feedback on password strength while typing.
- **File Integrity Checker** — Compare two files byte-for-byte to verify they're identical (e.g., confirming a decrypted file matches the original).
- **Optional Cloud Storage** — Sign in with Google (via Firebase Auth) to upload your encrypted `.cptk` files to Firebase Cloud Storage. This is entirely optional; the core encryption/decryption flow works without any backend or account.
- **100% Client-Side Crypto** — Encryption keys and plaintext file contents never touch a server.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Framework | [Next.js 15](https://nextjs.org/) (App Router) |
| Language | TypeScript |
| UI | Tailwind CSS, [shadcn/ui](https://ui.shadcn.com/), Radix UI |
| Cryptography | Web Crypto API (AES-GCM, PBKDF2) |
| Compression | [pako](https://github.com/nodeca/pako) |
| Auth & Storage (optional) | Firebase Auth, Firebase Cloud Storage |
| Forms | react-hook-form + zod |

---

## 🚀 Getting Started

### Prerequisites

- Node.js 18+
- npm

### Installation

```bash
git clone https://github.com/BourrBo/Secure-File-Encryption-Platform-Next.js-TypeScript-Web-Crypto-API.git
cd Secure-File-Encryption-Platform-Next.js-TypeScript-Web-Crypto-API
npm install
```

### Run the dev server

```bash
npm run dev
```

The app runs on **[http://localhost:9003](http://localhost:9003)**.

### Build for production

```bash
npm run build
npm start
```

---

## ⚙️ Environment Variables (Optional)

Cloud Storage and Google Sign-In are optional features powered by Firebase. If you don't configure them, the app works perfectly fine for local file encryption/decryption — those buttons simply won't appear.

To enable them, create a `.env` file in the project root with your Firebase project credentials:

```env
NEXT_PUBLIC_FIREBASE_API_KEY=
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=
NEXT_PUBLIC_FIREBASE_PROJECT_ID=
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=
NEXT_PUBLIC_FIREBASE_APP_ID=
```

You'll need to:
1. Create a project at the [Firebase Console](https://console.firebase.google.com/).
2. Enable **Google** as a sign-in provider under Authentication.
3. Enable **Cloud Storage**.
4. Copy your web app's config values into the `.env` file above.

> **Note:** When deploying (e.g., to Vercel), add these as Environment Variables in your project settings — `.env` files are not committed or deployed automatically.

---

## 🧠 How the Encryption Works

1. A random **salt** and **IV (initialization vector)** are generated for every encryption.
2. Your password + the salt are run through **PBKDF2** (250,000 iterations, SHA-256) to derive a 256-bit AES key.
3. The file is **compressed** with pako before encryption.
4. The compressed data is encrypted using **AES-256-GCM**.
5. The salt, IV, and ciphertext are concatenated into a single output file (`.cptk` extension).

Decryption reverses this process — extracting the salt and IV, re-deriving the key from your password, decrypting, then decompressing. An incorrect password will fail decryption (thanks to GCM's built-in authentication), rather than producing corrupted output.

---

## 🔒 Security Notes

- This project is intended as a learning/demo project showcasing the Web Crypto API. Review and audit the code yourself before relying on it for sensitive data.
- **There is no password recovery.** If you forget the password used to encrypt a file, the data cannot be recovered.
- Keep your dependencies up to date — run `npm audit` periodically and check for Next.js security advisories.

---

## 📄 License

This project is open source. Add a license of your choice (e.g., MIT) if you plan to share or accept contributions.
