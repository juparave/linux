# DKIM

Following steps from https://askubuntu.com/questions/438756/using-dkim-in-my-server-for-multiple-domains-websites

### Add TXT record to domain

After creating new keys, copy the value from: `cat /etc/opendkim/keys/otherdomain.com/default.txt` and add it to the DNS
as a TXT record

|Type|Hostname|Value|
|---|---|---|
|TXT|default._domainkey|v=DKIM1;h=sha256;k=rsa;s=email;p=MIIBIjANB...AQIDAQAB|

Verify DKIM Record on: https://easydmarc.com/tools/dkim-lookup

### Helper script to configure multiple domains

```bash
#!/bin/bash
# List of domains
domains=( 
        'example.com'
)
# file paths and directories
dkim="/etc/opendkim"
keys="$dkim/keys"
keyfile="$dkim/KeyTable"
signfile="$dkim/SigningTable"
trustfile="$dkim/TrustedHosts"
spffile="$dkim/spfs.txt"
# Set Ipv6 and Ipv4 addresses for the server here
ipv4=""
ipv6=""
# loopback addresses for the server
loop=( localhost 127.0.0.1 )
function loopback {
        for back in "${loop[@]}"
        do
                if ! grep -q "$back" "$trustfile"; then
                        echo "$back" >> "$trustfile"
                fi
        done
}
# Check for files and create / write to them if they dont exist
if [ ! -d "$keys" ]; then
        mkdir "$keys"
fi
if [ ! -f "$keyfile" ]; then
        touch "$keyfile"
fi
if [ ! -f "$signfile" ]; then
        touch "$signfile"
fi
if [ ! -f "$trustfile" ]; then
        touch "$trustfile"
        loopback
else
        loopback
fi
if [ ! -f "$spffile" ]; then
        touch "$spffile"
else
        rm -rf "$spffile"
        touch "$spffile"
fi
if [ ! -z "$ipv6" ]; then
        if ! grep -q "$ipv6" "$trustfile"; then
                echo "$ipv6" >> "$trustfile"
        fi
fi
if [ ! -z "$ipv4" ]; then
        if ! grep -q "$ipv4" "$trustfile"; then
                echo "$ipv4" >> "$trustfile"
        fi
fi
# Generate keys and write the spfs records we need for each domain to one file
for domain in "${domains[@]}"
do
        keydir="$keys/$domain"
        default="$keydir/default.txt"
        if [ ! -d "$keydir" ]; then
                mkdir $keydir
        fi
        cd $keydir
        opendkim-genkey -r -d $domain
        chown opendkim:opendkim default.private
        key="default._domainkey.$domain $domain:default:$keydir/default.private"
        sign="$domain default._domainkey.$domain"
        trust="$domain"
        spf="$(cat $default)"
        # Check only the last line against the spf file as the first line is always the same
        spflast="$(tail -1 $default)"
        if ! grep -q "$key" "$keyfile"; then
                echo "$key" >> "$keyfile"
        fi
        if ! grep -q "$sign" "$signfile"; then
                echo "$sign" >> "$signfile"
        fi
        if ! grep -q "$trust" "$trustfile"; then
                echo "$trust" >> "$trustfile"
        fi
        if ! grep -q "$spflast" "$spffile"; then
                echo "$spf" >> "$spffile"
        fi
done
```
