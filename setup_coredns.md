### Setup a SCION capable CoreDNS nameserver

### Installation

All you need is Golang 1.24 and git.
Clone the SCION capable CoreDNS fork and build it:

```
git clone https://github.com/netsys-lab/scion-coredns-doq --branch attempt-rebase coredns
cd coredns && CGO_ENABLED=0 go install -buildvcs=false -a .
```

Then run it with the following command:

```
#start CoreDNS nameserver
coredns -conf Corefile 2>&1 | rotatelogs -n 2 /var/log/coredns.log 1M &
```
### Configuration

The configuration of the nameserver happens via the `Corefile` just as described by the [CoreDNS Manual](https://coredns.io/manual/toc/).

A simple configuration for an Authoritative Primary Nameserver for dummy `scion.test.` zone is as follows
```
squic://scion.test.:853 {
	tls  ca/ns.scion.test-cert.pem
		  ca/ns.scion.test-key.pem
	root ./zones
	rhine db.scion.test.signed scion.test. {
		scion on
	}
	sign db.scion.test scion.test. {
	 	rcert file ../sign/root_cert.pem
		key file ../signing-keys/ed25519-256/Kscion.test.+015+19860.key
		directory . 
	}
	debug
	log
	errors
}
```
`squic` is the new schema for SCION DoQ and `853` is the standart port for DoQ.
This is the only transport which is supported for SCION.
The [rhine plugin](https://github.com/netsys-lab/scion-coredns-doq/tree/attempt-rebase/plugin/rhine) is a drop in replacement for the `file` plugin which also serves Resource Records (RR's) from a preloaded file that exists on disk but in addition returns the matching RHINE authenticators (answer + ZoneSigningKey (DNSKEY) + signatures (RRSIG) + RCert) along with the answer, required by the client to verify its authenticity. 
In order for the plugin to work the zonefile must contain the required DNSKEYs and RRSIGs which is taken care by the `sign` plugin. It reads the bare unsigned zonefile `scion.test.db` as well as the ZSK from disk and outputs the signed zonefile `db.scion.test.signed`, which is then served by `rhine`.
If you don't want to use RHINE, you can still use the `file` plugin just as before.

Another popular workload: load balancing between multiple servers - requires to configure zone transfer from primary to secondaries.
Here again for the dummy `scion.test` zone:

```
#PRIMARY NS                              #SECONDARY NS
squic://scion.test.:853 {                squic://scion.test.:853 {
	tls  ca/ns.scion.test-cert.pem            ...
		  ca/ns.scion.test-key.pem
	file db.scion.test scion.test.

                             AXFR over DoQ
    transfer scion.test. {      =====>      secondary scion.test. { 
            to *                                transfer from 19-ffaa:1:1067,[127.0.0.1]:853
    }                                        }
	root ./zones                            .....
	debug                                }
	log
	errors
}
```