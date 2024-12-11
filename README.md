# SCION Browser

The SCION Browser project (funded by [NGI Search](https://www.ngisearch.eu/view/Events/OC3Searchers)) aims to integrate SCION into the Brave web browser to enable path-aware retrieval of web resources. However, finding the most suitable paths is a challenging problem. This browser will use PANAPI to automatically find the corresponding paths, optimising application- and user-based metrics such as overall page load time, latency, bandwidth, privacy, and CO2 footprint according to the application's needs and user's preferences set in the browser. Additionally, it will also integrate support for RHINE into the Brave browser.

## Milestone 1 - Kick Off

## Milestone 2 - Development of the SCION Web Browser

### Implementation of SCION into Brave

SCION support is added to the Brave browser in combination of a browser plugin and a local SCION proxy. We also implement a browser GUI that enables users to easily configure their privacy and performance preferences.

- [Setup Instructions](setup.md)
- [Presentation](https://docs.google.com/presentation/d/1WOG8_PZ0plRXJO1rmgy-g2BhmfGkdW0j9SC0UPZ9W5M/edit?usp=sharing)
- [Browser Demo Video](https://drive.google.com/file/d/1ecuKRqZxcQujaOwI1KhVlhBcoWTv-BMv/view?usp=sharing)

## Milestone 3 - Implementation of PANAPI and RHINE into the SCION Browser

### Implementation of PANAPI into Brave 

PANAPI support is added to the SCION browser, to enable both automatic and user-configured path selection to meet the applications and users requirements (e.g. minimizing page load time or CO2 footprint).

- Comparison of PLT with and without PANAPI
- Demo Video

### Implementation of RHINE into Brave

RHINE support is added to the SCION browser to enable privacy preserving DNS lookups pointing to SCION addresses.

- Demonstration of DNS lookup with RHINE in the browser.
