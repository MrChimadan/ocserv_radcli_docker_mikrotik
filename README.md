



# OpenConnect server (ocserv) docker image (using alpine linux) + radcli (RADIUS auth for MikroTik User Manager)

[![Docker pulls)](https://img.shields.io/docker/pulls/chimadan/ocserv-radcli.svg)](https://hub.docker.com/r/chimadan/ocserv-radcli)
![LICENSE](https://img.shields.io/github/license/cherts/ocserv_docker)

ocerv_docker is an OpenConnect VPN Server boxed in a Docker image built by [Mikhail Grigorev](mailto:sleuthhound@gmail.com)

## What is OpenConnect server?

OpenConnect VPN server is an SSL VPN server that is secure, small, fast and configurable. It implements the OpenConnect SSL VPN protocol and has also (currently experimental) compatibility with clients using the AnyConnect SSL VPN protocol. The OpenConnect protocol provides a dual TCP/UDP VPN channel and uses the standard IETF security protocols to secure it. The OpenConnect client is multi-platform and available [here](https://www.infradead.org/openconnect/). Alternatively, you can try connecting using the official Cisco AnyConnect client (Confirmed working with AnyConnect 5.1.7.93) and all latest version OpenConnect. 

- [Homepage](https://www.infradead.org/openconnect/)
- [Documentation](https://ocserv.openconnect-vpn.net/ocserv.8.html)
- [Source](https://gitlab.com/openconnect/ocserv)

## How is this image different from others?

This project is a fork of an ocserv Docker image with **RADIUS authentication support**, designed specifically for integration with **MikroTik User Manager**.

- Uses the latest version of OpenConnect (v1.4.0);
- Strong SSL/TLS ciphers are used (see tls-priorities options);
- Alpine Linux base image is used;
- Easy customization of the image is possible (changing the directory of the configuration file, TCP and UDP ports and additional options for running ocserv through the variables);

The container includes:
- **radcli** utility
- **ocserv** built from source with RADIUS authentication support
- Automatic ocserv configuration for RADIUS-based authentication

The main goal of this project is to run **OpenConnect VPN (ocserv)** in Docker with authentication and accounting handled by **MikroTik User Manager**.

---

## Features

- ocserv compiled with:
  - RADIUS authentication backend
  - radcli support
- Compatible with MikroTik User Manager
- No local user database required
- Configuration via environment variables
- Suitable for ISP and enterprise VPN setups
- Designed for containerized environments

---

## Authentication

Authentication is performed via **RADIUS**:
- Username/password validation
- Accounting (Start/Stop)
- Centralized user management in MikroTik User Manager

Local authentication methods are disabled by default.

---

---

## Networking Notes

- NAT should be configured on the **host system**, not inside the container

### Environment Variables

All the variables to this image is optional, which means you don't have to type in any environment variables, and you can have a OpenConnect Server out of the box! However, if you like to config the ocserv the way you like it, here's what you wanna know.

`HC_WORKDIR`, this is the ocserv working directory, include configuration file ocserv.conf, DH params file (dh-params option), certificate files (server-cert and server-key options) and ocpasswd file (auth option).

`HC_TCP_PORT`, this is the ocserv TCP port number (tcp-port option).

`HC_UDP_PORT`, this is the ocserv UDP port number (tcp-port option).

`HC_OTHER_OPTS`, this is the ocserv comand line options.

`HC_CA_CN`, this is the common name used to generate the CA (Certificate Authority).

`HC_CA_ORG`, this is the organization name used to generate the CA.

`HC_CA_DAYS`, this is the expiration days used to generate the CA.

`HC_SRV_CN`, this is the common name used to generate the server certification.

`HC_SRV_ORG`, this is the organization name used to generate the server certification.

`HC_SRV_DAYS`, this is the expiration days used to generate the server certification.

`HC_NO_CREATE_DH_PARAMS`, while this variable is set to not empty, the DH params file will not be created. You have to create your own DH params file and set path to file into config ocserv (dh-params option). The default value is to generate DH params file automaticaly if not exist.

`HC_NO_CREATE_SERVER_CERT`, while this variable is set to not empty, the server certificate file will not be created. You have to create your own server certificate file and set path to file into config ocserv (server-cert and server-key option). The default value is to generate server certificate file automaticaly if not exist.
 
`HC_RAD_SRV`, Radius server IP address

`HC_RAD_SECRET`, shared secret of the Radius server

`HC_VPN_NET`,  VPN network addressing in CIDR notation.


The default values of the above environment variables:

|   Variable       |     Default     |
|:----------------:|:---------------:|
|  **HC_WORKDIR**  |   /etc/ocserv   |
|  **HC_TCP_PORT** |       443       |
|  **HC_UDP_PORT** |       443       |
|  **HC_CA_CN**    |      VPN CA     |
|  **HC_CA_ORG**   | My Organization |
| **HC_CA_DAYS**   |       9999      |
|  **HC_SRV_CN**   | www.example.com |
| **HC_SRV_ORG**   |    My Company   |
| **HC_SRV_DAYS**  |       9999      |
| **HC_RAD_SRV**   |    127.0.0.1    |
| **HC_RAD_SECRET**|    12345678     |
| **HC_VPN_NET**   |  10.20.30.0/24  |

---

## Docker Image

Prebuilt Docker image is available on Docker Hub:

https://hub.docker.com/r/chimadan/ocserv-radcli

---

## Disclaimer

This project is provided as-is. Make sure you understand your security and networking requirements before deploying it in production.
