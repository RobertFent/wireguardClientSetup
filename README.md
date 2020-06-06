# wireguardClientSetup
Client-side setup for wireguard on rpi

## prerequisite
wireguard setup on rpi:
```
sudo apt update
sudo apt upgrade
sudo apt install raspberrypi-kernel-headers
```

```
echo "deb http://httpredir.debian.org/debian buster-backports main contrib non-free" | sudo tee --append /etc/apt/sources.list.d/debian-backports.list
wget -O - https://ftp-master.debian.org/keys/archive-key-$(lsb_release -sr).asc | sudo apt-key add -
sudo apt update
sudo apt install wireguard
sudo reboot
```

## how to use
1. Run server-side setup for wireguard: https://github.com/RobertFent/wireguardServerSetup
2. Copy client-wg0.conf.tar.gz from server:
```
scp robert@136.244.85.157:/home/robert/repos/wireGuardServerSetup/client-wg0.conf.tar.gz /home/pi/repos/wireGuardClientSetup
```
3. Extract client-wg0.conf
```
tar -zxvf client-wg0.conf.tar.gz
```
4. Build docker image
```
docker build -t robertfent1/wireguard-rpi-client --file ./DockerFile .
```
5. Run ```up``` in the folder where docker-compose.yml is

## how to test
1. Ip addresses
```
ip a show wg0
```
should print:
```
wg0: <POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1420 qdisc noqueue state UNKNOWN group default qlen 1000
link/none
inet 10.0.0.2/24 scope global wg0
   valid_lft forever preferred_lft forever
```

2. Ping server
```
ping 10.0.0.1
```
should print something like this:
```
PING 10.0.0.1 (10.0.0.1) 56(84) bytes of data.
64 bytes from 10.0.0.1: icmp_seq=1 ttl=64 time=19.5 ms
64 bytes from 10.0.0.1: icmp_seq=2 ttl=64 time=18.7 ms
64 bytes from 10.0.0.1: icmp_seq=3 ttl=64 time=19.5 ms
64 bytes from 10.0.0.1: icmp_seq=4 ttl=64 time=18.7 ms
```
