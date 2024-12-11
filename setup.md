# SCION Browser Setup
The setup consists of two core components that interact: The SCION Browser Extension for Chromium based Browsers and the SCION HTTP Forward Proxy.
- [scion-browser-extension](https://scion-browser-extension.readthedocs.io/en/latest/)
- [HTTP Forward proxy](https://scion-http-proxy.readthedocs.io/en/latest/index.html)

Detailed instructions and explanations can be found in the respective readthedocs documentations. Here we provide a quick-start guide.

## SCION HTTP Forwarding Proxy
The SCION HTTP Forward Proxy provides access to HTTP(S) resources via SCION by using a configured proxy for all SCION-enabled domains. It is implemented as a Caddy plugin, and can be used with any compatible Caddy server version.

Setup Instructions:
- Download the [source code](https://github.com/scionproto-contrib/http-proxy) from the repository.
- Build the forward proxy `make build scion-caddy-forward`
- Run the proxy with the forwarding config: `./scion-caddy-forward run --config ./examples/scion-caddy-forward-proxy.json`

## SCION Browser Extension
The SCION browser extension is a tool that provides access to HTTP(S) resources via SCION for chromium-based browsers. 

Setup instructions:
- Download the [source code]([https://github.com/scionproto-contrib/http-proxy](https://github.com/scionproto-contrib/browser-extension)) from the repository.
- Run chromium with disabled certificate verification, e.g. `chromium --ignore-certificate-errors`
- Open `Extensions`, enable `Developer Mode` and click on `load unpacked`
- Open the `chrome` folder of the source code downloaded previously

If you're part of SCIERA:
- Add `netsys.ovgu.de` to your local hosts file `/etc/hosts` via a new line containing `71-2:0:4a,141.44.25.151 netsys.ovgu.de www.netsys.ovgu.de`
- Open `www.netsys.ovgu.de` in your Chromium Browser
