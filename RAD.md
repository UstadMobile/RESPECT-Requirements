# Resilient Asset Delivery (RAD)

For assets that do not contain personal information (e.g. videos, images, textures, etc that are generally responsible for most bandwidth usage) e.g. requests that would not use the Cache-Control: private header.

* App requests a list of HTTP urls to be saved to a given local (e.g. on disk) location
* If assets already available, return existing location
* If not available, use [Internet Cache Protocol (ICP)](https://en.wikipedia.org/wiki/Internet_Cache_Protocol) to query other devices on the network to see if the asset can be downloaded locally. Peers will be discovered standard [Network Service Discovery](https://developer.android.com/develop/connectivity/wifi/use-nsd).
* If the device is not connected to any network, the library/device MAY send ICP requests using Bluetooth Low Energy instead. If a nearby device is found to have the content, a WiFi connection may be formed using [WiFi Direct](https://developer.android.com/develop/connectivity/wifi/wifi-direct).
* If the network (e.g. school network, Mobile Network Operator) advertises a proxy using [Web Proxy Auto Discovery](https://en.wikipedia.org/wiki/Web_Proxy_Auto-Discovery_Protocol), then use (if already acceptped by user) the network cache
* If not available on local network, download using HTTP (which can use standard Content Delivery Networks e.g. CloudFlare) via the network cache if available.
* If the list of URLs aligns with a pre-assembled Play Asset Delivery pack, and Google Play Services are available, this may be used to fetch assets instead of the origin http server (e.g. to further reduce bandwidth costs and utilize the [Google Global Cache](https://support.google.com/interconnect/answer/9058809?hl=en) that many ISPs already utilize)
