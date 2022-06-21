# IPFS Private Swarm

----
## 0. Requirements

* [golang-go](https://golang.org/doc/)  
* [go-ipfs](https://github.com/ipfs/go-ipfs)    
* [go-ipfs-swarm-key-gen](https://github.com/Kubuxu/go-ipfs-swarm-key-gen)  
 

*Docker is useful for deploying test networks.*

----
## 1. Setup

1. Download latest release [go-ipfs](https://dist.ipfs.io/#go-ipfs) binary and untar it.  

2. Now copy ```go-ipfs/ipfs``` to ```/usr/local/bin/ipfs``` or somewhere $PATH points to.  

3. Test ipfs is working with ```ipfs help```  

4. Run ```ipfs init```, this will set up a node with the default ```IPFS_PATH=/home/$USER/.ipfs``` and create a new identity. 

> initializing IPFS node at /home/$USER/.ipfs  
generating 2048-bit RSA keypair...done  
peer identity: QmS4ustL54uo8FzR9455qaxZwuMiUhyvMcX9Ba8nUH4uVv  


5. Since this is a private swarm, delete the default bootstrap nodes with ```ipfs bootstrap rm --all```

 

----
## 2. Generate swarm.key

Skip these steps if you have a pre-shared ```swarm.key```.  

1. ```git clone https://github.com/Kubuxu/go-ipfs-swarm-key-gen```  
2. ```cd go-ipfs-swarm-key-gen/```  
3. :warning: **Next command will Overwrite existing swarm.key**
4. ```go run ipfs-swarm-key-gen/main.go > $IPFS_PATH/swarm.key```  

## 2.1 Generate swarm.key without Above Repository

As mentioned in https://github.com/ahester57/ipfs-private-swarm/issues/1, you can also generate a `swarm.key` using the following command:

> :warning: **Next command will Overwrite existing swarm.key**

```
(echo -e '/key/swarm/psk/1.0.0/\n/base16/' ; head -c 32 /dev/urandom | od -t x1 -A none - | tr -d '\n '; echo '') > $IPFS_PATH/swarm.key
```

##### I have a pre-shared swarm.key

Ok. Put that file in the default IPFS directory. On Linux that is ```/home/$USER/.ipfs/```.

This directory should also contain configuration and keystores. If it does not, go back and check where ```ipfs init``` put these.

Great, now your node won't connect to anyone without the same swarm.key. Be sure to remove the default ipfs bootstrap addresses before continuing. 


----
## 3. Connect to your swarm

1. Start your ipfs node with ```ipfs daemon```. You should see a message near the top. If you don't see the following message ensure the ```swarm.key``` file is in the correct location.

> Swarm is limited to private network of peers with the swarm key  
Swarm key fingerprint: xxxxxxxxxxxxxxxx

2. In a new terminal, run ```ipfs id```. You will see the peer id, public key, addresses, and versions. We are interested in the ```Addresses``` list.

3. There will be an address for each active network interface. Copy the address which other nodes will be able to reach you with.  
It looks like ```/ip4/192.168.1.11/tcp/4001/ipfs/<peer_id>```.

4. Hopefully you will have other nodes you have set up with the same configuration as above. Add the above address to the bootstrap list of those nodes with ```ipfs bootstrap add /ip4/192.168.1.11/tcp/4001/ipfs/<peer_id>```.

5. You should have multiple nodes set as bootstrap nodes to ensure network connectivity.

Congrats, you now have a private ipfs swarm up and running. Now to learn ipfs!

----
## Usage

Usage is congruent to the default usage of ipfs. Check out the documentation [here](https://ipfs.io/docs/). 

* Set up boot node to run IPFS deamon on boot: [IPFS_SERVICE](https://github.com/ahester57/ipfs-private-swarm/blob/master/IPFS_SERVICE.md)  
* Configure your local IPFS node for the private swarm: [CONNECT](https://github.com/ahester57/ipfs-private-swarm/blob/master/CONNECT.md)  
* Enable the WebUI at localhost:5001/webui for your private swarm: [WEBUI](https://github.com/ahester57/ipfs-private-swarm/blob/master/WEBUI.md)  



