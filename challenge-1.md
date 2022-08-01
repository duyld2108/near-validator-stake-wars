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

**Image screen Ubuntu**

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

##### GO TO NEXT: [CHALLENGE-2](/challenge-2.md)!!!
