### \#CHALLENGE-004 - SETUP MONITORING TOOLS FOR NODE STATUS

##### BEFORE THIS CHALLENGE, BE SURE YOU HAVE COMPLETED:
* [CHALLENGE-1](/challenge-1.md)
* [CHALLENGE-2](/challenge-2.md)
* and [CHALLENGE-3](/challenge-3.md) BEFORE!!!

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

**NOW WE ARE COMPLETED CHALLENGE-003**
