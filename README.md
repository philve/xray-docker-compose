# Xray Docker Compose

This repository contains sample Docker Compose files to run Xray upstream and bridge servers.

## Documentation

### Terminology

* Upstream Server: A server that has free access to the Internet.
* Bridge Server: A server that is available to clients and has access to the upstream server.
* Client: A user-side application with access to the bridge server.

```
(Client) <-> [ Bridge Server ] <-> [ Upstream Server ] <-> (Internet)
```

### Setup

#### UUIDs

We use the VMESS protocol as the primary protocol.
The VMESS protocol requires UUIDs for security reasons (instead of passwords).
We need two UUIDs for the two Xray servers (upstream and bridge servers).

You can generate UUIDs using:
* This Linux command: ```cat /proc/sys/kernel/random/uuid```
* This online tool: [https://www.uuidgenerator.net](https://www.uuidgenerator.net)

Sample generated UUIDs:
* `cfc3ac34-a70d-424e-b43c-33049cf4bf31`
* `143d98d8-ac89-465a-acb5-d8d51e1f851f`

#### Upstream Server

To setup the upstream server:
1. Copy the "xray-upstream-server" directory into the upstream server.
2. Replace `<UPSTREAM-UUID>` in the `config.json` file with one of the generated UUIDs.
3. Run `docker-compose up -d`.

#### Bridge Server

To setup the bridge server:
1. Copy the "xray-bridge-server" directory into the bridge server.
2. Replace the following variables in the `config.json` file with appropriate values.
    * `<SHADOWSOCKS-PASSWORD>`: A password for Shadowsocks users like `!FR33DoM!`.
    * `<BRIDGE-UUID>`: The generated UUID for the bridge server.
    * `<UPSTREAM-IP>`: The upstream server IP address like `13.13.13.13`.
    * `<UPSTREAM-UUID>`: The used UUID for the upstream server.
3. Run `docker-compose up -d`. 

#### Clients

The bridge server exposes these proxy protocols:
* Shadowsocks
* VMESS
* HTTP
* SOCKS

##### Shadowsocks Protocol

Shadowsocks is a popular proxy protocol.
You can find many client apps to use the Shadowsocks proxy on your devices.
These are recommended client apps:
* [Shadowsocks for macOS](https://github.com/shadowsocks/ShadowsocksX-NG/releases)
* [Shadowsocks for Linux](https://github.com/shadowsocks/shadowsocks-libev)
* [Shadowsocks for Windows](https://github.com/shadowsocks/shadowsocks-windows/releases)
* [Shadowsocks for Android](https://github.com/shadowsocks/shadowsocks-android/releases)
* [ShadowLink for iOS](https://apps.apple.com/us/app/shadowlink-shadowsocks-vpn/id1439686518)

Client configuration:
```
IP Address: <BRIDGE-IP>
Port: 1210
Encryption/Method/Algorithm: aes-128-gcm
Password: <SHADOWSOCKS-PASSWORD>
```

##### VMESS Protocol

The VMESS proxy protocol is the recommended one.
It's the primary protocol that Xray provides.
These are recommended client apps:
* [V2RayXS for macOS](https://github.com/tzmax/V2RayXS/releases)
* [Xray-core for Linux](https://github.com/XTLS/Xray-core)
* [v2rayN for Windows](https://github.com/2dust/v2rayN/releases)
* [ShadowLink for iOS](https://apps.apple.com/us/app/shadowlink-shadowsocks-vpn/id1439686518)
* [v2rayNG for Android](https://github.com/2dust/v2rayNG)

Client configuration:
```
IP Address: <BRIDGE-IP>
Port: 1310
ID/UUID/UserID: <BRIDGE-UUID>
Alter ID: 0
Level: 0
Security/Method/Encryption: aes-128-gcm
Network: TCP
```

##### HTTP & SOCKS Protocols

Moved here: [HTTP_SOCKS_PROTOCOLS.md](HTTP_SOCKS_PROTOCOLS.md)

## P.S.

See [v2ray-docker-compose](https://github.com/miladrahimi/v2ray-docker-compose) if you prefer V2Ray server.
