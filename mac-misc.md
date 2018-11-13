# Miscellaneous Stuff for the Mac

## Port Forwarding

### Display Current Port Forwarding Rules

`sudo pfctl -s nat`

### Remove Port Forwarding

`sudo pfctl -F all -f /etc/pf.conf`

### Add Port Forwarding Rules

To forward requests to the local machine (by IP or host name) on port 80 to 127.0.0.1 on port 8080 and
equests to the local machine (by IP or host name) on 443 to 127.0.0.1 on port 8443. Edit these rules as 
nessesary.

```
echo "
rdr pass inet proto tcp from any to any port 80 -> 127.0.0.1 port 8080
rdr pass inet proto tcp from any to any port 443 -> 127.0.0.1 port 8443
" | sudo pfctl -ef -
```
