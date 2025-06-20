# Path Traversal – PortSwigger Labs

This folder contains my solutions and learning notes for **Path Traversal labs** on the [PortSwigger Web Security Academy]([https://portswigger.net/web-security/file-path-traversal](https://portswigger.net/web-security/all-labs#path-traversal)).

## ✅ Labs Completed

- File path traversal, simple case
- File path traversal, traversal sequences blocked with absolute path bypass
- File path traversal, traversal sequences stripped non-recursively
- File path traversal, traversal sequences stripped with superfluous URL-decode
- File path traversal, validation of start of path
- File path traversal, validation of file extension with null byte bypass

## 🧠 Key Takeaways

- Basic payloads like `../../etc/passwd` can reveal sensitive files
- URL encoding (`%2e%2e%2f`) or double URL encoding can bypass filters
- Null byte injection (`%00`) helps in some file extension checks
- Always check how the server processes and normalizes file paths

---

📌 These write-ups are for educational purposes only.

