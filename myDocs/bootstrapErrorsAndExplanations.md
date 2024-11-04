# List of bootstrap errors
## What is bootstrap ?
Bootstrap is a procedure. When your node make a bootstrap, your node synchronize informations of Massa blockchain.

## Where can I bootstrap ?
At the begining, you can bootstrap from a list of official nodes.

You can find the officials node in the file **massa/massa-node/base_config/config.toml** in section **[bootstrap]**.

## A brief list of bootstrap some errors (and what you can do)
1. **Bootstrap from server IPtargetNode:31245 failed. Your node will try to bootstrap from another server in 60s.** : your node wait **60s** before a new try of bootstrap on other target node.
2. **Error while connecting to bootstrap server: io error: No route to host (os error 113)** : the target node is down or is unreachable. Wait, your node try another in your bootstrap list.
3. **Error received from bootstrap server: Your last bootstrap on this server was 11h 59m 43s 895ms 540us 156ns ago and you have to wait before retrying.** : in official nodes you've got one try per 12h. In this sample, I have to wait 11h 59m. Wait your node make another try in 60s on an another target node.
4. **Error received from bootstrap server: Bootstrap failed because the bootstrap server currently has no slots available.** : you're not lucky, the target node can't accept more bootstrap at the same time, be patient and wait, your node try another in your bootstrap list in 60s.
5. **Error while bootstrapping: io error: early eof** : 2 possibilities
	1. the IP of your node isn't allow to bootstrap with the target node. The target must had your IP in the file **massa/massa-node/base_config/bootstrap_whitelist.json** and restart the target node. It is planned to add dynamically IP in the target node in version 18.
	2. the port 31244 and/or 31245 are not open. Open it in your node and in your box or in the VPS management interface
6. **Error while bootstrapping: `massa_signature` error Signature error : Signature verification failed: signature error: Verification equation was not satisfied** : Verify the signature of the target node in the file where is the list of the bootstrap node. If you add nodes for bootstrap, there is an error when you copy address and/or node ID. You can delete the node or comment the line with **#** at the beginning.
7. **Error while connecting to bootstrap server: io error: Connection refused (os error 111)** : we have this error when you run many **massa-node**. We close all massa-node processus with `pkill -f massa-node` and you run massa-node as usual.
8. **WARN massa_bootstrap::client: Error while connecting to bootstrap server: io error: Network is unreachable (os error 101)** : you have an IPv4 internet address and your node try to bootstrap to an official node with an IPv6 internet address. An IPv4 can contact only IPv4 (See **Notes**). We can add in **massa/massa-node/config/config.toml** :

 ```toml
[bootstrap]
    # force the bootstrap protocol to use: "IPv4", "IPv6", or "Both". Defaults to using both protocols.
    bootstrap_protocol = "IPv4"
 ```

9. **WARN massa_bootstrap::client: Error while bootstrapping: general bootstrap error: Parsing Error: Failed BootstrapServerMessage deserialization / Failed MIP store deserialization / Failed MipStoreRaw der / Failed mip store stats der / Failed MipStoreStats network version counters der / Failed counters len der / Fail / Input: [15, 0, 15]** : At the moment, I don't know why you have this problem but it will be solve with testnet version 22.

10. **Error received from bootstrap server: IP ....... is not in the whitelist** : the IP of your node is not allowed to make a bootstrap on this node. You must add your IP in the target node with `massa-client`:
```sh
node_bootstrap_whitelist add NodeIP
```

11. **Error while connecting to bootstrap server: all io errors except for Timedout, and would-block (unix error when timed out)** : the error is in the file `massa/massa-node/config/config.toml`. You must check it. You have to change `[network]` to `[protocol]` since testnet version 22. You must add a blank line at the end of the file.

12. **Error while connecting to bootstrap server: Bootstrap IO error: Address family not supported by protocol (os error 97)** : you disable IPv6 in your OS and your node can't be connected with a node with an IPv6.

13. **Error while connecting to bootstrap server: io error: Cannot assign requested address (os error 99)** : at the moment, I don't know how to solve the problem.

## Notes
1. If you have an IPv4, you can't bootstrap on target node with IPv6, only IPv4
2. If you have an IPv6, you can bootstrap on target node with IPv6 or IPv4
