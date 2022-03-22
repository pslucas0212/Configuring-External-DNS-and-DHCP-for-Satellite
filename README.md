# Configuring External DNS and DHCP for Satellite

[Tutorial Menu](https://github.com/pslucas0212/RedHat-Satellite-VM-Provisioning-to-vSphere-Tutorial)  

### Install and configure DNS and DHCP services to Support Dynamic Updates
See [DNS Dynamic Updating](https://github.com/pslucas0212/DNSUpdating/blob/main/README.md) 

When you first start named, named will generate a rndc key file.  Copy the /etc/rndc.key file from the external DNS server to Satellite Server.  On your Satellite server run the following command.
```
# scp root@ns02.example.com:/etc/rndc.key /etc/rndc.key
```

On your Satellite server onfigure the ownership, permissions, and SELinux context:
```
# restorecon -v /etc/rndc.key
# chown -v root:named /etc/rndc.key
# chmod -v 640 /etc/rndc.key
```

```
# satellite-installer --scenario satellite \
--foreman-initial-admin-username admin \
--foreman-initial-admin-password Passw0rd! \
--foreman-proxy-dhcp true \
--foreman-proxy-dhcp-managed true \
--foreman-proxy-dhcp-gateway "10.1.10.1" \
--foreman-proxy-dhcp-interface "ens192" \
--foreman-proxy-dhcp-nameservers "10.1.10.254" \
--foreman-proxy-dhcp-range "10.1.10.149 10.1.10.199" \
--foreman-proxy-dhcp-server "10.1.10.254" \
--foreman-proxy-dns true \
--foreman-proxy-dns-managed true \
--foreman-proxy-dns-forwarders "10.1.1.254" \
--foreman-proxy-dns-interface "ens192" \
--foreman-proxy-dns-reverse "10.1.10.in-addr.arpa" \
--foreman-proxy-dns-server "127.0.0.1" \
--foreman-proxy-dns-zone "example.com" \
--foreman-proxy-tftp true \
--foreman-proxy-tftp-managed true
```


