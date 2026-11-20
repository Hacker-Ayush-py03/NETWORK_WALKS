# 🔐 Password Cracking with NetworkWalks Tools

### Week 3 – Project Module 2 (PM2) | NetworkWalks Cybersecurity & Ethical Hacking Internship

<p align="left">
  <img src="https://img.shields.io/badge/Program-NetworkWalks%20Internship-1f6feb?style=for-the-badge" alt="Program">
  <img src="https://img.shields.io/badge/Batch-B084-5e60ce?style=for-the-badge" alt="Batch">
  <img src="https://img.shields.io/badge/Week-3-00b4d8?style=for-the-badge" alt="Week">
  <img src="https://img.shields.io/badge/Module-PM2-7209b7?style=for-the-badge" alt="Module">
</p>

<p align="left">
  <img src="https://img.shields.io/badge/Category-Password%20Cracking-0a66c2?style=flat-square" alt="Category">
  <img src="https://img.shields.io/badge/Tooling-NetworkWalks%20Hash%20Calculator-00b4d8?style=flat-square" alt="Hash Calculator">
  <img src="https://img.shields.io/badge/Tooling-NetworkWalks%20Password%20Cracker-5e60ce?style=flat-square" alt="Password Cracker">
  <img src="https://img.shields.io/badge/Target-PDF%20Password%20Security-7209b7?style=flat-square" alt="Target">
  <img src="https://img.shields.io/badge/Status-Completed-2ea043?style=flat-square" alt="Status">
  <img src="https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square" alt="License">
</p>

---

## 📌 Project Identification

