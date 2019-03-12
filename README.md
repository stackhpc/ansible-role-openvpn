OpenVPN Server and Clients
==========================

For a VPN server and list of clients, install OpenVPN and Easy-RSA
packages and generate server and client configurations for OpenVPN
tunnels.  Bring the VPN connections up.


Requirements
------------

For access to OpenVPN and Easy-RSA packages, this role will install
the EPEL repo, if it is not already present.

The hosts in the play must be able to communicate with one another
via TCP/IP or UDP/IP.


Role Variables
--------------

`openvpn_server`: Inventory hostname of the host acting as OpenVPN server.

`openvpn_clients`: List of hostnames of clients.

`openvpn_subnet_cidr`: OpenVPN tunnel network subnet.  The OpenVPN server
  will take the first IP address in the subnet.
  The default value is `10.8.0.0/24`

`openvpn_server_proto`: Establish the connection using TCP or UDP.
  The default value is `tcp`

`openvpn_server_port`: The TCP or UDP port for communication with the
  OpenVPN server.
  The default value is `1194`


Dependencies
------------

None.

Example Playbook
----------------

The following playbook creates VPN tunnels between a server and
some client hosts.

    ---
    # This playbook synchronises timekeeping and establishes an OpenVPN
    # tunnel between a VPN server and a number of clients.

    - hosts:
        - openvpn-server
        - openvpn-clients
      become: yes
      roles:
        - role: resmo.ntp

        - role: stackhpc.openvpn
          openvpn_server: "{{ groups['openvpn-server'] | first }}"
          openvpn_clients: "{{ groups['openvpn-client'] }}"
          openvpn_server_proto: "udp"

    ...

Author Information
------------------

- Stig Telfer (<stig@stackhpc.com>)
- Bharat Kunwar (<bharat@stackhpc.com>)
