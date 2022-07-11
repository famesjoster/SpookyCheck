# SpookyCheck
Check Michael's exclusive Lemax items

Crontab:
```
*/10 * * * * (echo "$(date)"; /home/pi/bin/spookycheck.sh "https://www.michaels.com/lemax-the-future-looks-dark/10692284.html") | tee -a /var/log/spookycheck.log
```

Script:
```
#!/bin/bash
#set -x

URL=$1

if [[ -z $URL ]]; then echo "URL DUMBASS"; exit 1; fi

if [[ $(curl -ks "$URL" | grep -q out-of-stock; echo $?) -eq 0 ]]; then
        echo -e "OUT OF STOCK $URL"
else
        echo -e "IN STOCK\n $URL" | msmtp -a default <email address>
        echo -e "IN STOCK\n $URL" | msmtp -a default <email address>
        echo -e "IN STOCK $URL"
fi
```

msmtp config:
```
defaults
port 587
tls on
tls_trust_file /etc/ssl/certs/ca-certificates.crt
logfile -

account gmail
host smtp.gmail.com
from <gmail address>
auth on
user <gmail username, no @>
password <app password for gmail>
#passwordeval gpg --no-tty --quiet --decrypt ~/.msmtp-gmail.gpg

# Set a default account
account default : gmail
```
