# Mise en place de la topologie et routage
  - [I. Setup GNS3](#i-setup-gns3)
  - [II. Routes routes routes](#ii-routes-routes-routes)
  - [Potit bilan](#potit-bilan)

### Tableau d'adressage

| Machine          | Réseau 1        | Réseau 2        | Réseau 3        |
| ---------------- | --------------- | --------------- | --------------- |
| `node1.net1.tp3` | `10.3.1.11/24`  | nop             | nop             |
| `node2.net1.tp3` | `10.3.1.12/24`  | nop             | nop             |
| `router1.tp3`    | `10.3.1.254/24` | nop             | `10.3.100.1/30` |
| `router2.tp3`    | nop             | `10.3.2.254/24` | `10.3.100.2/30` |
| `node1.net2.tp3` | nop             | `10.3.2.11/24`  | nop             |
| `node2.net2.tp3` | nop             | `10.3.2.12/24`  | nop             |

## I. Setup GNS3

🌞 **Mettre en place la topologie dans GS3**

**Réseau 1**

```bash
[dorian@node1net1 ~]$ ping 10.3.1.12
PING 10.3.1.12 (10.3.1.12) 56(84) bytes of data.
64 bytes from 10.3.1.12: icmp_seq=1 ttl=64 time=1.98 ms
64 bytes from 10.3.1.12: icmp_seq=2 ttl=64 time=1.65 ms
^C
--- 10.3.1.12 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 1.646/1.811/1.977/0.165 ms

[dorian@node1net1 ~]$ ping 10.3.1.254
PING 10.3.1.254 (10.3.1.254) 56(84) bytes of data.
64 bytes from 10.3.1.254: icmp_seq=1 ttl=64 time=1.79 ms
64 bytes from 10.3.1.254: icmp_seq=2 ttl=64 time=1.53 ms
^C
--- 10.3.1.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 1.530/1.660/1.791/0.130 ms
```

```bash
[dorian@node2net1 ~]$ ping 10.3.1.254
PING 10.3.1.254 (10.3.1.254) 56(84) bytes of data.
64 bytes from 10.3.1.254: icmp_seq=1 ttl=64 time=1.69 ms
64 bytes from 10.3.1.254: icmp_seq=2 ttl=64 time=1.57 ms
^C
--- 10.3.1.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 1.567/1.628/1.689/0.061 ms
```

**Réseau 2**
```bash
[dorian@node1tp2 ~]$ ping 10.3.2.11
PING 10.3.2.11 (10.3.2.11) 56(84) bytes of data.
64 bytes from 10.3.2.11: icmp_seq=1 ttl=64 time=0.051 ms
64 bytes from 10.3.2.11: icmp_seq=2 ttl=64 time=0.077 ms
^C
--- 10.3.2.11 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1047ms
rtt min/avg/max/mdev = 0.051/0.064/0.077/0.013 ms

[dorian@node1tp2 ~]$ ping 10.3.2.254
PING 10.3.2.254 (10.3.2.254) 56(84) bytes of data.
64 bytes from 10.3.2.254: icmp_seq=1 ttl=64 time=2.34 ms
64 bytes from 10.3.2.254: icmp_seq=2 ttl=64 time=1.74 ms
^C
--- 10.3.2.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 1.738/2.041/2.344/0.303 ms
```

```bash
[dorian@node2tp2 ~]$ ping 10.3.2.254
PING 10.3.2.254 (10.3.2.254) 56(84) bytes of data.
64 bytes from 10.3.2.254: icmp_seq=1 ttl=64 time=1.67 ms
64 bytes from 10.3.2.254: icmp_seq=2 ttl=64 time=1.46 ms
^C
--- 10.3.2.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 1.461/1.563/1.665/0.102 ms
```

**Réseau 3**

```bash
[dorian@router1 ~]$ ping 10.3.100.2
PING 10.3.100.2 (10.3.100.2) 56(84) bytes of data.
64 bytes from 10.3.100.2: icmp_seq=1 ttl=64 time=1.56 ms
64 bytes from 10.3.100.2: icmp_seq=2 ttl=64 time=1.41 ms
^C
--- 10.3.100.2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1005ms
rtt min/avg/max/mdev = 1.413/1.484/1.555/0.071 ms
```

**Internet `router1.tp3`**

```bash
[dorian@router1 ~]$ ping google.com
PING google.com (172.217.20.174) 56(84) bytes of data.
64 bytes from waw02s07-in-f14.1e100.net (172.217.20.174): icmp_seq=1 ttl=114 time=15.4 ms
64 bytes from waw02s07-in-f14.1e100.net (172.217.20.174): icmp_seq=2 ttl=114 time=17.0 ms
^C
--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 15.359/16.185/17.011/0.826 ms

[dorian@router1 ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=114 time=15.6 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=114 time=17.2 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 15.570/16.401/17.233/0.831 ms
```

## II. Routes routes routes

🌞 **Activer le routage sur les deux machines `router`**

**J'active le routage sur `router1` et `router2`**
```bash
sudo sysctl -w net.ipv4.ip_forward=1

sudo firewall-cmd --add-masquerade

sudo firewall-cmd --add-masquerade --permanent
```

🌞 **Mettre en place les routes locales**

```bash
[dorian@router1 ~]$ cat /etc/sysconfig/network-scripts/route-enp0s8 
10.3.2.0/24 via 10.3.100.2

[dorian@router2 ~]$ sudo ip route add 10.3.2.0/24 via 10.3.100.2 dev enp0s8
```

```bash
[dorian@router2 ~]$ sudo ip route add 10.3.1.0/24 via 10.3.100.1 dev enp0s3

[dorian@router2 ~]$ sudo cat /etc/sysconfig/network-scripts/route-enp0s3
10.3.1.0/24 via 10.3.100.1

[dorian@router2 ~]$ ping 10.3.2.11
PING 10.3.2.11 (10.3.2.11) 56(84) bytes of data.
64 bytes from 10.3.2.11: icmp_seq=1 ttl=64 time=1.38 ms
64 bytes from 10.3.2.11: icmp_seq=2 ttl=64 time=1.54 ms
64 bytes from 10.3.2.11: icmp_seq=3 ttl=64 time=1.76 ms
^C
--- 10.3.2.11 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 1.378/1.559/1.762/0.157 ms
```

```bash
[dorian@node1net1 ~]$ sudo cat /etc/sysconfig/network-scripts/route-enp0s3
10.3.2.0/24 via 10.3.1.254

[dorian@node1net1 ~]$ sudo ip route add 10.3.2.0/24 via 10.3.1.254 dev enp0s3

[dorian@node1net1 ~]$ ping 10.3.2.11
PING 10.3.2.11 (10.3.2.11) 56(84) bytes of data.
64 bytes from 10.3.2.11: icmp_seq=1 ttl=62 time=5.80 ms
64 bytes from 10.3.2.11: icmp_seq=2 ttl=62 time=4.14 ms
^C
--- 10.3.2.11 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 4.139/4.967/5.795/0.828 ms
```

```bash
[dorian@node2net1 ~]$ sudo cat /etc/sysconfig/network-scripts/route-enp0s3
10.3.2.0/24 via 10.3.1.254

[dorian@node2net1 ~]$ sudo ip route add 10.3.2.0/24 via 10.3.1.254 dev enp0s3

[dorian@node2net1 ~]$ ping 10.3.2.11
PING 10.3.2.11 (10.3.2.11) 56(84) bytes of data.
64 bytes from 10.3.2.11: icmp_seq=1 ttl=62 time=4.65 ms
64 bytes from 10.3.2.11: icmp_seq=2 ttl=62 time=3.61 ms
^C
--- 10.3.2.11 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 3.614/4.131/4.648/0.517 ms
```

```bash
[dorian@node1net2 ~]$ sudo ip route add 10.3.1.0/24 via 10.3.2.254 dev enp0s3

[dorian@node1net2 ~]$ sudo cat /etc/sysconfig/network-scripts/route-enp0s3
10.3.1.0/24 via 10.3.2.254

[dorian@node1net2 ~]$ ping 10.3.1.11
PING 10.3.1.11 (10.3.1.11) 56(84) bytes of data.
64 bytes from 10.3.1.11: icmp_seq=1 ttl=62 time=3.63 ms
64 bytes from 10.3.1.11: icmp_seq=2 ttl=62 time=3.44 ms
^C
--- 10.3.1.11 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 3.436/3.532/3.628/0.096 ms
```

```bash
[dorian@node2net2 ~]$ sudo ip route add 10.3.1.0/24 via 10.3.2.254 dev enp0s3

[dorian@node2net2 ~]$ sudo cat /etc/sysconfig/network-scripts/route-enp0s3
10.3.1.0/24 via 10.3.2.254

[dorian@node2net2 ~]$ ping 10.3.1.11
PING 10.3.1.11 (10.3.1.11) 56(84) bytes of data.
64 bytes from 10.3.1.11: icmp_seq=1 ttl=62 time=1.90 ms
64 bytes from 10.3.1.11: icmp_seq=2 ttl=62 time=3.84 ms
^C
--- 10.3.1.11 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 1.895/2.865/3.835/0.970 ms
```

🌞 **Mettre en place les routes par défaut**
```bash
[dorian@node1net1 ~]$ echo 'GATEWAY=10.3.1.254' | sudo tee /etc/sysconfig/network

[dorian@node2net1 ~]$ echo 'GATEWAY=10.3.1.254' | sudo tee /etc/sysconfig/network

[dorian@node1net2 ~]$ echo 'GATEWAY=10.3.2.254' | sudo tee /etc/sysconfig/network

[dorian@node2net2 ~]$ echo 'GATEWAY=10.3.2.254' | sudo tee /etc/sysconfig/network

[dorian@router2 ~]$ echo 'GATEWAY=10.3.100.1' | sudo tee /etc/sysconfig/network
```

**Je reboot mes machines**

- faire en sorte que toutes les machines de votre topologie aient un accès internet, il faut donc :
  - sur les machines du réseau 1, ajouter `router.net1.tp3` comme passerelle par défaut
  - sur les machines du réseau 2, ajouter `router.net2.tp3` comme passerelle par défaut
  - sur l`router.net2.tp3`, ajouter `router1.net.tp3` comme passerelle par défaut
- prouvez avec un `ping` depuis `node1.net1.tp3` que vous avez bien un accès internet
- prouvez avec un `traceroute` depuis `node2.net1.tp3` que vous avez bien un accès internet, et que vos paquets transitent bien par `router2.tp3` puis par `router1.tp3` avant de sortir vers internet

> *Là encore, utilisez le [**mémo**](../../../memo/rocky_network.md) pour l'ajout de la route par défaut.*

Toutes les machines peuvent se joindre, et ont un accès internet. Yay.

![The siiize](../img/routing_table.jpg)

## Potit bilan

➜ Une fois cette section terminée, vous savez interconnecter autant de réseaux que nécessaires, de façon statique :

- **les routeurs sont l'élément central** : ils permettent aux paquets d'un réseau de passer vers un autre
- **pour pouvoir communiquer avec un autre réseau B, une machine doit :**
  - avoir une IP dans un réseau A
  - connaître l'IP d'un routeur qui lui aussi est dans le réseau A : il agira comme passerelle pour la machine
  - indiquer dans la table de routage de la machine qu'il existe une route vers le réseau B, en passant par la passerelle du réseau A
- **peu importe qu'il y ait des réseaux intermédiaires entre A et B : la machine cliente n'a pas besoin de le savoir**, elle n'a besoin que de connaître sa passerelle !

> *En effet, dans notre exemple, aucune des machines du Réseau 1 ou du Réseau 2 ne peut joindre les IPs du Réseau 3. Pourtant des paquets transitent par ce réseau quand le Réseau 1 et le Réseau 2 échangent des paquets, ou même quand les memebres du Réseau 2 vont sur internet.*

On peut passer à la suite : [config des services réseau](../network_services/README.md).