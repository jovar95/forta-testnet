# Forta Testnet

**Scan Node Requirements**


The following are the requirements for running a Forta scan node:

- 64-bit Linux distribution

- CPU with 4+ cores

- 16GB RAM

- Connection to Internet

- Docker v20.10+

- 100GB SSD (in addition to full node requirements)

- Recommended: Full node (any chain)


# Node Setup

First, we need to install docker: *copy and paste all*
```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-cache policy docker-ce
sudo systemctl status docker
```

For more details about [Install Docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)

Add a file called daemon.json to your /etc/docker directory with the following contents:

```bash
{
   "default-address-pools": [
        {
            "base":"172.17.0.0/12",
            "size":16
        },
        {
            "base":"192.168.0.0/16",
            "size":20
        },
        {
            "base":"10.99.0.0/16",
            "size":24
        }
    ]
}
```
This daemon.json should be located here

![image](https://user-images.githubusercontent.com/62967690/164976600-d1971074-bc20-43e6-b4a2-025505b34485.png)

Then restart docker with 
```bash
systemctl restart docker
```
Initialize Forta Directory (choose a passphrasse): *copy & paste directly to the terminal*

```bash
$ forta init --passphrase <your_passphrase> 
```

This generates a config directory, a private key, and output your a scaner address

Scanner address: 0xAAA8C491232cB65a65FBf7F36b71220B3E695AAA

Successfully initialized at /yourname/.forta

Then create fortad service

```bash
echo "[Unit]
Description=Forta
After=network.target

[Service]
Environment="FORTA_DIR=/root/.forta"
Environment="FORTA_PASSPHRASE=<your_passphrase>"
User=root
Type=simple
ExecStart=forta run
Restart=on-failure
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target" > /etc/systemd/system/fortad.service
```

Now, you need create an Ethereum mainnet API Key using Alchemy Growth plan and integrate your private keys to the `config.yml` file from the `root/.forta/`

Go to [AlchemyAPI](https://www.alchemy.com/pricing) and choose the **49$/month** plan (this plan allows to use trace_block function)

![iwqD5k92ww](https://user-images.githubusercontent.com/62967690/164980535-847b9e16-9323-4957-9b0c-c52c08e9b7d5.png)

Create your account and attach your credit card for the payment

<img width="523" alt="dcVG4rhnwQ" src="https://user-images.githubusercontent.com/62967690/164980668-6c9475af-83b1-47ae-8399-e7e5d2801ad3.png">

Once payment done you need to edit your Ethereum Mainnet App and take your KEY (https://):

<img width="918" alt="I8ZWVZAEeF" src="https://user-images.githubusercontent.com/62967690/164980858-bfef6d36-20fa-436f-abcc-ae7d4ed2cb3a.png">

Then you need to put your keys to the config.yml file on 8 and 13 lines:

<img width="432" alt="1LBgcl7iOb" src="https://user-images.githubusercontent.com/62967690/164981041-36b647b9-cfb4-4faf-ac1d-ac7560cbf8b0.png">

Then check docker

```bash
docker ps
```
Then enable service and restart

```bash
sudo systemctl daemon-reload
sudo systemctl enable fortad
sudo systemctl restart fortad

journalctl -u fortad -f
```

Then run forta

```bash
$ forta run --passphrase <your_passphrase>
```
Then check the status

```bash
forta status
```
You should have this output

![2PZBjpFKKq](https://user-images.githubusercontent.com/62967690/164981908-d7a8f957-3556-4b87-89ae-59396d879065.png)

Now you have to create a Metamusk Wallet Address on Polygon and fund it with 1 MATIC

Follow this [LINK](https://docs.polygon.technology/docs/develop/metamask/hello) to see tutorial

Once your Polygon wallet created on MetaMask and funded with 1 MATIC, run this command:

```bash
forta register --owner-address <Your_Polygon_Address> --passphrase <your_passphrase>
```

Once transaction finished you have to send **FORM** that you received by e-mail


I hope this guide helped you to run your node

Nick Kaufmann
