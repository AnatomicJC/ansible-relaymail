Relaymail
=========

This role will configure postfix to send mails through a smarthost

Why shouldn't I use ssmtp, isn't it easier to setup?
=========

Source of this explanation: https://github.com/Yannik/ansible-role-relaymail

I actually believe that this role makes it even easier to setup postfix than ssmtp.

This is what I found out when I installed ssmtp myself:

>I wanted to use ssmtp today too, but noticed that it does NOT verify the SSL/TLS certificate of the remote server on the current debian & ubuntu releases and also does NOT verify the hostname of the certificate. This is a major issue, as this effectively renders the encryption useless and your password is being transmitted alike to being plaintext and anyone can sniff it. This has also been reported in a debian bug, but there has not been any progress for years: [https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=662960](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=662960)

>The ssmtp version in the Redhat packages has been patched to atleast verify the certificate, but the hostname is still NOT being verified and the encryption is therefore as insecure as on debian/ubuntu. There is a bug for this, but there is also no progress for years: [https://bugzilla.redhat.com/show_bug.cgi?id=864894](https://bugzilla.redhat.com/show_bug.cgi?id=864894)

>So, if you care about the security of the email account you use for your servers outgoing emails, do NOT use ssmtp.

>ssmtp has had no active development since atleast 2009: [https://anonscm.debian.org/gitweb/?p=ssmtp/ssmtp.git](https://anonscm.debian.org/gitweb/?p=ssmtp/ssmtp.git)

In addition to these points, any user that can send mails over ssmtp needs read-access to the ssmtp config file which includes the username and password used for smtp auth.
In normal conditions, you would probably give read permission to 'other', which would mean that for every single user/service on that system could read your smtp credentials.

This is not the case with the security-focused design of postfix.

Role Variables
--------------

- `relaymail_root_aliases`: mail addresse(s) who will recieve mails for root user (separated by comma)
   - Example: `user@domain.com`
- `relaymail_smtp_server`: SMTP Server FQDN
   - Example: `smtp.gmail.com`
- `relaymail_smtp_port`: SMTP Server port, usually 25 or 587
   - Example: `25`
- `relaymail_smtp_user`: SMTP Server user for authentication
   - Example: `user@domain.com`
- `relaymail_smtp_password`: SMTP Server user password
   - Example: `password`
- `relaymail_secure_tls`: If True, SMTP Relay must have SSL certificate configured (to be activated if you used gmail account)
   - Values: False (default) / True
- `relaymail_use_tls`: Connect to SMTP Server through TLS ?
   - Values: False (default) / True
- `relaymail_main_domain`: main domain who will be configured in /etc/postfix/main.cf
   - Example: domain.com

Example Playbook
----------------

    - hosts: all
      roles:
         - { role: AnatomicJC.relaymail }

License
-------

WTFPL

Author Information
------------------

Look at the commits
