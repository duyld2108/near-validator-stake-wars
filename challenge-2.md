### \#CHALLENGE-002 - DEPLOY THE NODE AND ACTIVE AS VALIDATOR

##### BEFORE THIS CHALLENGE, BE SURE YOU HAVE COMPLETED [CHALLENGE-1](/challenge-1.md) BEFORE!!!

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

##### GO TO NEXT: [CHALLENGE-3](/challenge-3.md)!!!
