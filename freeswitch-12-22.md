流量转发
## 转发机： 
网络类型：公网
可以用kamailio或者opensips，也可以用下面的方式把流量转发过去，
业务端口转发到fs上处理，包括媒体、RTC，
root@call-proxy-003:~# iptables -t nat -L -n -v
Chain PREROUTING (policy ACCEPT 374 packets, 22068 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 DNAT       tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:28443 to:12.0.0.10:28443
  163 38258 DNAT       udp  --  *      *       0.0.0.0/0            0.0.0.0/0            udp dpt:28650 to:12.0.0.10:28650
  982 84846 DNAT       udp  --  *      *       0.0.0.0/0            0.0.0.0/0            udp dpts:16384:32768 to:12.0.0.10
   11 11533 DNAT       udp  --  *      *       0.0.0.0/0            0.0.0.0/0            udp dpt:3911 to:xxxx:3911
    0     0 DNAT       udp  --  *      *       0.0.0.0/0            0.0.0.0/0            udp dpt:3911 to:xxxx:3911

Chain INPUT (policy ACCEPT 335 packets, 19732 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain OUTPUT (policy ACCEPT 375 packets, 22846 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain POSTROUTING (policy ACCEPT 375 packets, 22846 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 MASQUERADE  tcp  --  *      *       0.0.0.0/0            12.0.0.10            tcp dpt:28443
  163 38258 MASQUERADE  udp  --  *      *       0.0.0.0/0            12.0.0.10            udp dpt:28650
  982 84846 MASQUERADE  udp  --  *      *       0.0.0.0/0            12.0.0.10            udp dpts:16384:32768
    4  4239 MASQUERADE  udp  --  *      *       0.0.0.0/0            x.x.x.x         udp dpt:3911


## 业务机器 
网络类型：纯内网
root@call-fs-008:~# iptables -t nat -L -n -v
Chain PREROUTING (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
17343 1447K DOCKER     all  --  *      *       0.0.0.0/0            0.0.0.0/0            ADDRTYPE match dst-type LOCAL
    0     0 DNAT       all  --  *      *       125.64.14.31         0.0.0.0/0            to:12.0.0.10
    0     0 DNAT       all  --  *      *       12.0.0.10            0.0.0.0/0            to:xxxx

Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
   11 11533 DNAT       udp  --  *      *       0.0.0.0/0            xxxx         udp dpt:3911 to:10.0.0.50

Chain POSTROUTING (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
   11 11533 MASQUERADE  udp  --  *      *       0.0.0.0/0            10.0.0.50            udp dpt:3911

Chain DOCKER (1 references)
 pkts bytes target     prot opt in     out     source               destination
    0     0 RETURN     all  --  docker0 *       0.0.0.0/0            0.0.0.0/0
