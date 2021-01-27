# Note:  I have not taken this exam yet so there's absolutely nothing in this document that has anything to do with the exam.  I am bound by a non-disclosure agreement so please don't ask me anything about the exam.


# Things to Remember:
Installation:

Always install iDM on a clean system that has no DNS kerberos or Directory server instances.
Disable name service cache daemon! (iDM uses SSSD caching mechanisms)
IPv6 *must* be enabled but you don't have to use ipv6 addresses.
DNS resolution (ie client)must be working and pointing to a dns server that will resolve the names 
$(hostname) must not resolve to localhost or localhost6
$(hostname) must resolve to the FQDN of the server

firewall must allow 80, 443, 389, 636, 88, 464, 53(tcp) and 88, 464, 53, 123 (UDp)
Add these services with:
firewall-cmd --add-service freeipa-ldap --add-service freeipa-ldaps



klist -kt /etc/krb5.keytab

klist -e (encrytpion algorithm)
yum install ipa-client (very important)

yum install httpd mod_auth_kerb.x86_64

/usr/share/doc/mod_auth_kerb/example.conf for an example 

ipa service-show
You must have a valid KRB ticket to perform the following steps (kinit admin)
ipa-getkeytab     minimums: server connecting to, pricipal name, and keytab to write to.
ipa-getkeytab -s idm.lab.example.net -p HTTP/client.lab.example.net -k /etc/httpd/conf/demo.keytab


ensure that /etc/krb5.conf "includedir" doesn't reference something that doesn't exist.
ensure /etc/krb5.conf has [domain_realm] reference for the machine you're on.
ensure you are pointing to a dns server so that you can look up the KDC


ensure host name in /etc/hosts is NOT in "localhost" or "127.0.0.1".  Make sure the host name has a real IP.


add freeipa-ldap, freeipa-ldaps and dns services to firewall

firewall-cmd --add-service=freeipa-ldap --permanent
firewall-cmd --add-service=freeipa-ldaps --permanent
firewall-cmd --add-service=dns --permanent
firewall-cmd --reload


Ensure local IDM server has forward and reverse DNS addresses.

Verify host name is in /etc/hosts with correct IP addresses


yum install ipa-server ipa-server-dns -y

Errors about AAAA DNS records are OK to ignore.

> ipa-service-install

  --no-ntp # use when chrony is already installed
  # must specify forwarders for DNS
  # setup AD trust after installation
  
  
  
  
  ipactl status
  ipa
  klist
  kinit 
  
  # Chapter 2:
  
  Password policies do not become active retroactively.  Only when users change their passwords are new settings enforced.
  
  User account state:  Staged (cannot yet login), Active (can login), Preserved (inactive and cannot authenticate)
  
  ipa stageuser-*
  
  ## Manaing IDM Clients
  yum install ipa-client
  
  journalctl -u named-pkcs11.service
  ipa-join --unenroll # Unenrolls from the domain.
  
  
  ipa host-add client.example.com --random
  
  ipa host-show <client registered>
  
  ## Programming with the Management Interface
  
  First POST a request to get an authentication token
  
  https:<url>/ipa/session/login_kerberos
  https:<url>/ipa/session/login_password
  
  You can find the URL with "ipa -v user-show <valid_user>"  Use multple -vvv for more verbosity.
  
  
  
  curl XXXXXXXX |  python -m json.tool
  # Chapter 3: Authenticating Identities with Kerberos 
  
  
  
  kinit works behind the scenes 
  
  ldap:  uid, gid, home dirs
  klist
  
  
  When logging in for the first time, Kerberos (kinit) happens automatically behind the scenens when you log in.
  

Chapter 4

ipa-getcert
 
 
 
 
 
