
## Pseudonym

[NameCheckup](https://namecheckup.com/) find available social media username and domain for your startup, business, personal brand.

## Email Addresses

### Through OSINT

[MOSINT](https://github.com/alpkeskin/mosint) is an OSINT Tool for emails. It helps you gather information about the target email.

Features:

- Verification Service: Check if email exist
- Check social accounts with Socialscan
- Check data breaches
- Find related emails *[need API]*
- Find related phone numbers
- Find related domains
- Scan Pastebin Dumps
- Google Search
- DNS Lookup

```bash
python3 mosint.py
```

[holehe](https://github.com/megadose/holehe) allows you to check if the mail is used on different sites like twitter, instagram and will retrieve information on sites with the forgotten password function.

```bash
holehe email@local.com
```

## URL

### Through OSINT

[Unfurl](https://dfir.blog/introducing-unfurl/) takes a URL and expands ("unfurls") it into a directed graph, extracting every bit of information from the URL and exposing the obscured. It does this by breaking up a URL up into components, extracting as much information as it can from each piece, and presenting it all visually.

It can be either by CLI or through graph

```bash
pip install dfir-unfurl
unfurl_cli.py URL --detailed
```

## Internet Protocol address

### Through OSINT

|Charasteristic|IPDump.py|NERD|VirusTotal (Basic)|IPinfo (Free)|
|-|-|-|-|-|
|**Rate** (Query per second)|45/min|1/sec|500/day|1000/day
|Abuse|:x:|:x:|:x:|:white_check_mark:|
|Autonomous System Number|:white_check_mark:|:white_check_mark:|:white_check_mark:|:white_check_mark:|
|Autonomous System Label|:white_check_mark:|:white_check_mark:|:white_check_mark:|:white_check_mark:|
|Classless Inter-Domain Routing||:white_check_mark:|:white_check_mark:|:white_check_mark:|
|Country|:white_check_mark:|:white_check_mark:|:white_check_mark:|:white_check_mark:|
|DShield|:x:|:white_check_mark:|:x:|:x:|
|IP blacklists|:x:|:white_check_mark:|:white_check_mark:|:x:|
|Reputation|:x:|:white_check_mark:|:white_check_mark:|:x:|
|Reverse DNS Lookup|:white_check_mark:|:white_check_mark:|:white_check_mark:|:white_check_mark:|
|Hosting Provider|:white_check_mark:|:x:|:x:|:white_check_mark:|
|Hosting Location|:white_check_mark:|:x:|:x:|:white_check_mark:|

Collections:

- [DShield](https://isc.sans.edu/howto.html): is a community-based collaborative firewall log correlation system.
&#8594; It receives **logs from volunteers worldwide** and uses them to analyze attack trends through **firewall logs**.
- [UCEPROTECT®-Network Project](http://www.uceprotect.net/en/index.php?m=1&s=0) mail abuse database.
&#8594; UCEPROTECT-Network's core database is fed by a **cluster of more than 50 UCEPROTECT-Servers (Executive-Members)** located in Germany, Austria, Switzerland, Canada and Australia.

[NERD](https://nerd.cesnet.cz/) stands for ‘Network Entity Reputation Database’ and gathers information on IP addresses and keeps track of all kinds of malicious or suspicious activity.

- [Future Misbehavior Probability](https://github.com/CESNET/NERD/wiki/FMP-score): is the estimated probability, that a given IP address (or other entity) will be reported as malicious in a given future time interval,

[IPDump](https://github.com/GlobalThermonuclearWar/IPDump) is python utility for quickly generating reports for IP Addresses, Websites and domains.

```python
./ipdump.py IP -i 
```

[IPInfo](https://github.com/ipinfo/python) is the official Python client library for the IPinfo.io IP address API, allowing you to lookup your own IP address

```python
import ipinfo
access_token = '123456789abc'
handler = ipinfo.getHandler(access_token)
ip_address = '216.239.36.21'
details = handler.getDetails(ip_address)
details.city
'Mountain View'
```

## SSL Certificates

### Through OSINT

[IPDump](https://github.com/GlobalThermonuclearWar/IPDump) is python utility for quickly generating reports for IP Addresses, Websites and domains.

```python
./ipdump.py -s domain.com 
```