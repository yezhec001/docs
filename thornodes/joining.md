---
description: How to join the system.
---

# Joining

## **Joining THORChain** 

Now that you have a THORNode deployed in your Kubernetes cluster, you need to start operating your node to join the network.

There are a couple of steps to follow to do so.

### 1. Check your current node status

The first step would be to make sure your deployment was successful and your node is running correctly. ****To check the current status of your node, you can run the command status from the \`helm-charts\` repository on your terminal:

```text
make status
```

  
You will get an output along those lines, the example below is for a testnet node:

```text
THORNode Status
Address   thor1ryr5eancepklax5am8mdpkx6mr0rg4xjnjx6zz
IP
Version   0.1.0
API       http://:1317
RPC       http://:26657
Midgard   http://:8080
Vault BNB Bond Address tbnb14z64mhgsnlayj28llzyehq7uh9e9u2ec2jzysu
Preflight { 
    "status": "standby", 
    "reason": "node account has invalid registered IP address", 
    "code": "1" 
}
```

Your node is running but as you can see in the \`Preflight\` section, your node is not yet ready to be churned in and currently is in standby status, since your node has no IP address setup yet.

But to be able to set up the node IP address, you first need to get it registered in the chain by sending your BOND.

### 2 - Send BOND

From the previous status command, you got the current Vault BNB address from the chain running:

```text
Vault BNB Bond Address tbnb14z64mhgsnlayj28llzyehq7uh9e9u2ec2jzysu
```

You need to send your BOND on the BNB chain to this address using the memo `BOND:<node-address>`, which gives with the example before: 

`BOND:thor1ryr5eancepklax5am8mdpkx6mr0rg4xjnjx6zz`

Notes For Mainnet

1. You should practice this first on Testnet
2. You should send a test transaction of 10 RUNE and wait for the system to pick it up and refund you, since it is less than the minimum of 1m RUNE. \(Anything less than 1 RUNE won't result in a refund, since it will consume it all in fees\). 
3. You can then send in at least 1,000,000 RUNE

Give the network a minute or two to pick up your bond.

### 3 - Setup Node IP Address

To set up your node IP address automatically using the load balancer that was created just run the command:

```text
make set-ip-address
```

If you run the status command again, you should now see a different message for the Preflight section saying you need to set your node keys.

### 4 - Setup Node keys

To set up your node keys automatically, just run the command:

```text
make set-node-keys
```

If you run the status command again, you should now see that your node is in status “ready” and is now ready to be churned in the next rotation. Although your node is ready to be churned in, it doesn’t mean it will be the next one to be selected.
