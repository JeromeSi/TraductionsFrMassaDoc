# List of bootstrap errors
## What is bootstrap ?
Bootstrap is a procedure. When your node make a bootstrap, your node synchronize informations of Massa blockchain.

## Where can I bootstrap ?
At the begining, you can bootstrap from a list of official nodes.

You can find the officials node in the file **massa/massa-node/base_config/config.toml** in section **[bootstrap]**. 

## A brief list of bootstrap errors (and what you can do)
1. **Error while connecting to bootstrap server: io error: No route to host (os error 113)** : the target node is down or is unreachable. Wait, your node try another in your bootstrap list.
2. **Error received from bootstrap server: Your last bootstrap on this server was 11h 59m 43s 895ms 540us 156ns ago and you have to wait before retrying.** : in offcial nodes you've got one try per 12h. In this samplen I have to wait 11h 59m.
