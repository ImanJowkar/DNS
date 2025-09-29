# DNS

mail.google.com.
               ^ root
            ----
            TLD
    -------------
    2nd level domain
-------------------
sub-domain

[ref](iana.org)

[ref2](root-servers.org)


## Client package for test the dns server
```sh

# on debian
dnf install bind-utils

# on rhel 
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




# work with nslookup
nslookup cisco.com
nslookup cisco.com 8.8.8.8

nslookup -type=ns cisco.com
nslookup -all
```

```sh

apt install host
apt install bind9 bind9-doc

# you can see the cache in the cache only dns server
rndc dumpdb -cache
rndc flush

 

# below version of dig hase a bug
dig -v
DiG 9.18.30-0ubuntu0.24.04.2-Ubuntu

sudo apt remove --purge bind9-dnsutils

sudo apt update

sudo apt install bind9-dnsutils
dig +trace yahoo.com +nodnssec

```


[download from here](https://www.isc.org/download/#BIND)
## Setup Bind in ubuntu 24 

```sh

sudo add-apt-repository ppa:isc/bind
sudo apt update

sudo apt install bind9 bind9-dnsutils bind9-doc

# named -v
# BIND 9.18.39-0ubuntu0.24.04.1-Ubuntu (Extended Support Version) <id:>

named -v
BIND 9.20.13-1+ubuntu24.04.1+deb.sury.org+1-Ubuntu (Stable Release) <id:>



cd /etc/bind

```
[option block] (https://bind9.readthedocs.io/en/v9.20.13/reference.html#options-block-grammar)
# Basic configuration
```sh
vim /etc/bind/named.conf.options
---------------------
acl allowedclients {
        192.168.96.0/24;
};


options {
        directory "/var/cache/bind";
        dnssec-validation no;
        //listen-on-v6 { any; };
        listen-on { 192.168.96.70; };
        recursion yes;
        allow-query { allowedclients; };
        version none;
        hostname none;

};

----------------------
systemctl restart named
systemctl cat named


dig version.bind  TXT  CH

named-checkconf

```
## service management

```sh
rndc status
rndc zonestatus localhost
rndc reload 
rnds stats  
rndc dumpdb -cache    # store a file in directory  directory "/var/cache/bind";

rndc flushname yahoo.com   # remove yahoo.com from cache




```
## Tunning bind 

```sh

# set log
mkdir /var/log/named
cd /var/log/
chown bind:bind named
systemctl restart named

vim /etc/bind/named.conf.options
---------------------
acl allowedclients {
        192.168.96.0/24;
};

logging {
        channel named_log {
         file "/var/log/named/named.log" versions 3 size 5m;
         severity info;
         print-category yes;
         print-severity yes;
         print-time yes;
        };

        category security { named_log; };
        category queries { named_log; };
        category zoneload { named_log; };
        category query-errors { named_log; };
        category resolver { named_log; };
        category network { named_log; };
        category update { named_log; };
        category serve-stale { named_log; };
};



options {
        directory "/var/cache/bind";
        dnssec-validation no;
        //listen-on-v6 { any; };
        listen-on { 192.168.96.70; };
        recursion yes;
        allow-query { allowedclients; };
        version none;
        hostname none;

};

----------------------
systemctl restart named



# set recursive-clients correctly
vim bulk.txt
----------------
google.com A
faradars.org A
imdb.com A
cisco.com A
fortigate.org A
f5.com A
ebay.com A
openai.com A
reddit.com A
wikipedia.org A
maktabkhoone.org A
github.com A
microsoft.com A
----------------


apt install dnsperf
dnsperf -s 192.168.96.70 -v -d bulk.txt

options {
        recursive-clients 10;


};

```