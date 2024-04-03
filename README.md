# WOL command
send magic packets.

## Features
* yaml config to store MAC addresses
  * you can foget MAC address!
* group waking up
* target ip address chengable

## How To Use

command line:
```shell:example
#wake pc1, srv1
$ wol pc1 srv1

#wake group 'servers' (=srv1, srv2)
$ wol -g servers

#wake hosts by ip/name specified (dns resolvable/rarely success)
$ wol 192.168.10.10 calc-server

#direct MAC, ip, port specified
$ wol -t AA:CC:EE:BB:DD:FF -i 10.255.255.255 -p 7

#list hosts/groups in config
$ wol -l

#show help
$ wol -h
```

yaml config:
```yaml:~/.wol_conf.yaml
wol:
  default:
    ip: "255.255.255.255"
    port: 7
  hosts:
    pc1:
      #mac is required for each host
      mac: "AA:BB:CC:DD:EE:FF"
    srv1:
      mac: "00:11:22:33:44:55"
    srv2:
      mac: "00:11:22:33:44:AA"
      #ip is optional, CIDR is also accepable
      ip: "172.16.0.1/12"
  groups:
    servers:
      - srv1
      - srv2
```
**remaks for yaml**
 * mac is also called like physical, hardware, or ethernet address, etc.
 * 'ip' must be a broadcast address for the subnet of each hosts, or CIDR.
 * all keys are case-sensitive (lowercase except host/group names).

## Requirements
* python
* (python package) pyyaml
* (shell commands) ip, awk

## Limitations
* No warranty
* not tested: WOL among subnets
* waking by hostname/ip: arp table would be 'FAILD' while host is down, so rarely success to wake that.

## Host resolving
1. 'Host' listed in yaml/wol/hosts:
   * get mac from yaml
2. (group mode) 'Group' listed in yaml/wol/groups:
   * get hosts from yaml, then get these macs from yaml
3. 'Host' not listed in yaml
   1. if a hostname specified, resolve it to ip by 'gethostbyname'
   2. check arp table (shell: ip neigh show) to get mac address from ip

## Lisence
* MIT
