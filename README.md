# How to become a NEAR validator - Stake wars (Step-by-step)

More informations about Stake Wars, please visit [here](https://near.org/stakewars/)

![Stake Wars episode III](/images/stake-wars-banner.png)

### \#CHALLENGE-001

**1. Create your shardnet wallet**
Go to [https://wallet.shardnet.near.org/](https://wallet.shardnet.near.org/) and hit **Create Account**

![Create Wallet 1](/images/create-wallet-1.png)

Then choose your ID and hit **Reserve My Account ID**

![Create Wallet 2](/images/create-wallet-2.png)

Make sure selection is **Secure Passphrase** then hit **Continue**

![Create Wallet 3](/images/create-wallet-3.png)

Hit the **Copy** button, then save your passphrase and click **Continue**

![Create Wallet 3](/images/create-wallet-4.png)

Enter the word you have just saved then click **Verify & Complete** to complete creat account

![Create Wallet 4](/images/create-wallet-5.png)

_(In this example, I need to type the word number #12)_

After **SHARDNET** account created, you will receive **50 NEAR** to join network

**2. Rent a VPS and login via SSH**

First, you need rent a VPS with minimum requirements

**Hardware**: Chunk-Only Producer Specifications

**CPU**: 4-Core CPU with AVX support

**RAM**: 8GB DDR4

**Storage**: 500GB SSD

Use any SSH client you want (eg. Terminal, Putty, Termius...) to login to your VPS.

**Image screen Ubuntu**

**3. Step-by-step to setup a validator and sync it to the actual state of the network**

***First, Update your VPS server***
```
sudo apt update && sudo apt upgrade -y
```
![Update OS](/images/1-update-ubuntu.jpg)

***Next, Install Nodejs and NPM***
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![Install Nodejs](/images/2-install-nodejs.jpg)
```
sudo apt install build-essential nodejs
```
![Install Nodejs](/images/3-install-nodejs-2.jpg)

Hit **"Y"** and **Enter**

```
PATH="$PATH"
```
![Set PATH](/images/4-path.jpg)

***Next, Install NEAR CLI***
```
sudo npm install -g near-cli
```
![Install NEAR cli](/images/5-install-near-cli.jpg)

![Install NEAR cli](/images/5-install-near-cli-2.jpg)

***Create Shardnet Environment***
```
export NEAR_ENV=shardnet
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
echo 'export NEAR_ENV=shardnet' >> ~/.bash_profile
source $HOME/.bash_profile
```
![Create Shardnet Environment](/images/6-create-shardnet-environment.jpg)

**NOW WE ARE COMPLETED CHALLENGE-001**

### \#CHALLENGE-002

Make sure your VPS’s CPU is supported
```
lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null \
  && echo "Supported" \
  || echo "Not supported"
```
