This repository contains the ancient patch (dated of April 7, 2003) that
extends Michael Handler's SRV patch to support syntactic sugar NAPTR entries.
These two resource records are used by current DDDS RFCs (340x).

**This is being maintained here just for historic purposes.**

Original README from Dan Bernstein:

----

    djbdns 1.05
    20010211
    Copyright 2001
    D. J. Bernstein
    
    djbdns home page: http://cr.yp.to/djbdns.html
    Installation instructions: http://cr.yp.to/djbdns/install.html

----

Original message posted at dns@list.cr.yp.to:

a) For SRV records, use the following format in tinydns-data (same as
Michael Handler's patch):

    Sfqdn:ip:x:port:weight:priority:ttl:timestamp

"Standard rules for ip, x, ttl, and timestamp apply. Port, weight, and
priority all range from 0-65535. Weight and priority are optional;
they default to zero if not provided."

    Sconsole.zoinks.example.com:1.2.3.4:rack102-con1:2001:69:7:300:

b) For NAPTR records, use the following format:

    Nfqdn:order:pref:flags:service:regexp:replacement:ttl:timestamp

The same standard rules for ttl and timestamp apply. Order and
preference (optional) range from 0-65535, and they default to zero if
not provided. Flags, service and replacement are character-strings.
The replacement is a fqdn that defaults to '.' if not provided.

    Nsomedomain.org:100:90:s:SIP+D2U::_sip._udp.somedomain.org
    Ncid.urn.arpa:100:10:::!^urn:cid:.+@([^\.]+\.)(.*)$!\2!i:

c) This patch makes axfr-get decompose SRV and PTR records and write
them out in native format, rather than opaque (see M.H. patch
comments). This one makes axfr-get convert NAPTR records to the format
explained above.

d) It extends the dnsq and dnsqr to support SRV and NAPTR accordingly.
Example:

    $ dnsqr naptr somedomain.org
    35 somedomain.org:
    x bytes, 1+1+0+0 records, response, noerror
    query: 35 somedomain.org
    answer: somedomain.org 78320 NAPTR 100 90 "s" "SIP+D2U" "" _sip._tcp.somedomain.org
    
    $ dnsqr srv console.zoinks.example.com
    33 console.zoinks.example.com:
    85 bytes, 1+1+0+0 records, response, noerror
    query: 33 console.zoinks.example.com
    answer: console.zoinks.example.com 300 SRV 7 69 2001 rack102-con1.example.com
