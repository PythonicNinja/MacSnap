# MacSnap
Mac OS invalid login snapping

## Requirement
1. gmail account
2. imagesnap (`brew install imagesnap`)
3. mpack (`brew install mpack`)

## Setup mail

1. edit `sudo vi /etc/postfix/main.cf` paste: 
```
mydomain_fallback = localhost
mail_owner = _postfix
setgid_group = _postdrop

#Gmail SMTP
relayhost=smtp.gmail.com:587
# Enable SASL authentication in the Postfix SMTP client.
smtp_sasl_auth_enable=yes
smtp_sasl_password_maps=hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options=noanonymous
smtp_sasl_mechanism_filter=plain
# Enable Transport Layer Security (TLS), i.e. SSL.
smtp_use_tls=yes
smtp_tls_security_level=encrypt
tls_random_source=dev:/dev/urandom
```
2. edit `sudo vi /etc/postfix/sasl_passwd` paste:
```
smtp.gmail.com:587 your_email@gmail.com:your_password
```
3. `sudo postmap /etc/postfix/sasl_passwd`
4. `sudo postfix reload`
5. `date | mail -s testing destination@gmail.com`

At this point you should recieve mail at your destination email.

## Test sending snap:
```
/usr/local/bin/imagesnap /tmp/photo.jpg > /dev/null && /usr/local/bin/mpack -s "OSX usage: $(date)" /tmp/photo.jpg destination@gmail.com
```

At this point you should recieve mail with img.

## DEBUG:

### Checking mail queue

`mailq`

### Inspecting logs

`tail -f /var/log/mail.log`

### Flush mail queue

`sudo postsuper -d ALL`



