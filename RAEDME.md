# DNS

mail.google.com.
               ^ root
            ----
            TLD
    -------------
    2nd level domain
-------------------
sub-domain


## Client package for test the dns server
```sh

# on rhel
dnf install bind-utils

# on debian 
apt install dnsutils



# usefull cmds
dig google.com
dig google.com +short

dig faradars.org +noall +answer +stats

dig cisco.com soa
dig cisco.com ns
dig . ns

dig com ns
dig cisco.com txt

dig cisco.com mx

dig @4.2.2.4 cisco.com

dig -x 72.163.4.185 # get PTR records


# obtain the version of dns server
dig version.bind  TXT -c CH @ns1.google.com
dig version.bind  TXT -c CH @ligia.ns.cloudflare.com
dig version.bind  TXT -c CH @ns0.dnsmadeeasy.com



nslookup cisco.com
nslookup cisco.com 8.8.8.8

nslookup -type=ns cisco.com
nslookup -all
```

```sh

apt install host
apt install bind9 bind9-doc





```