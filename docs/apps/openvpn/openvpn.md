# OPENVPN
----

## <i class="fa-solid fa-hashtag"></i> Information

![Logo](../../_media/apps/openvpn/openvpn_logo.svg ':size=250 :no-zoom')


> <i class="fa-solid fa-quote-left"></i> **OpenVPN** is a full-featured open source SSL VPN solution that accommodates a wide range of configurations, including remote access, site-to-site VPNs, Wi-Fi security, and enterprise-scale remote access solutions with load balancing, failover, and fine-grained access-controls. Starting with the fundamental premise that complexity is the enemy of security, **OpenVPN** offers a cost-effective, lightweight alternative to other VPN technologies that is well-adapted for the SME and enterprise markets.
>
> The **OpenVPN** security model is based on SSL, the industry standard for secure communications via the internet. **OpenVPN** implements OSI layer 2 or 3 secure network extension using the SSL/TLS protocol, supports flexible client authentication methods based on certificates, smart cards, and/or 2-factor authentication, and allows user or group-specific access control policies using firewall rules applied to the VPN virtual interface. **OpenVPN** is not a web application proxy and does not operate through a web browser.
>
> What can you do with **OpenVPN** ?
>
> - Tunnel any IP subnetwork or virtual ethernet adapter over a single UDP or TCP port,
> - Configure a scalable, load-balanced VPN server farm using one or more machines which can handle thousands of dynamic connections from incoming VPN clients,
> - Use all of the encryption, authentication, and certification features of the SSL library to protect your private network traffic as it transits the internet,
> - Use any cipher, key size, or HMAC digest (for datagram integrity checking) supported by the SSL library,
> - Choose between static-key based conventional encryption or certificate-based public key encryption,
> - Use static, pre-shared keys or TLS-based dynamic key exchange,
> - Use real-time adaptive link compression and traffic-shaping to manage link bandwidth utilization,
> - Tunnel networks whose public endpoints are dynamic such as DHCP or dial-in clients,
> - Tunnel networks through connection-oriented stateful firewalls without having to use explicit firewall rules,
> - Tunnel networks over NAT,
> - Create secure ethernet bridges using virtual tap devices, and
> - Control **OpenVPN** using a GUI on Windows or Mac OS X.
>
> What distinguishes **OpenVPN** from other VPN packages?
>
> - **OpenVPN**'s principal strengths include cross-platform portability across most of the known computing universe, excellent stability, scalability to hundreds or thousands of clients, relatively easy installation, and support for dynamic IP addresses and NAT.
> - **OpenVPN** supports IPv6 transport (**OpenVPN** over IPv6 network) and IPv6 payload (IPv6 inside IPv4)
> - OpenPVN can make use of either OpenSSL or PolarSSL
> - **OpenVPN** provides an extensible VPN framework which has been designed to ease site-specific customization, such as providing the capability to distribute a customized installation package to clients, or supporting alternative authentication methods via **OpenVPN**'s plugin module interface (For example the openvpn-auth-pam module allows **OpenVPN** to authenticate clients using any PAM authentication method -- such methods may be used exclusively or combined with X509 certificate-based authentication).
> - **OpenVPN** offers a management interface which can be used to remotely control or centrally manage an **OpenVPN** daemon. The management interface can also be used to develop a GUI or web-based front-end application for **OpenVPN**.
> - On Windows, **OpenVPN** can read certificates and private keys from smart cards which support the Windows Crypto API.
> - **OpenVPN** uses an industrial-strength security model designed to protect against both passive and active attacks. OpenVPN's security model is based on using SSL/TLS for session authentication and the IPSec ESP protocol for secure tunnel transport over UDP. **OpenVPN** supports the X509 PKI (public key infrastructure) for session authentication, the TLS protocol for key exchange, the OpenSSL cipher-independent EVP interface for encrypting tunnel data, and the HMAC-SHA1 algorithm for authenticating tunnel data.
> - **OpenVPN** is built for portability. At the time of this writing, **OpenVPN** runs on Linux, Solaris, OpenBSD, FreeBSD, NetBSD, Mac OS X, and Windows (XP and later versions). Because **OpenVPN** is written as a user-space daemon rather than a kernel module or a complex modification to the IP layer, porting efforts are dramatically simplified.
> - **OpenVPN** is easy to use. In general, a tunnel can be created and configured with a single command (and without any required configuration files). **OpenVPN**'s documentation contain examples illustrative of its ease of use.
> - **OpenVPN** has been rigorously designed and tested to operate robustly on unreliable networks. A major design goal of **OpenVPN** is that it should be as responsive, in terms of both normal operations and error recovery, as the underlying IP layer that it is tunneling over. That means that if the IP layer goes down for 5 minutes, when it comes back up, tunnel traffic will immediately resume even if the outage interfered with a dynamic key exchange which was scheduled during that time.
> - **OpenVPN** has been built with a strongly modular design. All of the crypto is handled by the SSL library, and all of the IP tunneling functionality is provided through the TUN/TAP virtual network driver. The benefits of this modularity can be seen, for example, in the way that **OpenVPN** can be dynamically linked with a new version of the SSL library and immediately have access to any new functionality provided in the new release. In the same way, **OpenVPN**'s user-space design allows straightforward porting to any OS which includes a TUN/TAP virtual network driver.
> - **OpenVPN** is fast. Running Redhat 7.2 on a Pentium II 266mhz machine, using TLS-based session authentication, the Blowfish cipher, SHA1 authentication for the tunnel data, and tunneling an FTP session with large, pre-compressed files, OpenVPN achieved a send/receive transfer rate of 1.455 megabytes per second of CPU time (combined kernel and user time).
> - While **OpenVPN** provides many options for controlling the security parameters of the VPN tunnel, it also provides options for protecting the security of the server itself, such as `--chroot` for restricting the part of the file system the OpenVPN daemon has access to, `--user` and `--group` for downgrading daemon privileges after initialization, and `--mlock` to ensure that key material and tunnel data is never paged to disk where it might later be recovered. <i class="fa-solid fa-quote-left fa-rotate-180"></i>


