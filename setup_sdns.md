### Setup a SCION capable SDNS recursive resolver

### Installation

All you need is Golang 1.24 and git.
Clone the SCION capable `sdns` fork and build it.
```
git clone https://github.com/netsys-lab/scion-sdns --branch new-main scion-sdns
cd scion-sdns && CGO_ENABLED=0 go install -buildvcs=false -a .
```
If you want to run the resolver locally you first need to disable the existing stub resolver:

```
# disable system resolver listening on 127.0.0.1:53
systemctl stop systemd-resolved.service
```
Or add a new listen address to  your `/etc/resolv.conf` file (that isn't already in use) and set the same address as `bind` in `sdns.conf`.
If you want to operate a public resolver, this step is not required and you only need to be concerned with `bindsdoq` in.

Then finally start the resolver:
```
# start recursive resolver
sudo sdns --config sdns.conf  2>&1 | rotatelogs -n 2 /var/log/sdns.log 1M &
```

### Configuration

Configuration happens with the `sdns.conf` YAML file.
A reasonable template is:

```
qname_min_level = 5

bind = "0.0.0.0:53"      # local Do53 listen address (NO Encryption & DNS Privacy )
bindsdoq = "0.0.0.0:853" # public DoE listen address
scion = true
lookupSCIONAddressEager = true

cacertificatefile = "scionlab/CACert.pem" # RHINE certificate to validate RRs with
loglevel = "debug"

tlscertificate = "ca/resolver.ovgu.scionlab-cert.pem" # TLS certificate file
tlsprivatekey = "ca/resolver.ovgu.scionlab-key.pem" # TLS private key file

namedRootServers = [ ["192.5.5.241:53", "f.root-servers.net."], 
   				 ["123.456.789.1:53", "rhine.ovgu.scionlab."],... ] 

namedRootSCIONServers = [ ["19-ffaa:1:fe4,[127.0.0.1]:853", "rhine.ovgu.scionlab."], ... ]
```
For more detailed information i.e. how to configure `forwarding` see [upstream sdns](https://github.com/semihalev/sdns).
