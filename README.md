# Pi isc-dhcp-server

## dhcpd.conf setting

### Get isc-package 
> sudo apt-get install isc-dhcp-server

### Tftp address format
option vivso <entreprise number> <length of the vivso TLV contents> <option type> <option length> <option value>...
So, for one TFTP address it can be substituted as follows:

* Entreprise Number = 4491 (entreprise number of CableLabs, in dotted hex: 00:00:11:8b)
* option type = 2 (CL_V4OPTION_TFTPSERVERS)
* option length = 4 (bytes in an IP address)
* option value = IP address in hex (in our example 10.16.0.4 would be 0a:10:00:04)
* length of the vivso TLV contents = 6 (1 byte of type, 1 byte of length and 4 bytes of data)

As our tftp server 192.168.143.249, it would be c0.a8.8f.f9 

> option vivso 00:00:11:8b:06:02:04:c0:a8:8f:f9 ;


### subnet for modem

```
 subnet 192.168.143.0 netmask 255.255.255.0 {
    range 192.168.143.0 netmask 255.255.255.0;
    option domain-name-servers 192.168.143.249;
    option domain-name "pi.com";
    option routers 192.168.143.248; 
    option boradcast-address 192.168.143.255;
    default-lease-time 600;
    max-lease-time 7200;
    filename "cm.cfg";
    option bootfile-name "cm.cfg";
    option docsis-mta.dhcp-server-1 192.168.143.249;
 }
```

## TFTPD setting
> sudo apt-get install tftpd

> sudo apt-get install tftp

> sudo apt-get install xinetd


/etc/xinetd.d/tftp
```
service tftp
{
    socket_type = dgram
    protocol    = udp
    port        = 69
    wait        = yes
    user        = root
    server      = /usr/sbin/in.tftpd
    server_args = /tftpboot/ -s
    isable      = no
    flags       = IPv4
}
```