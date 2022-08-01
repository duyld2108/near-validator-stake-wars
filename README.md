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
![C2-check-cpu-support.jpg](/images/C2-check-cpu-support.jpg)

The result is **"Supported"** then you good to go

***Next install developer tools***
```
sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo
```
![C2-install-developer-tools.jpg](/images/C2-install-developer-tools.jpg)

***Next, install the Python pip***
```
sudo apt install -y python3-pip
```
![C2-install-python-pip.jpg](/images/C2-install-python-pip.jpg)

***Continue, set the Configuration***
```
USER_BASE_BIN=$(python3 -m site --user-base)/bin
export PATH="$USER_BASE_BIN:$PATH"
```
![C2-set-the-configuration.jpg](/images/C2-set-the-configuration.jpg)

***Install Building env***
```
sudo apt install -y clang build-essential make
```
![C2-install-building-env.jpg](/images/C2-install-building-env.jpg)

***Next, Install Rush & Cargo***
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
![C2-install-rush-and-cargo.jpg](/images/C2-install-rush-and-cargo.jpg)

Hit **"Y"** and **Enter**

![C2-install-rush-and-cargo-2.jpg](/images/C2-install-rush-and-cargo-2.jpg)

Enter **"1"** then **Enter**

![C2-install-rush-and-cargo-3.jpg](/images/C2-install-rush-and-cargo-3.jpg)

Then Source the environment

![C2-install-rush-and-cargo-4.jpg](/images/C2-install-rush-and-cargo-4.jpg)

***Clone Nearcore Project from GitHub***

First, clone the nearcore repository
```
git clone https://github.com/near/nearcore
cd nearcore
git fetch
```
![C2-clone-nearcore](/images/C2-clone-nearcore.jpg)

Checkout to the commit needed. Please refer to the commit defined in [this file](https://github.com/near/stakewars-iii/blob/main/commit.md).
```
git checkout <commit>
```
At this time (Aug 1st, 2022), it will be
```
git checkout c1b047b8187accbf6bd16539feb7bb60185bdc38
```
![C2-git-checkout.jpg](/images/C2-git-checkout.jpg)

***Compile Nearcore binany***
```
cargo build -p neard --release --features shardnet
```
![C2-compile-nearcore](/images/C2-compile-nearcore.jpg)

It will take 7-10 minutes to complete

![C2-compile-nearcore-2](/images/C2-compile-nearcore-2.jpg)
