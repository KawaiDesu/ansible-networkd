networkd 
=========

An Ansible role for configuring systemd-networkd.

This project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Requirements
------------

This role assumes that networkd is already present in the system. So it should be suitable for any distro with networkd.

Role Variables
--------------

Example configuration. Follow networkd documentation to construct yours.
```yaml
networkd:
 link:
     # This is file name
   - name: eth0
     # This is prefix for file. This results in following file name: 50-eth0.link
     priority: 50
     content:
       - Match:
         - MACAddress: "aa:bb:cc:dd:ee:ff"
       - Link:
         - Name: eth0
 netdev:
   - name: br0
     priority: 50
     content:
       - NetDev:
         - Name: br0
         - Kind: bridge
 network:
   - name: eth0
     priority: 50
     content:
       - Match:
         - Name: eth0
       - Network:
         - DHCP: ipv4
         - LinkLocalAddressing: no
         - LLDP: yes
       - DHCPv4:
         - UseHostname: no
         - Hostname: gimme-some-addr
         - UseMTU: yes
   - name: br0_slaves
     priority: 50
     content:
       - Match:
         - MACAddress: "11:bb:cc:dd:ee:ff 22:bb:cc:dd:ee:ff"
       - Network:
         - Bridge: br0
```

What to do on configuration changes. Could be "restart", "reload" or "nothing". Variable is mandatory.
```yaml
networkd_apply_action: "restart"
```

Custom content for `/etc/resolv.conf`. Every element in list is string in file. Variable is optional.
```yaml
networkd_resolv_conf_content:
  - nameserver 1.1.1.1
  - nameserver 8.8.8.8
```

License
-------

MIT