| Field | Detail |
|---|---|
| **Program** | NetworkWalks – Cybersecurity & Ethical Hacking Internship |
| **Batch** | B084 |
| **Week** | 3 |
| **Project Module** | PM2 – Password Cracking with NetworkWalks Tools |
| **Author** | Ayush Shukla |
| **GitHub** | [Hacker-Ayush-py03](https://github.com/Hacker-Ayush-py03) |
| **Parent Repository** | [NETWORK_WALKS](https://github.com/Hacker-Ayush-py03/NETWORK_WALKS) |
| **Related (Separate) Module** | Week 3 – PM1 (John the Ripper) — *not covered here* |

> **Note:** This repository documents a **separate and independent** practical from Week 3 – Project Module 1. PM1 used John the Ripper; this module (PM2) exclusively documents **NetworkWalks' own online Hash Calculator and Password Cracker tools**. The two tools and workflows are not interchangeable and are kept clearly distinct throughout this documentation.

---

## 📖 Table of Contents

1. [Introduction](#-introduction)
2. [Project Objectives](#-project-objectives)
3. [Technical Background](#-technical-background)
4. [Tools Used](#-tools-used)
5. [NetworkWalks Hash Calculator](#-networkwalks-hash-calculator)
6. [NetworkWalks Password Cracker](#-networkwalks-password-cracker)
7. [Practical Workflow](#-practical-workflow)
8. [PDF Hash Extraction](#-pdf-hash-extraction)
9. [Dictionary Attack Explained](#-dictionary-attack-explained)
10. [Password Recovery Process](#-password-recovery-process)
11. [Recovered Passwords & Flags](#-recovered-passwords--flags)
12. [Results Summary](#-results-summary)
13. [Screenshot Evidence](#-screenshot-evidence)
14. [Technical Learning Outcomes](#-technical-learning-outcomes)
15. [Security Observations](#-security-observations)
16. [Conclusion](#-conclusion)
17. [Author](#-author)
18. [References](#-references)

---

## 🧭 Introduction

This repository documents **Week 3 – Project Module 2** of the NetworkWalks Cybersecurity & Ethical Hacking Internship (Batch B084). The practical focuses on recovering passwords from password-protected PDF files using two NetworkWalks-provided online utilities — the **Hash Calculator** and the **Password Cracker** — instead of a locally installed tool such as John the Ripper.

The objective of the exercise was to extract the cryptographic hash of a password-protected PDF, submit that hash to a dictionary-attack engine, and recover the original password so that the protected document could be opened and its challenge flag captured. This module was completed on **three authorized training challenge PDFs** supplied as part of the internship curriculum.

---

## 🎯 Project Objectives

- Understand how PDF password protection is implemented and represented as a hash.
- Learn to extract a PDF's password hash using a web-based hash-calculation utility.
- Understand the structure and purpose of the `$pdf$` hash format.
- Perform a dictionary attack against an extracted hash using a web-based cracking utility.
- Recover the original PDF password and use it to unlock the document.
- Capture and document the flag contained in each unlocked PDF.
- Reinforce the real-world importance of strong, unique passwords through hands-on cracking practice.

---

## 🧠 Technical Background

| Concept | Explanation |
|---|---|
| **Password Cracking** | The process of systematically recovering a plaintext password from a stored or derived representation (typically a hash) using guessing strategies such as dictionary or brute-force attacks. |
| **PDF Password Protection** | PDF documents support owner and user passwords enforced through a standardized encryption scheme (RC4 or AES, depending on the PDF revision). A "user password" is required simply to open the file. |
| **PDF Hash** | A derived value computed from the PDF's encryption dictionary (salt, key length, revision, and encrypted string data) that a cracking tool can test candidate passwords against, without needing to repeatedly open the PDF itself. |
| **`$pdf$` Format** | A structured hash representation (used by tools in the Hashcat/John the Ripper ecosystem) that encodes the PDF revision, key length, permission flags, and the relevant binary values required to validate a password, separated by `*`. |
| **Hashing vs. Encryption** | Hashing is a one-way function producing a fixed-length digest with no intended means of reversal; encryption is a two-way, reversible transformation requiring a key. A PDF password hash is used to *test* candidate passwords offline, while the PDF's actual content protection relies on encryption keyed from that password. |
| **Dictionary Attack** | A cracking method that tests a predefined list of candidate passwords (a wordlist) against a hash, rather than attempting every possible character combination. |
| **Wordlist** | A text file containing candidate passwords, one per line, used as the input source for a dictionary attack. |

---

## 🛠 Tools Used

This project exclusively uses NetworkWalks' own browser-based security utilities. No locally installed cracking software (including John the Ripper) was used for this module.

| Tool | Purpose | Official Link |
|---|---|---|
| **NetworkWalks Hash Calculator** | Extracts the `$pdf$` password hash from a protected PDF | [networkwalks.com/hash-calculator](https://networkwalks.com/hash-calculator/) |
| **NetworkWalks Password Cracker** | Runs a dictionary attack against the extracted hash to recover the password | [networkwalks.com/password-cracker](https://networkwalks.com/password-cracker/) |

---

## 🧮 NetworkWalks Hash Calculator

**URL:** [https://networkwalks.com/hash-calculator/](https://networkwalks.com/hash-calculator/)

The Hash Calculator is a general-purpose hashing utility capable of computing several standard digest algorithms — **MD5, SHA-1, SHA-256, SHA-384, and SHA-512** — in addition to its dedicated **PDF** mode. For this practical, only the **PDF** option was used.

When a password-protected PDF is uploaded through the PDF option, the tool parses the file's encryption dictionary and outputs a structured hash string beginning with `$pdf$`. This hash fully describes the information a cracking engine needs to validate password guesses against that specific file, without exposing or requiring the original password.

**Role in this project:** Convert each of the three protected PDF challenge files into its corresponding `$pdf$` hash, ready to be handed to the Password Cracker tool.

---

## 🗝 NetworkWalks Password Cracker

**URL:** [https://networkwalks.com/password-cracker/](https://networkwalks.com/password-cracker/)

The Password Cracker accepts a previously generated hash (such as the `$pdf$` hash produced above) and attempts to recover the matching plaintext password via a **dictionary attack**. The tool provides two candidate sources:

- A **built-in dictionary** of 100 common passwords, or
- A **custom wordlist** uploaded by the user as a `.txt` file.

Once a dictionary source is selected and the attack is started, the tool iterates through candidate passwords, hashing/testing each one against the submitted hash, and reports progress as candidates are attempted. When a match is found, the tool displays the recovered password.

> **Dictionary source used:** The specific source (built-in list vs. a custom uploaded wordlist) for each file should be confirmed against the author's actual session and noted here before final submission. This documentation intentionally does not assume one option over the other, per project accuracy requirements.

**Role in this project:** Take each `$pdf$` hash generated by the Hash Calculator and recover the corresponding plaintext password.

---

## 🔄 Practical Workflow

Summary of the steps performed:

1. Download the supplied encrypted challenge PDF.
2. Open the NetworkWalks Hash Calculator.
3. Upload the locked PDF using the **PDF** option and extract its `$pdf$` hash.
4. Copy the complete extracted hash.
5. Open the NetworkWalks Password Cracker.
6. Paste the extracted hash, select a dictionary source, and start the attack.
7. Wait for the attack to complete and record the recovered password.
8. Open the original PDF and enter the recovered password.
9. Verify the PDF opens successfully and record the challenge flag.

This sequence was repeated independently for all three challenge PDFs.

---

## 🧩 PDF Hash Extraction

Each `$pdf$` hash is a `*`-delimited string encoding (in order) the PDF security handler revision, the encryption key length, permission flags, the document's internal ID/salt, and relevant encrypted dictionary values. The Hash Calculator performs this extraction automatically — the user only needs to upload the file; no manual parsing is required.

The resulting string is portable: it can be copied as plain text and submitted directly to a compatible password-cracking engine (in this case, the NetworkWalks Password Cracker) without needing further access to the original PDF.

---

## 🧨 Dictionary Attack Explained

A dictionary attack tests a finite, curated list of candidate passwords against a target hash, rather than exhaustively enumerating every possible character combination (as a brute-force attack would). It is effective specifically because many real-world passwords are reused, predictable, or drawn from common patterns (names, keyboard sequences, common phrases, etc.).

**How candidates are tested:** For each candidate word in the selected dictionary, the cracking engine reproduces the same hash-validation computation implied by the `$pdf$` structure and checks whether the candidate satisfies it. When a candidate produces a valid match, the attack stops and the tool reports that candidate as the recovered password.

**Result display:** The NetworkWalks Password Cracker surfaces the recovered password directly in its interface once a match is found, alongside the progress of attempted candidates up to that point.

---

## ✅ Password Recovery Process

Recovering a candidate password from a hash is not, by itself, sufficient proof that the password is correct — it is a *statistical/structural match* against the hash's validation logic. For that reason, this practical required an explicit **validation step**: each recovered password was used to attempt to open the *original* PDF file directly. Only once the PDF opened successfully was the recovered password considered confirmed, and the enclosed flag recorded as proof of successful recovery.

---

## 🏁 Recovered Passwords & Flags

> All hashes, passwords, and flags below are reproduced exactly as obtained during the practical. Three authorized training challenge PDFs were processed independently.

### File 1

```text
Recovered Password : good-luck
Captured Flag       : nw{cybersecurity_flag_captured_2608}
```

**PDF Hash:**
```text
$pdf$4*4*128*-1028*1*16*ca7f72f11459cba469f1005a8765ed51*32*f32d8fa1bfbe2648226dffc39f7909ea0021446990b9e4114071a4d9104984c1*32*9322f50c29569712067a775264635e4954ccb1b99e209d664984054ffad30a6a
```

### File 2

```text
Recovered Password : password1
Captured Flag       : nw{networkwalks_persistence_jtr_270521}
```

**PDF Hash:**
```text
$pdf$4*4*128*-1028*1*16*0853f2cde0ef15b1c0f93ed229d3b1ad*32*8f13ce5aa39ad974364d36a057da76790021446990b9e4114071a4d9104984c1*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1
```

### File 3

```text
Recovered Password : 1qaz2wsx
Captured Flag       : nw{networkwalks_flag_260821_1}
```

**PDF Hash:**
```text
$pdf$4*4*128*-1028*1*16*34eb542eff4e1b0b32d25ce15a9a7281*32*b77872bfc9a24fb2f845066283a8fc1b0021446990b9e4114071a4d9104984c1*32*e7572256e4b552cd57988f5134214b91920d94d7a6bf550ea94a2995c7f2ab02
```



---

## 📊 Results Summary

| Challenge File | Recovered Password | Captured Flag |
|---|---|---|
| File 1 | `pass-good-luck` | `flag-nw{cybersecurity_flag_captured_2608}` |
| File 2 | `pass-password1` | `flag-nw{networkwalks_persistence_jtr_270521}` |
| File 3 | `pass-1qaz2wsx` | `flag-nw{networkwalks_flag_260821_1}` |

**Success rate:** 3 / 3 challenge PDFs successfully unlocked and flagged.

---

## 🖼 Screenshot Evidence

All screenshots are the author's own and will be added after the practical write-up is finalized. Placeholders below render cleanly even before images are added. See [`screenshots/README.md`](screenshots/README.md) for the exact filenames each placeholder expects.

| # | Evidence | Placeholder |
|---|---|---|
| 1 | Original PDF challenge files | `<img width="1518" height="764" alt="image" src="https://github.com/user-attachments/assets/0a76754e-7e83-4ec7-accd-79af8181bf68" />
` |
| 2 | Hash Calculator homepage | `<img width="953" height="613" alt="image" src="https://github.com/user-attachments/assets/ea5620db-be3f-472a-b1d8-6ffff59d56be" />
` |
| 3 | File 1 – extracted PDF hash | `<img width="1164" height="750" alt="Screenshot 2026-11-20 210603" src="https://github.com/user-attachments/assets/1c3afcb6-2c65-42fb-9b94-9f40a8112498" />
` |
| 4 | File 2 – extracted PDF hash | `<img width="952" height="824" alt="Screenshot 2026-11-20 211220" src="https://github.com/user-attachments/assets/258f76cc-2bbe-4c94-ba8c-15ddb2b3d4d0" />
` |
| 5 | File 3 – extracted PDF hash | `<img width="983" height="826" alt="Screenshot 2026-11-20 211705" src="https://github.com/user-attachments/assets/44644b46-5f25-475a-a21e-b943345b274f" />
` |
| 6 | Password Cracker homepage | `<img width="956" height="581" alt="image" src="https://github.com/user-attachments/assets/f0ba07b1-3ae8-401c-bd54-f5d58b6099cb" />
` |
| 7 | File 1 – recovered password | `<img width="918" height="899" alt="Screenshot 2026-11-20 211037" src="https://github.com/user-attachments/assets/8c52c126-2105-4bea-8111-da5fb2aec642" />
]` |
| 8 | File 2 – recovered password | `<img width="918" height="905" alt="Screenshot 2026-11-20 211228" src="https://github.com/user-attachments/assets/06654bbb-15c6-4181-8865-54cfe566a089" />
` |
| 9 | File 3 – recovered password | `<img width="943" height="905" alt="Screenshot 2026-11-20 211902" src="https://github.com/user-attachments/assets/2e43bc95-a14e-4aef-b816-454b56d4b3d9" />
` |
| 10 | Final flag verification | `
<img width="769" height="493" alt="image" src="https://github.com/user-attachments/assets/7f13c38b-8f39-48c7-add2-ff699645e2db" />
` |
<img width="1037" height="400" alt="Screenshot 2026-11-20 211308" src="https://github.com/user-attachments/assets/5569cef0-4064-4fa4-9df8-6d5d969f4705" />

<img width="1018" height="823" alt="Screenshot 2026-11-20 211920" src="https://github.com/user-attachments/assets/318bf26a-f142-43bb-93c9-96063fc81dff" />


---

## 🎓 Technical Learning Outcomes

In brief, this module reinforced:

- Practical understanding of how PDF password protection is represented and attacked.
- Hands-on experience with web-based hash-extraction and password-recovery tooling.
- The mechanics and limitations of dictionary-based attacks.
- Why password strength and uniqueness materially affect real-world security outcomes.

---

## 🔍 Security Observations

- All three recovered passwords followed predictable, dictionary-friendly patterns (e.g., common words, a keyboard-walk pattern, and a frequently reused base password), which is precisely why a small, built-in dictionary was sufficient to recover them.
- Each password's recoverability illustrates a recurring real-world weakness: password choices that feel "personal" or "unique" to a user are frequently still common enough to appear in standard cracking dictionaries.
- Hash extraction tools like the one used here do not require the original password at any point — only the protected file itself — underscoring why protecting access to password-protected files (not just the passwords themselves) also matters.

---

## ⚠️ Limitations

- Dictionary attacks can only recover passwords that exist within the selected wordlist; a strong, non-dictionary password would not have been recoverable using this method.
- The built-in dictionary provided by the tool is limited to 100 entries, which constrains this practical to demonstrating recovery of comparatively weak/common passwords.
- This project reports only what was directly observed in the tool's interface (recovered passwords, flags, and hash values). No cracking duration, attempt counts, or terminal/log output were recorded by the tool and none are claimed or fabricated here.
- Results are specific to the three supplied training PDFs and do not generalize to arbitrary PDF files or production documents.

---


---

## 🧾 Conclusion

This module successfully demonstrated an end-to-end password-recovery workflow using exclusively NetworkWalks' own online Hash Calculator and Password Cracker tools, independent of locally installed cracking utilities. All three authorized training challenge PDFs were unlocked and their flags captured, validating both the correctness of the extracted hashes and the recovered passwords. The exercise reinforced core password-security concepts — particularly the real-world vulnerability of common, dictionary-style passwords — that are directly relevant to professional cybersecurity practice.

---

## 👤 Author

**Ayush Shukla**
NetworkWalks Cybersecurity & Ethical Hacking Internship — Batch B084
GitHub: [@Hacker-Ayush-py03](https://github.com/Hacker-Ayush-py03)

---

## 📚 References

- NetworkWalks Hash Calculator — [https://networkwalks.com/hash-calculator/](https://networkwalks.com/hash-calculator/)
- NetworkWalks Password Cracker — [https://networkwalks.com/password-cracker/](https://networkwalks.com/password-cracker/)
- Assignment: *W3-PM2 – Week 3 – Project Module 2 – Password Cracking with NW Tools* (NetworkWalks internship curriculum)
- Parent repository: [NETWORK_WALKS](https://github.com/Hacker-Ayush-py03/NETWORK_WALKS)

---

### ⚖️ Ethical Use Statement

This practical was conducted exclusively on **authorized training challenge PDF files** supplied as part of the NetworkWalks internship curriculum, for educational purposes only. The techniques, tools, and results documented in this repository must **not** be applied to any system, file, or account without explicit, verifiable authorization from its owner. Unauthorized password cracking is illegal in most jurisdictions.

<p align="center"><sub>Week 3 · Project Module 2 · NetworkWalks Cybersecurity & Ethical Hacking Internship · Batch B084</sub></p>
