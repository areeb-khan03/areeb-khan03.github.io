# Cloud Wireguard VPN Using Docker

## Using Digital Ocean

1. Use the given [link](https://m.do.co/c/4d7f4ff9cfe4) to create a Digital Ocean account with $200 credit.
2. Create an account and add a payment method. 
3. Navigate to the control panel and click on **Droplets** on the left panel. 
4. Create a new Droplet. Click on **Marketplace** and choose the Docker on Ubuntu image. Change the CPU option to regular with SSD and select the $6/mo option. Set a root password and create the Droplet.

## Setting Up Wireguard
1. Create a directory of Wireguard and its config files using the following commands:
```Bash
mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
```
2. Create a **.yml** file and copy paste the following contents into the file. Make sure to change the **TZ** and **SERVERURL** appropriately. You can grab the server url from the droplet's **Networking** tab under the **Public IPV4 Address**.

```Bash
vim ~/wireguard/docker-compose.yml

version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
      - SERVERURL=143.244.182.158
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
```

3. Run the docker using the following command:

```Bash
cd ~/wireguard/
sudo docker-compose up -d
```

## Connecting to Phone

1. Use the following command to generate the QR code to connect to your phone.
```Bash
docker logs wireguard
```
2. Scan the QR code with the mobile **Wireguard** app to create the VPN. 

### Phone Active VPN Tunnel
![Imgur](https://i.imgur.com/eM587Iy.png)

### IP Address on Phone Before Enabling Wireguard
![Imgur](https://i.imgur.com/2S74vDg.png)

### IP Address on Phone After Enabling Wireguard
![Imgur](https://i.imgur.com/w3WAUP7.png)

## Connecting to PC

1. Change directory to **peer_pc1** in the config file.
```Bash
cd ~/wireguard/config/peer_pc1/
```
2. Print out the contents of the **peer_pc1.conf** file. Copy the text printed out. Create a new text document and paste the items into this document. Change the file extension from a **.txt** to a **.conf**.
3. Open the **Wireguard** application installed on your computer and import this document. Now you can successfully activate this VPN.

### Laptop Active VPN Tunnel
![Imgur](https://i.imgur.com/RPLMItu.png)

### IP Address on PC Before Enabling Wireguard
![Imgur](https://i.imgur.com/Rfjrd6h.png)

### IP Address on PC After Enabling Wireguard
![Imgur](https://i.imgur.com/6OS0v5x.png)
