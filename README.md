# Pihole - AdGuard Home - Unbound IN DOCKER

This sets up Pihole as your front line filtering, DHCP (Optional) Group/Device Managment service. It can then forward to AdGuardHome which can use encrypted upstreams for DNS (tls://9.9.9.9) and Unbound for recursion/fall-back/cache

Here is a guide to every port used in the combined Docker Compose file.

# Pi-hole

- 53:53 (TCP/UDP): This is the standard port for DNS. Your network clients (computers, phones, etc.) will send their domain name queries to Pi-hole on this port. Pi-hole is configured to be the primary DNS server for your network on the standard port.

- 8888:80 (TCP): This maps port 8888 on your host machine to port 80 inside the Pi-hole container. It exposes the Pi-hole web admin interface via HTTP. You can access it at http://your-docker-host-ip:8888.

- 4433:443 (TCP): This maps port 4433 on your host to port 443 in the container, exposing the Pi-hole web admin interface via HTTPS for a secure connection.

- 67:67 (UDP) (Optional): This port is for the DHCP (Dynamic Host Configuration Protocol) service. If you enable Pi-hole's built-in DHCP server, it will use this port to assign IP addresses to devices on your network.

- 123:123 (UDP) (Optional): This is the port for NTP (Network Time Protocol). You can enable this if you want Pi-hole to act as a time server for your network devices.

# AdGuard Home

- 5353:5353 (TCP/UDP): This configuration exposes AdGuard Home's DNS server on a non-standard port, 5353. This avoids conflicting with Pi-hole. For this to work, you must configure AdGuard Home (during its initial setup) to listen for DNS queries on port 5353.

- 3000:3000 (TCP): The initial setup port for AdGuard Home's web interface. When you first launch the container, you will navigate to http://your-docker-host-ip:3000 to configure it.

- 8080:8080 (TCP): The primary port for the AdGuard Home web admin interface after the initial setup is complete.

- 853:853 (TCP): Used for DNS-over-TLS (DoT), an encrypted DNS protocol that increases privacy and security by tunneling DNS queries through a TLS connection.

- 853:853 (UDP): Used for DNS-over-QUIC (DoQ), another modern, encrypted DNS protocol.

- 5443:5443 (TCP/UDP): Used for DNS-over-HTTPS (DoH). This protocol wraps DNS queries in HTTPS traffic, which can help bypass certain firewalls and further enhance privacy.

# Unbound

- 5335:5335 (TCP/UDP): This exposes the Unbound recursive DNS resolver on host port 5335. Unbound listens on the standard DNS port 53 inside its container. You would typically configure Pi-hole or AdGuard Home to use Unbound as their upstream DNS server by pointing them to unbound:5335 or docker-host-ip:5335.
