# MacSnap
Mac OS invalid login snapping

## Setup mail

Create email with gmail.

1. `sudo vi /etc/postfix/main.cf`
paste:
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
2. `sudo vi /etc/postfix/sasl_passwd`
paste:
```
smtp.gmail.com:587 your_email@gmail.com:your_password
```
3. `sudo postmap /etc/postfix/sasl_passwd`
4. `sudo postfix reload`
5. `date | mail -s testing destination@gmail.com`

At this point you should recieve mail at your destination email.

## Install required utils

1. `brew install imagesnap`
2. `brew install mpack`


## Test sending snap:
```
/usr/local/bin/imagesnap /tmp/photo.jpg > /dev/null && /usr/local/bin/mpack -s "OSX usage: $(date)" /tmp/photo.jpg destination@gmail.com
```

At this point you should recieve mail with img



