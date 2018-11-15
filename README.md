# ansible-wireguard-vpn

Ansible + Wireguard = VPN

## Prerequisites

- Ansible
- SSH
- Root account

## Usage

First, list all VPN servers, including the VPN IP and public IP, in an Ansible hosts file:

```
server-1 ansible_ssh_host=212.x.x.x vpn_ip=192.168.0.1 public_ip=212.x.x.x
server-2 ansible_ssh_host=212.x.x.x vpn_ip=192.168.0.2 public_ip=212.x.x.x

[vpn-servers]
server-1
server-2
```

Next, install the VPN network:

```
$ ansible-playbook wireguard.yml -i hosts -u root
```

## Troubleshooting

- ping: sendmsg: Required key not available

```bash
# ssh 192.168.0.1
# ping 192.168.0.2
PING 192.168.0.2 (192.168.0.2) 56(84) bytes of data.
From 192.168.0.1 icmp_seq=1 Destination Host Unreachable
ping: sendmsg: Required key not available
```

```bash
# ssh 192.168.0.1
# ping 192.168.0.2
PING 192.168.0.2 (192.168.0.2) 56(84) bytes of data.
From 192.168.0.1 icmp_seq=1 Destination Host Unreachable
ping: sendmsg: Required key not available
```

Solution: Check that the peer server's IP address is in the range defined by AllowedIPs.

- all other issues

Verify that all the peers' private and public keys are correct:

```bash
# wg
interface: wg0
  public key: <check this>
  private key: (hidden) <check this>
  listening port: 50111

peer: <check this>
  endpoint: 212.xxx
  allowed ips: 192.168.0.0/16
  transfer: 0 B received, 4.34 KiB sent
```
