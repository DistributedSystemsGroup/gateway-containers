# gateway-containers
Docker containers that act as gateways to access overlay networks

## SOCKS proxy container

Contains the Dante SOCKS proxy, listening on port 1080 with 4 workers.
Can be built as-is.

## SSHd container

Requires customization: it is configured for LDAP authentication and needs a `ldap.conf`
file to be placed in the build directory. In the Dockerfile an admin group is given
passwordless sudo access inside the container.

