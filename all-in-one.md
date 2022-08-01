### \#CHALLENGE-001 - CREATE WALLET, SERVER AND DEPLOY THE NEAR-CLI

**Step 1. Create your shardnet wallet**

Go to [https://wallet.shardnet.near.org/](https://wallet.shardnet.near.org/) and hit **Create Account**

![Create Wallet 1](/images/challenge1/create-wallet-1.png)

Then choose your ID and hit **Reserve My Account ID**

![Create Wallet 2](/images/challenge1/create-wallet-2.png)

Make sure selection is **Secure Passphrase** then hit **Continue**

![Create Wallet 3](/images/challenge1/create-wallet-3.png)

Hit the **Copy** button, then save your passphrase and click **Continue**

![Create Wallet 3](/images/challenge1/create-wallet-4.png)

Enter the word you have just saved then click **Verify & Complete** to complete creat account

![Create Wallet 4](/images/challenge1/create-wallet-5.png)

_(In this example, I need to type the word number #12)_

After **SHARDNET** account created, you will receive **50 NEAR** to join network

**Step 2. Rent a VPS**

First, you need to know about hardware requirements

| Hardware       | Chunk-Only Producer  Specifications                                   |
| -------------- | ---------------------------------------------------------------       |
| CPU            | 4-Core CPU with AVX support                                           |
| RAM            | 8GB DDR4                                                              |
| Storage        | 500GB SSD                                                             |

Before do next step, you need rent a VPS (Virtual Private Sever). I recommend to choose [Contabo](https://contabo.com/en/vps) because it's cheap with the hardware that event requires. You can see below:

![contabo-review.png](/images/challenge1/contabo-review.png)

Go to [https://contabo.com/en/vps](https://contabo.com/en/vps) and choose **CLOUD VPS M** as below:

![contabo-vps-choose.png](/images/challenge1/contabo-vps-choose.png)

Keep the default config as:
* **Region**: Choose your region you want. Keep ***European Union (Germany)*** for free
* **Storage Type**: Keep 400 GB SSD or increase to 600 GB SSD (increase $2.50/month)
* **Image**: I choosed Ubuntu 20.04

Fill the password and click **Next**

![contabo-vps-config.png](/images/challenge1/contabo-vps-config.png)

Fill out the form to create new account if you are new customer, or you can change to **I’m a Contabo Customer"** tab to login if you have account on Contabo before. Then click **Next**

![contabo-vps-register.png](/images/challenge1/contabo-vps-register.png)

Next, enter your payment information (Credit Card or Paypal). Then click **Next**.

![contabo-vps-payment.png](/images/challenge1/contabo-vps-payment.png)

Review then click **Order & Pay** to complete purchase VPS

![contabo-vps-place-order.png](/images/challenge1/contabo-vps-place-order.png)

Then wait for Contabo to send you the login info in you email.

![contabo-vps-login-info.png](/images/challenge1/contabo-vps-login-info.png)

Use any SSH client you want (e.g. Terminal, [Putty](https://www.putty.org/), [Termius](https://termius.com/)...) to login to your VPS.

![ubuntu-screen.jpg](/images/challenge1/ubuntu-screen.jpg)

**Step 3. Config environments, setup dependencies to install NEAR**

***3.1. First, Update your VPS server***
```
sudo apt update && sudo apt upgrade -y
```
![Update OS](/images/challenge1/1-update-ubuntu.jpg)

***3.2. Next, Install Nodejs and NPM***
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![Install Nodejs](/images/challenge1/2-install-nodejs.jpg)
```
sudo apt install build-essential nodejs
```
![Install Nodejs](/images/challenge1/3-install-nodejs-2.jpg)

Hit **"Y"** and **Enter**

```
PATH="$PATH"
```
![Set PATH](/images/4-path.jpg)

***3.3. Next, Install NEAR CLI***
```
sudo npm install -g near-cli
```
![Install NEAR cli](/images/challenge1/5-install-near-cli.jpg)

![Install NEAR cli](/images/challenge1/5-install-near-cli-2.jpg)

***3.4. Create Shardnet Environment***
```
export NEAR_ENV=shardnet
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
echo 'export NEAR_ENV=shardnet' >> ~/.bash_profile
source $HOME/.bash_profile
```
![Create Shardnet Environment](/images/challenge1/6-create-shardnet-environment.jpg)

**NOW WE ARE COMPLETED CHALLENGE-001**

### \#CHALLENGE-002 - DEPLOY THE NODE AND ACTIVE AS VALIDATOR

**Step 1: Make sure your VPS’s CPU is supported**
```
lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null \
  && echo "Supported" \
  || echo "Not supported"
```
![C2-check-cpu-support.jpg](/images/challenge2/C2-check-cpu-support.jpg)

The result is **"Supported"** then you good to go

**Step 2: Install dependencies**

***First, install developer tools***
```
sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo
```
![C2-install-developer-tools.jpg](/images/challenge2/C2-install-developer-tools.jpg)

***Next, install the Python pip***
```
sudo apt install -y python3-pip
```
![C2-install-python-pip.jpg](/images/challenge2/C2-install-python-pip.jpg)

***Continue, set the Configuration***
```
USER_BASE_BIN=$(python3 -m site --user-base)/bin
export PATH="$USER_BASE_BIN:$PATH"
```
![C2-set-the-configuration.jpg](/images/challenge2/C2-set-the-configuration.jpg)

***Install Building env***
```
sudo apt install -y clang build-essential make
```
![C2-install-building-env.jpg](/images/challenge2/C2-install-building-env.jpg)

***Next, Install Rush & Cargo***
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
![C2-install-rush-and-cargo.jpg](/images/challenge2/C2-install-rush-and-cargo.jpg)

Hit **"Y"** and **Enter**

![C2-install-rush-and-cargo-2.jpg](/images/challenge2/C2-install-rush-and-cargo-2.jpg)

Enter **"1"** then **Enter**

![C2-install-rush-and-cargo-3.jpg](/images/challenge2/C2-install-rush-and-cargo-3.jpg)

Then Source the environment

![C2-install-rush-and-cargo-4.jpg](/images/challenge2/C2-install-rush-and-cargo-4.jpg)

**Step 3: Install NEARCore**

***3.1. Clone NEARCore Project from GitHub***

First, clone the NEARCore repository
```
git clone https://github.com/near/nearcore
cd nearcore
git fetch
```
![C2-clone-nearcore](/images/challenge2/C2-clone-nearcore.jpg)

Checkout to the commit needed. Please refer to the commit defined in [this file](https://github.com/near/stakewars-iii/blob/main/commit.md).
```
git checkout <commit>
```
At this time (Aug 1st, 2022), it will be
```
git checkout c1b047b8187accbf6bd16539feb7bb60185bdc38
```
![C2-git-checkout.jpg](/images/challenge2/C2-git-checkout.jpg)

***3.2. Compile NEARCore binany***
```
cargo build -p neard --release --features shardnet
```
![C2-compile-nearcore](/images/challenge2/C2-compile-nearcore.jpg)

It will take 7-10 minutes to complete

![C2-compile-nearcore-2](/images/challenge2/C2-compile-nearcore-2.jpg)

***3.3. Initialize working directory***
```
./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis
```
![C2-initialize-working-directory.jpg](/images/challenge2/C2-initialize-working-directory.jpg)

Replace the config.json
```
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
```
![C2-replace-config-json.jpg](/images/challenge2/C2-replace-config-json.jpg)

> **At this time, because after Hardfork on Shardnet during 2022-07-18, we not required to get the lastest snapshot**

***3.4. Run the Node***

Come back to nearcore folder
```
cd ~/nearcore
```
It takes time to run and sync the node to the actual state, so to keep it running I use **tmux**
```
sudo apt install -y tmux
```
![C2-install-tmux.jpg](/images/challenge2/C2-install-tmux.jpg)

Then call tmux
```
tmux
```
It will show up like this

![C2-tmux-screen.jpg](/images/challenge2/C2-tmux-screen.jpg)

> If you are not at nearcore folder, you have to move to that folder using this command (cd ~/nearcore)

then run the node with this command:
```
./target/release/neard --home ~/.near run
```
![C2-downloading-headers.jpg](/images/challenge2/C2-downloading-headers.jpg)

while wait the node to fully sync, we will do next step. But first we have to **detach tmux** by hit **Ctrl + B**, then press **D** (if you use the MacOS, you have to hit Command and B at the same time then let go and hit D) then we can detact the tmux session.

![C2-tmux-detached.jpg](/images/challenge2/C2-tmux-detached.jpg)

**Step 4: Activation the node as validator**

A full access key needs to be installed locally to be able to sign transactions via **NEAR-CLI**.

Run this commad
```
near login
```
![C2-near-login.jpg](/images/challenge2/C2-near-login.jpg)

Hit **"Y"** and **Enter**

Then copy the URL, paste it in the browser (which we have created/login the wallet before)

![C2-near-login-2.jpg](/images/challenge2/C2-near-login-2.jpg)

Then hit **"Next"** and **"Connect"**

![C2-near-login-3.jpg](/images/challenge2/C2-near-login-3.jpg)

enter your account wallet (i.e. _xx_.shardnet.near) and hit **"Confirm"**

![C2-near-login-4.jpg](/images/challenge2/C2-near-login-4.jpg)

The browser will show up like this

![C2-near-login-local.jpg](/images/challenge2/C2-near-login-local.jpg)

Come back to the SSH terminal, enter your account wallet and hit **Enter**

![C2-near-login-5.jpg](/images/challenge2/C2-near-login-5.jpg)

![C2-near-login-6.jpg](/images/challenge2/C2-near-login-6.jpg)

when **successfully**, Hooyah! we are good to go.

**Step 5: Check the _validator_key.json_**
```
cat ~/.near/validator_key.json
```
> ***NOTE:*** If a `validator_key.json` is not present, follow these steps to create one

Create a `validator_key.json`

* Generate the Key file:
```
near generate-key <pool_id>
```
Replace the <pool_id> with your pool ID, for example
```
near generate-key duyld2108.factory.shardnet.near
```
![C2-create-validator-key-json.jpg](/images/challenge2/C2-create-validator-key-json.jpg)

* Copy the file generated to shardnet folder:
Make sure to replace <pool_id> by your accountId
```
cp ~/.near-credentials/shardnet/YOUR_WALLET.json ~/.near/validator_key.json
```
For example:
```
cp ~/.near-credentials/shardnet/duyld2108.shardnet.near.json ~/.near/validator_key.json
```
![C2-create-validator-key-json-2.jpg](/images/challenge2/C2-create-validator-key-json-2.jpg)

Next, edit the `validator_key.json` as below:
* Edit “account_id” => xx.factory.shardnet.near, where xx is your PoolName
* Change `private_key` to `secret_key`
Use this command:
```
sudo nano ~/.near/validator_key.json
```
![C2-edit-validator-key.jpg](/images/challenge2/C2-edit-validator-key.jpg)

change to this

![C2-edit-validator-key-2.jpg](/images/challenge2/C2-edit-validator-key-2.jpg)

and this

![C2-edit-validator-key-3.jpg](/images/challenge2/C2-edit-validator-key-3.jpg)

then hit **Ctrl + O** to write out and hit **Ctrl + X** to exit

**Step 6: Setup System Command**

Run this command:
```
sudo nano /etc/systemd/system/neard.service
```
and paste this content:
```
[Unit]
Description=NEARd Daemon Service

[Service]
Type=simple
User=<USER>
#Group=near
WorkingDirectory=/home/<USER>/.near
ExecStart=/home/<USER>/nearcore/target/release/neard run
Restart=on-failure
RestartSec=30
KillSignal=SIGINT
TimeoutStopSec=45
KillMode=mixed

[Install]
WantedBy=multi-user.target
```
**_Note_**: Change \<USER\> to your paths

In this example:
![C2-create-system-command.jpg](/images/challenge2/C2-create-system-command.jpg)

then hit **Ctrl + O** to write out and hit **Ctrl + X** to exit

Then wait for the node to sync. To check how the node is syncing attach Tmux again.

Use this command:
```
tmux attach - t 0
```
When you see this, it means it fully syncs

![C2-neard-log.jpg](/images/challenge2/C2-neard-log.jpg)

We will detach (**Ctrl + B**, then hit **D**) and kill this tmux session then start _"neard"_. Use this command:
```
tmux kill-session - t 0
```
**Start neard**
```
sudo systemctl enable neard
sudo systemctl start neard
```
then check log
```
journalctl -n 100 -f -u neard
```
To get out, hit **Ctrl + C**

To make logs output in pretty print. Install `ccze`:
```
sudo apt install -y ccze
```
![C2-install-ccze.jpg](/images/challenge2/C2-install-ccze.jpg)

Then view logs with colors pretty
```
journalctl -n 100 -f -u neard | ccze -A
```
![C2-neard-log-ccze.jpg](/images/challenge2/C2-neard-log-ccze.jpg)

To get out, hit **Ctrl + C**

**NOW WE ARE COMPLETED CHALLENGE-002**

### \#CHALLENGE-003 - DEPLOY THE NEW STAKING POOL

**Step 1: Deploy a Staking Pool Contract**

Use this command:
```
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "<pool id>", "owner_id": "<accountId>", "stake_public_key": "<public key>", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="<accountId>" --amount=30 --gas=300000000000000
```
From the example command above, you need to replace:
* **\<pool id\>**: The pool name, e.g. duyld. It will create the pool: duyld.factory.shardnet.near
* **\<accountId\>**: The SHARDNET account (i.e. duyld2108.shardnet.near).
* **\<public key\>**: The public key in your `validator_key.json` file. To view your public key, use this command: `cat ~/.near/validator_key.json`
* **numerator is 5**: It's mean you will take 5% fee on staking reward.

For example:
```
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "duyld", "owner_id": "duyld2108.shardnet.near", "stake_public_key": "ed25519:BRBHTsgp9WMNMBQxAqMBwnNi9ZoBVqgkYdRqsQHWYWxa", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="duyld2108.shardnet.near" --amount=30 --gas=300000000000000
```
As command above, we will create a new staking pool name **_duyld.factory.shardnet.near_** with **_5%_** of fees, managed by account **_duyld210.shardnet.near_**

![create-staking-pool.png](/images/challenge3/create-staking-pool.png)

**Step 2: Request deposit NEAR to staking pool**

Because there are too many accounts that cheat on this test network, so NEAR team has changed the process. Now you need to go to Discord of NEAR, the [**#stake-wars-tokens_delegation**](https://discord.gg/fBd4y4EuB3) channel. Submit your **shardnet wallet** address and **pool id** in the channel, the team will send enough NEARs to your validator.

At this time, you can not check your validator at ~~[https://explorer.shardnet.near.org/nodes/validators](https://explorer.shardnet.near.org/nodes/validators)~~ because it's not updated.

**Step 3: Check your validator active**

After receive deposit from NEAR team to your pool, you can check your validator by this command:
```
near validators next | grep <your pool id>
```
Example:
```
near validators next | grep duyld
```
![validators-next.png](/images/challenge3/validators-next.png)

If get result as image above. **Congratulation! you are successfully create a Node Validator!!!**

To deposit more NEAR from SHARDNET wallet to your staking pool, use this command:
```
near call <staking_pool_id> deposit_and_stake --amount <amount> --accountId <accountId> --gas=300000000000000
```
With:
* **\<staking_pool_id\>**: your staking pool. e.g. duyld.factory.shardnet.near
* **\<amount\>**: amount of NEAR you want to stake.
* **\<accountId\>**: The SHARDNET account (i.e. duyld2108.shardnet.near).

**Step 4: Ping to keep validator in list**
Command:
```
crontab -e
```
Enter this command to end of file to keep "ping" every 2 hours.
```
0 */2 * * * export NEAR_ENV=shardnet && near call <staking_pool_id>  ping '{}' --accountId <accountId> --gas=300000000000000
```
* **\<staking_pool_id\>**: your staking pool. e.g. duyld.factory.shardnet.near
* **\<accountId\>**: The SHARDNET account (i.e. duyld2108.shardnet.near).

![ping-validator.png](/images/challenge3/ping-validator.png)

To check, visit Shardnet explorer at: [https://explorer.shardnet.near.org/](https://explorer.shardnet.near.org/), enter your SHARDNET wallet to see "ping" command run every 2 hours.

![ping-1.png](/images/challenge3/ping-1.png)

![ping-2.png](/images/challenge3/ping-2.png)

**NOW WE ARE COMPLETED CHALLENGE-003**

### \#CHALLENGE-004 - SETUP MONITORING TOOLS FOR NODE STATUS

In Challenge 002, we knew how to check `neard` logs by use this command:
```
journalctl -n 100 -f -u neard | ccze -A
```
![](/images/challenge2/C2-neard-log-ccze.jpg)

Now we will install RPC for node status update. Install by:
```
sudo apt install -y curl jq
```

##### To check your node version
```
curl -s http://127.0.0.1:3030/status | jq .version
```

You could get your validator node status update with this scripts:
* Get update via Telegeram: [https://github.com/Klesh-/near-protocol-node-telegram-notifications](https://github.com/Klesh-/near-protocol-node-telegram-notifications)
* Dashboard monitor network: [https://github.com/Klesh-/near-protocol-node-monitoring](https://github.com/Klesh-/near-protocol-node-monitoring)
