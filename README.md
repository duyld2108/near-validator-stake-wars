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

| Hardware       | Chunk-Only Producer  Specifications                                   |
| -------------- | ---------------------------------------------------------------       |
| CPU            | 4-Core CPU with AVX support                                           |
| RAM            | 8GB DDR4                                                              |
| Storage        | 500GB SSD                                                             |

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

***Initialize working directory***
```
./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis
```
![C2-initialize-working-directory.jpg](/images/C2-initialize-working-directory.jpg)

Replace the config.json
```
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
```
![C2-replace-config-json.jpg](/images/C2-replace-config-json.jpg)

**At this time, because after Hardfork on Shardnet during 2022-07-18, we not required to get the lastest snapshot**

***Run the Node***

Come back to nearcore folder
```
cd ~/nearcore
```
It takes time to run and sync the node to the actual state, so to keep it running I use **tmux**
```
sudo apt install -y tmux
```
![C2-install-tmux.jpg](/images/C2-install-tmux.jpg)

Then call tmux
```
tmux
```
It will show up like this
![C2-tmux-screen.jpg](/images/C2-tmux-screen.jpg)

> If you are not at nearcore folder, you have to move to that folder using this command (cd ~/nearcore)

then run the node with this command:
```
./target/release/neard --home ~/.near run
```
![C2-downloading-headers.jpg](/images/C2-downloading-headers.jpg)

while wait the node to fully sync, we will do next step. But first we have to **detach tmux** by hit **Ctrl + B**, then press **D** (if you use the MacOS, you have to hit Command and B at the same time then let go and hit D) then we can detact the tmux session.

![C2-tmux-detached.jpg](/images/C2-tmux-detached.jpg)

** NEXT: We activation the node as validator**

A full access key needs to be installed locally to be able to sign transactions via NEAR-CLI.

Run this commad
```
near login
```
![C2-near-login.jpg](/images/C2-near-login.jpg)

Hit **"Y"** and **Enter**

Then copy the URL, paste it in the browser (which we have created/login the wallet before)

![C2-near-login-2.jpg](/images/C2-near-login-2.jpg)

Then hit **"Next"** and **"Connect"**

![C2-near-login-3.jpg](/images/C2-near-login-3.jpg)

enter your account wallet (eg. _xx_.shardnet.near) and hit **"Confirm"**

![C2-near-login-4.jpg](/images/C2-near-login-4.jpg)

The browser will show up like this

![C2-near-login-local.jpg](/images/C2-near-login-local.jpg)

Come back to the SSH terminal, enter your account wallet and hit **Enter**

![C2-near-login-5.jpg](/images/C2-near-login-5.jpg)

![C2-near-login-6.jpg](/images/C2-near-login-6.jpg)

when **successfully**, Hooyah! we are good to go.

**Check the _validator_key.json_**
```
cat ~/.near/validator_key.json
```
> ***NOTE:*** If a validator_key.json is not present, follow these steps to create one

Create a `validator_key.json`

* Generate the Key file:
```
near generate-key <pool_id>
```
Replace the <pool_id> with your pool ID, for example
```
near generate-key duyld2108.factory.shardnet.near
```
![C2-create-validator-key-json.jpg](/images/C2-create-validator-key-json.jpg)

* Copy the file generated to shardnet folder:
Make sure to replace <pool_id> by your accountId
```
cp ~/.near-credentials/shardnet/YOUR_WALLET.json ~/.near/validator_key.json
```
For example:
```
cp ~/.near-credentials/shardnet/duyld2108.shardnet.near.json ~/.near/validator_key.json
```
![C2-create-validator-key-json-2.jpg](/images/C2-create-validator-key-json-2.jpg)

Next, edit the `validator_key.json` as below:
* Edit “account_id” => xx.factory.shardnet.near, where xx is your PoolName
* Change `private_key` to `secret_key`
Use this command:
```
sudo nano ~/.near/validator_key.json
```
![C2-edit-validator-key.jpg](/images/C2-edit-validator-key.jpg)

change to this

![C2-edit-validator-key-2.jpg](/images/C2-edit-validator-key-2.jpg)

and this

![C2-edit-validator-key-3.jpg](/images/C2-edit-validator-key-3.jpg)

then hit **Ctrl + O** to write out and hit **Ctrl + X** to exit

**Now setup System Command**
