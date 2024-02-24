# Ansible roles for WireGuard VPN

I've made these roles for deploying WireGuard VPN on a bastion server, however
they can be used for any other type of server.

## Roles

### [server](./ansible/roles/server/)

The server role actually configures non-distro-specific things for the VPN
server and starts the systemd service using `wg-quick` service.

### [el](./ansible/roles/el/)

Role that installs dependencies and sets up firewall rules on Enterprise Linux
distributions (think RHEL, not SUSE).

### [ubuntu](./ansible/roles/ubuntu/)

Role that installs dependencies and sets up firewall rules on Ubuntu
distributions.

## Variables

### Server configuration

| Variable name         | Description                                         |
|-----------------------|-----------------------------------------------------|
| wg_interface          | Interface name for WireGuard                        |
| wg_out_interace       | Out interface name (only on Ubuntu)                 |
| wg_server_addr4       | IPv4 address of the server                          |
| wg_server_addr6       | IPv6 address of the server                          |
| wg_server_port        | Port on which WireGuard will listen for connections |
| wg_subnet4            | Subnet for IPv4 range                               |
| wg_subnet6            | Subnet for IPv6 range                               |
| wg_netmask4           | Netmask for IPv4 range                              |
| wg_netmask6           | Netmask for IPv6 range                              |
| wg_server_private_key | Base64 encoded privat key of the server             |

### Peer configuration

For configuring peer entries on the server, you will need to define a `wg_peers`
variable which is an array of objects. The following properties need to be
present in an object.

| Property name | Description                      |
|---------------|----------------------------------|
| public_key    | Public key of the peer           |
| addr4         | IPv4 allowed address of the peer |
| addr6         | IPv6 allowed address of the peer |

All peers are allowed only one address (netmask 32).

To se an example of all the variables, you can view the [variables file](./ansible/vars/all.yaml).

## Tests

In the [vagrant](./vagrant/) directory there is a [Vagrantfile](./vagrant/Vagrantfile)
which creates, starts and provisions virtual machines using the VirtualBox
provider which can be used for testing the roles on different distributions.

The playbooks used for testing are available in the [ansible](./ansible/)
directory.

For now the only tested distributions are the following:

- Rocky Linux 9
- Ubuntu 22.04

## License

[MIT](./LICENSE)
