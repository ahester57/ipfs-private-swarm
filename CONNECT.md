
# Connecting to your IPFS boot node 

The following steps will allow you to connect to an IPFS bootstrap node.  
This document assumes you have access to an IPFS boot node setup on AWS or on a local network.  

Before you connect, you will need a few things:  

1. IP address of the boot node, ```$BOOTNODE_IP```.  
2. IPFS peer id of boot node, ```$BOOTNODE_ID```.
3. The swarm key stored at ```$IPFS_PATH/swarm.key```.

Ask your SysAdmin for the above information. They will be able to supply the boot node's peer address, 
which includes both ```$BOOTNODE_IP``` and ```$BOOTNODE_ID```.  

A peer address looks like:  

>  /ip4/172.33.22.55/tcp/4001/ipfs/QmS4ustL54uo8FzR9455qaxZwuMiUhyvMcX9Ba8nUH4uVv

For the following steps, we will assume the boot node is located at the above peer address.

----
## 1. Setup IPFS

Follow the setup instructions found in this repository at ```README.md```.  

Once IPFS is installed, run:

```ipfs init```  
```ipfs bootstrap rm --all```  

The above commands will initialize your IPFS directory found at ```$IPFS_PATH```.  
The default ```$IPFS_PATH``` on Mac/Linux is ```~/.ipfs```.  

Ensure you have removed the default bootstrap nodes with:  

```ipfs bootstrap```

Nothing should appear.

----
## 2. Supply the swarm key

This step requires you have been given the secret file, ```swarm.key```.

The contents of ```~/.ipfs``` should look like: 

    api     config     datastore_spec     version
    blocks  datastore  keystore
    
If not, then IPFS is not initialized or has been initialized somewhere else. Check where ```ipfs init``` put these files.  

Find the file, ```swarm.key```.  Now, 

```mv ./swarm.key ~/.ipfs/swarm.key```  
```chmod 400 ~/.ipfs/swarm.key```  

Now the contents of ```~/.ipfs``` should look like: 

    api     config     datastore_spec     version
    blocks  datastore  keystore           swarm.key
    
Ensure the swarm key is properly installed.  
Run ```ipfs daemon```. You should see this message near the top:  

> Swarm is limited to private network of peers with the swarm key  
Swarm key fingerprint: xxxxxxxxxxxxxxxx

----
## 3. Add the boot node address to your bootstrap list  

This is the final step. First, check the peers you are connected to with ```ipfs swarm peers```.  
You should have no peers at this point since you have an empty bootstrap list.  
View your bootstrap list with ```ipfs bootstrap```.  

It should be empty. To add the boot node to your bootstrap list, run the following command with the peer address 
provided to you.  

```ipfs bootstrap add /ip4/172.33.22.55/tcp/4001/ipfs/QmS4ustL54uo8FzR9455qaxZwuMiUhyvMcX9Ba8nUH4uVv```  

Check your bootstrap list again. The given peer address should appear.  
Now check your connected peers again with ```ipfs swarm peers```. You should see the bootstrap node listed as a peer.

If so then everything went right. Otherwise there is a problem. Potential problems include but are not limited to:  

1. ```swarm.key``` mis-located or mismatched.  
2. Peer IP address or peer ID wrong.
3. Port 4001 not open. 


----
## Usage

Usage is congruent to the default usage of ipfs. Check out the documentation [here](https://ipfs.io/docs/). 


