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
IMG_DATE="$(date +%Y-%m-%d:%H:%M:%S)" && /usr/local/bin/imagesnap /tmp/$IMG_DATE.jpg > /dev/null && /usr/local/bin/mpack -s "OSX usage: $(date)" /tmp/$IMG_DATE.jpg destination@gmail.com
```

At this point you should recieve mail with img.

## Hook up at script login

1. Start Automator.app
2. Select "Application"
3. click "Show library" in the toolbar (if hidden)
4. Add "Run shell script" (from the Actions/Utilities)
5. Copy&paste your script into the window
6. Test it
7. Save somewhere, for example you can make an "Applications" folder in your HOME (you will get an your_name.app)
8. Go to System Preferences -> Accounts -> Login items
9. Add this app

## DEBUG:

### Checking mail queue

`mailq`

### Inspecting logs

`tail -f /var/log/mail.log`

### Flush mail queue

`sudo postsuper -d ALL`



