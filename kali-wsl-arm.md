# Kali Linux on WSL (Windows on ARM)

Notes from setting up Kali Linux inside WSL on a Snapdragon X Plus (ARM64) laptop, alongside my VirtualBox Kali VM.

## Why WSL Kali?
- Starts in seconds, no full VM needed
- Runs **natively on ARM64** — no emulation
- Good for command-line tools (Nmap, Metasploit, John, sqlmap)
- Use the VirtualBox Kali for GUI tools (Burp Suite, Wireshark GUI)

## Install
1. Install **Kali Linux** from the Microsoft Store.
2. Install WSL itself (PowerShell):
   ```powershell
   wsl --install --no-distribution
   ```
3. Restart Windows, open **Kali** from the Start menu and create a UNIX username + password.

## Install the tools (no desktop)
```bash
sudo apt update && sudo apt install -y kali-linux-headless
```
`kali-linux-headless` = the default Kali toolset without the GUI. Good fit for WSL.

Setup prompts I got during install:
| Prompt | Answer | Why |
|---|---|---|
| Kismet setuid root | No | WSL can't reach the Wi-Fi card anyway |
| macchanger auto-change MAC | No | Not needed; run manually if required |
| sslh: inetd or standalone | standalone | Not used; either is fine |
| Restart services without asking | Yes | Avoids the prompt on every upgrade |

## Verify
```bash
nmap --version          # Nmap 7.99, aarch64
msfconsole --version    # Framework 6.5.x
john --help | head -3   # John the Ripper jumbo, aarch64
sqlmap --version
nmap localhost          # first safe scan: my own machine
```

## ARM limitations to remember
- x86-only VMs (Metasploitable, most VulnHub boxes) won't run on ARM
- No NVIDIA GPU → no CUDA; Hashcat is slow → use Google Colab for GPU work
- Kernel anti-cheat / some x86-only tools won't work
- Workaround: TryHackMe AttackBox / Hack The Box labs run in the browser

## Handy
```powershell
wsl --shutdown          # free RAM when done
wsl -l -v               # list distros + WSL version
```

> Only scan systems I own or have permission to test (my own machine, class labs, THM/HTB).
