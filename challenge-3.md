### \#CHALLENGE-003 - DEPLOY THE NEW STAKING POOL

##### BEFORE THIS CHALLENGE, BE SURE YOU HAVE COMPLETED [CHALLENGE-1](/challenge-1.md) AND [CHALLENGE-2](/challenge-2.md) BEFORE!!!

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

##### GO TO NEXT: [CHALLENGE-4](/challenge-4.md)!!!
