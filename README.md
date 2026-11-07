# VeeBase ban list

Addresses that attacked a VeeBase server: at least 5 failed SSH logins in the last 30 days, plus addresses currently
banned by fail2ban or blocked in the firewall. Exported from [VeeBase Console](https://console.veebase.com).

| File | What's in it |
|---|---|
| `banlist.txt` | One address or network per line, `#` comments at the top |
| `banlist.json` | The same addresses with failed-login counts, first and last seen, country and network |

## Using it

**VeeBase Console:** Security page → Banned by fail2ban → **Import**, then choose `banlist.json` or `banlist.txt`.

**fail2ban:**

```bash
curl -s https://raw.githubusercontent.com/Hmzeid/veebase-banlist/main/banlist.txt | grep -v '^#' | xargs -n 100 sudo fail2ban-client set recidive banip
```

**ipset:**

```bash
sudo ipset create veebase-ban hash:net -exist
curl -s https://raw.githubusercontent.com/Hmzeid/veebase-banlist/main/banlist.txt | grep -v '^#' | xargs -I{} sudo ipset add veebase-ban {} -exist
sudo iptables -I INPUT -m set --match-set veebase-ban src -j DROP
```

Addresses change hands; review the list before blocking it permanently. No warranty.
