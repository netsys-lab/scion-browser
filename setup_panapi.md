# Setup instructions for PANAPI in the Browser
The setup of PANAPI in the Browser requires three repositories to be cloned with specific branches:

## Setup a SCIONLab Connection
This documentation is based on using our `www.netsys.ovgu.de` site which is hosted within [scionlab](https://www.scionlab.org/). Please create a user AS and connect to one of the attachment points to follow the instructions.

## Initializing repository

```sh
mkdir panapi
cd panapi
git clone git@github.com:netsys-lab/pan-lua.git
git checkout aca0127dede53556f6509c1527dd574c96f952ba

git clone git@github.com:netsys-lab/scion-apps.git
git checkout 3a80f48b45ec62fcfb93e396f87f359fc7a40a9c

git clone git@github.com:netsys-lab/scion-browser-extensions.git
git checkout 98a2f7267b21732de7e0012b73b49f88fb09f7c5
```

## Setup Browser Extensions
Open your chrome/chromium browser, navigate to `Extensions` and enable `Developer Mode`. Then click the button `Load unpacked` and load the browser extension from the `panapi/scion-browser-extensions/chrome` directory.

## Build Skip Proxy
This steps requires `go >= 1.21` to be installed on your system. Navigate to `panapi/scion-apps/skip/skip` binary and runu  `go build`. This will create the `./skip` binary.

## Starting the Proxy
Start `skip` binary with argument `--script=ceohtwo.lua` (skip needs to be started with a nonempty `--script` flag, otherwise the lua subsystem doesn't load). At this point, a different or modified script can already be uploaded to `skip` via HTTP POST. To do it with curl: `curl -T <scriptname.lua> localhost:8888/lua/script`. The currently loaded script can be retreived via HTTP GET on `localhost:8888/lua/script`).

To send any kind of JSON payload to the currently running Lua script (via the `panapi.JSON` callback function that needs to be implemented in the script, see `ceohtwo.lua` for an example), JSON encoded data can be POSTed to `localhost:8888/lua/json`. To do this with curl and the included `CO2_emissions.json`, do the following: `curl -X POST -H "Content-Type: application/json" localhost:8888/lua/json -T CO2_emissions.json`
JSON can also be used to instruct the running script to switch into a different mode. Again, see `ceohtwo.lua` for an example. To switch to a different path selection strategy, POST one of the following JSON snippets to `localhost:8888/lua/json` (e.g. using `curl` as seen above):
  - `{"strategy": "shortestPath"}`
  - `{"strategy": "lowestCO2"}`
  - `{"strategy": "balancedCO2"}`

To view the CO2 usage, open `www.netsys.ovgu.de` in your chrome/chromium browser with the browser extension installed and click on the SCION icon. There you should see CO2 usage and path statistics. Whenever changing the lua script, reload the page to see some effect.