- <i class="fa-solid fa-globe"></i> **Site Officiel** : https://openvpn.net/
- <i class="fa-solid fa-book"></i> **Documentation** : https://community.openvpn.net/openvpn
- <i class="fa-solid fa-file-circle-question"></i> **Changelog** : https://github.com/OpenVPN/openvpn/releases
- <i class="fa-brands fa-github"></i> **Github** : https://github.com/OpenVPN/openvpn/
- <i class="fab fa-blogger-b"></i> **Blog** : https://openvpn.net/blog/
- <i class="fas fa-comments"></i> **Forum** : https://forums.openvpn.net/?_ga=2.244000263.326976489.1648437183-1750138733.1646446773

---

## <i class="fa-regular fa-newspaper"></i> Articles

<details open>

</details>

---

## <i class="fa-solid fa-glasses"></i> Articles externes

<details>

- [Build a Real VPN with OpenVPN](https://www.linux.com/learn/intro-to-linux/2017/3/build-real-vpn-openvpn)
- [CentOS 7.0 Set Up OpenVPN Server In 5 Minutes](https://www.cyberciti.biz/faq/centos-7-0-set-up-openvpn-server-in-5-minutes/)
- [CentOS 8 Set Up OpenVPN Server In 5 Minutes](https://www.cyberciti.biz/faq/centos-8-set-up-openvpn-server-in-5-minutes/)
- [CentOS 8 Set Up OpenVPN Server In 5 Minutes](https://www.cyberciti.biz/faq/centos-8-set-up-openvpn-server-in-5-minutes/)
- [Comment mettre en place et configurer un serveur OpenVPN sur CentOS 8](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-an-openvpn-server-on-centos-8-fr)
- [Comment mettre en place et configurer un serveur OpenVPN sur Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-an-openvpn-server-on-ubuntu-20-04-fr)
- [How To Install and Configure an OpenVPN Server on Debian 9 In 5 Minutes](https://www.cyberciti.biz/faq/how-to-install-and-configure-an-openvpn-server-on-debian-9-in-5-minutes/)
- [How to Install and Configure OpenVPN Server in CentOS 8/7](https://www.tecmint.com/install-openvpn-in-centos/)
- [How to install and configure OpenVPN Server on Debian 10](https://www.howtoforge.com/how-to-install-and-configure-openvpn-server-on-debian-10/)
- [How to Install OpenVPN on CentOS 7](https://www.rosehosting.com/blog/how-to-install-openvpn-on-centos-7/)
- [How to Install OpenVPN Server and Client with Easy-RSA 3 on CentOS 7](https://www.howtoforge.com/tutorial/how-to-install-openvpn-server-and-client-with-easy-rsa-3-on-centos-7/)
- [How to Install OpenVPN Server and Client with Easy-RSA 3 on CentOS 8](https://www.howtoforge.com/tutorial/how-to-install-openvpn-server-and-client-with-easy-rsa-3-on-centos-8/)
- [How To Set Up an OpenVPN Server on CentOS 7](https://linuxize.com/post/how-to-set-up-an-openvpn-server-on-centos-7/)
- [How To Set Up an OpenVPN Server on Debian 10](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-debian-10)
- [How To Set Up an OpenVPN Server on Debian 9](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-debian-9)
- [How To Set Up an OpenVPN Server on Ubuntu 18.04](https://linuxize.com/post/how-to-set-up-an-openvpn-server-on-ubuntu-18-04/)
- [How To Set Up an OpenVPN Server on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-18-04)
- [How To Set Up and Configure an OpenVPN Server on CentOS 8](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-an-openvpn-server-on-centos-8)
- [How To Set Up and Configure an OpenVPN Server on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-an-openvpn-server-on-ubuntu-20-04)
- [Howto OpenVPN](https://wiki.evolix.org/HowtoOpenVPN)
- [Installation et configuration hautement sécurisée de OpenVPN](https://blog.mirabellette.eu/index.php?article36/installation-et-configuration-hautement-securisee-openvpn)
- [OpenVPN IPv6 and IPv4 configuration](https://4sysops.com/archives/openvpn-ipv6-minimal-configuration/)
- [Qu’est-ce qu’OpenVPN ?](https://www.it-connect.fr/quest-ce-quopenvpn/)

</details>