>> ffuf-cheat-sheet
------
>> 👤 Author

**NIGHTFURY0X01 (Arash)**

Aspiring Red Team Operator | Offensive Security & CTF Player
-----

## 📌 About

A quick reference cheat sheet for FFUF (Fuzz Faster U Fool)

------

# 👾 FFUF Usage Examples Cheat Sheet 👾

---

## 1. Directory & File Discovery

```bash
ffuf -u https://target.com/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

* Replaces `FUZZ` with each word in the list.

---

## 2. File Extension Brute-Force

```bash
ffuf -u https://target.com/indexFUZZ -w /usr/share/wordlists/extensions_common.txt -e .php,.asp,.aspx,.jsp,.html,.bak
```

* Tries multiple extensions for hidden files.

---

## 3. Subdomain Fuzzing

```bash
ffuf -u https://FUZZ.target.com/ -w /usr/share/wordlists/subdomains-top1million-5000.txt -H "Host: FUZZ.target.com"
```

* Tests for subdomains by fuzzing the `Host` header.

---

## 4. Virtual Host (VHOST) Discovery

```bash
ffuf -u http://target.com/ -w /usr/share/wordlists/subdomains.txt -H "Host: FUZZ.target.com"
```

* Useful when domains share the same IP.

---

## 5. Parameter Fuzzing (GET)

```bash
ffuf -u "https://target.com/page.php?FUZZ=test" -w /usr/share/wordlists/params.txt
```

* Finds hidden GET parameters.

---

## 6. Parameter Value Fuzzing

```bash
ffuf -u "https://target.com/login.php?user=admin&pass=FUZZ" -w /usr/share/wordlists/rockyou.txt
```

* Brute-forces parameter values (e.g., weak passwords).

---

## 7. POST Data Fuzzing

```bash
ffuf -u https://target.com/login -w /usr/share/wordlists/rockyou.txt -X POST -d "username=admin&password=FUZZ" -H "Content-Type: application/x-www-form-urlencoded"
```

* Tests login forms.

---

## 8. Multiple Wordlists (Recursive FUZZing)

```bash
ffuf -u https://target.com/FUZZ/FUZZ2 -w wordlist1.txt:FUZZ -w wordlist2.txt:FUZZ2
```

* Fuzzes two positions at once.

---

## 9. Filtering Results

Filter by status code:

```bash
ffuf -u https://target.com/FUZZ -w common.txt -fc 404
```

Filter by size:

```bash
ffuf -u https://target.com/FUZZ -w common.txt -fs 0
```

---

## 10. Rate Limit Control (Stealth Mode)

```bash
ffuf -u https://target.com/FUZZ -w common.txt -rate 50
```

* Sends **50 requests per second** (good for avoiding bans).

---

## 11. Recursive Directory Discovery

```bash
ffuf -u https://target.com/FUZZ -w common.txt -recursion -recursion-depth 2
```

---

## 12. JSON API Fuzzing

```bash
ffuf -u https://target.com/api -w params.txt -X POST -d '{"FUZZ":"test"}' -H "Content-Type: application/json"
```

---

## 13. Detecting Hidden Parameters via Reflection

```bash
ffuf -u "https://target.com/page.php?FUZZ=ffuf123" -w params.txt -mr "ffuf123"
```

* Matches reflected values in responses (useful for XSS/IDOR).

---

## 14. Fuzzing Cookies

```bash
ffuf -u https://target.com/dashboard -w wordlist.txt -H "Cookie: session=FUZZ"
```

---

## 15. Advanced Match & Filter

Match by response words:

```bash
ffuf -u https://target.com/FUZZ -w common.txt -mw 10
```

Match regex:

```bash
ffuf -u https://target.com/FUZZ -w common.txt -mr "admin|root"
```

---

⚡ With these, you can cover:

* Directories/files
* Subdomains & VHosts
* Parameters (GET/POST)
* API fuzzing
* Hidden cookies & headers

---


# 🔥 Advanced FFUF Examples

---

## 🎯 Directory & File Discovery (Tricks)

**1. Recursive with custom extensions**

```bash
ffuf -u https://target.com/FUZZ -w common.txt -recursion -recursion-depth 3 -e .php,.bak,.old,.zip
```

**2. Brute-force backup files**

```bash
ffuf -u https://target.com/FUZZ -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-medium-words.txt -e .bak,.old,.zip,.tar.gz,.7z
```

**3. Case sensitivity check**

```bash
ffuf -u https://target.com/FUZZ -w common.txt -ic
```

(`-ic` = ignore case)

---

## 🌍 Subdomain / VHOST

**4. Subdomain fuzz with HTTPS**

```bash
ffuf -u https://FUZZ.target.com/ -w subdomains.txt
```

**5. VHOST fuzzing with IP**

```bash
ffuf -u http://192.168.1.10/ -w subdomains.txt -H "Host: FUZZ.target.com"
```

**6. Hidden admin panels**

```bash
ffuf -u https://target.com/FUZZ -w panels.txt -mc 200,301
```

---

## 🔑 Parameter Hunting

**7. GET parameter brute-force**

```bash
ffuf -u "https://target.com/index.php?FUZZ=1" -w params.txt
```

**8. POST parameter brute-force**

```bash
ffuf -u https://target.com/api -X POST -w params.txt -d "FUZZ=value" -H "Content-Type: application/x-www-form-urlencoded"
```

**9. JSON parameter fuzzing**

```bash
ffuf -u https://target.com/api -X POST -w params.txt -d '{"FUZZ":"123"}' -H "Content-Type: application/json"
```

**10. Reflection detection (useful for XSS/IDOR)**

```bash
ffuf -u "https://target.com/page.php?FUZZ=ffuf123" -w params.txt -mr "ffuf123"
```

---

## 🧑‍💻 Login / Auth Fuzzing

**11. Password brute-force**

```bash
ffuf -u https://target.com/login -w rockyou.txt -X POST -d "username=admin&password=FUZZ" -H "Content-Type: application/x-www-form-urlencoded" -fc 302
```

**12. Username enumeration**

```bash
ffuf -u https://target.com/login -w users.txt -X POST -d "username=FUZZ&password=test123" -H "Content-Type: application/x-www-form-urlencoded" -fr "Invalid username"
```

**13. Session hijacking via cookie**

```bash
ffuf -u https://target.com/dashboard -w session_tokens.txt -H "Cookie: session=FUZZ" -mc 200
```

---

## 📡 API Testing

**14. Fuzzing REST endpoints**

```bash
ffuf -u https://target.com/api/FUZZ -w apis.txt -mc 200
```

**15. GraphQL hidden fields**

```bash
ffuf -u https://target.com/graphql -X POST -w graphql-fields.txt -d '{"query":"{FUZZ}"}' -H "Content-Type: application/json"
```

**16. HTTP methods fuzz**

```bash
ffuf -u https://target.com/endpoint -w methods.txt -X FUZZ -mc 200,403,500
```

(try PUT, DELETE, OPTIONS, etc.)

---

## 🕵️ Filtering / Matching

**17. Match by size**

```bash
ffuf -u https://target.com/FUZZ -w common.txt -fs 4242
```

**18. Match by regex**

```bash
ffuf -u https://target.com/FUZZ -w common.txt -mr "root|admin|dashboard"
```

**19. Filter by response time**

```bash
ffuf -u https://target.com/FUZZ -w common.txt -ft 2
```

**20. Filter by response words**

```bash
ffuf -u https://target.com/FUZZ -w common.txt -fw 0
```

---

## 🚀 Performance / Stealth

**21. Low & slow mode (avoid detection)**

```bash
ffuf -u https://target.com/FUZZ -w common.txt -rate 10
```

**22. Multithreading (faster scans)**

```bash
ffuf -u https://target.com/FUZZ -w big.txt -t 200
```

---

## ⚡ Extra Tricks

**23. Fuzz headers**

```bash
ffuf -u https://target.com/ -w headers.txt -H "FUZZ: test"
```

**24. Fuzz file uploads**

```bash
ffuf -u https://target.com/upload.php -w extensions.txt -X POST -d "file=@testFUZZ" -H "Content-Type: multipart/form-data"
```

**25. Content discovery inside JS files**

```bash
ffuf -u https://target.com/FUZZ.js -w js_wordlist.txt -mc 200
```

**26. Recursive API fuzzing**

```bash
ffuf -u https://target.com/api/FUZZ -w endpoints.txt -recursion -recursion-depth 2
```

---

Happy hacking 👾



