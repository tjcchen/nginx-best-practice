## Domain

### Common Records Explanation

A record: mapping a domain to an IP address.

CNAME: mapping domain to other domains, which is equivalent to domain alias.

NS: DNS server analysis

MX( mail exchange ): Email services, eg: SMTP, POP3


### Generic Domain Record

A record: *.tjcchen.org - (whatever.tjcchen.org can assess your website)

Use cases: multiple users application, eg: james.tjcchen.org

A record: tjcchen.org - (with sub domain name left blank)

Use cases: http://tjcchen.org


### Multiple Port Access

To allow multiple ports access, like http://35.172.190.158:8080/ or http://35.172.190.158:8081/,
We need to open port support on host's specific security group.
