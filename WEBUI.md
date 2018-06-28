# Get your node up and running with the WebUI

In a private swarm, the default bootstrap nodes are not available to serve the WebUI
at http://localhost:5001/webui.  

To make available the WebUI without running the dev build every time, you will need a go-ipfs client which 
points to the correct location. The WebUI is served by the IPFS network, and the client hard-codes the hash 
of this location. Therefore, a custom client is needed in order to point to the correct hash.

Note: This only needs to be done once per private network. Once the boot node serves the WebUI, all other nodes 
only require the custom client which points to the correct hash.

----
## 0. Requirements

* golang-1.10+
* custom IPFS client
* [ipfs-webui](https://github.com/ipfs-shipyard/ipfs-webui)

----
## 1. Install golang-1.10+

Building go-ipfs from source requires golang-1.10+. 

```add-apt-repository ppa:gophers/archive```  
```apt update```  
```apt install golang-1.10-go```  
```echo "export PATH=/usr/lib/go-1.10/bin:$PATH" >> ~/.bashrc```  
```source ~/.bashrc``` || ```bash```  

Confirm the install with ```go version```.  
If the version is still old, then you probably still have an old version of golang installed. Remove it.  

----
## 2. Install the WebUI on the boot node

Why did we just install golang-1.10? We will need it when building the client from source.  
But first we need the hash our client will point to.  

1. Clone the repository https://github.com/ipfs-shipyard/ipfs-webui.  
2. ```cd ipfs-webui && npm install```  
3. Build to ```./dist``` with ```npm run build```  
4. Upload the directory ```./dist``` to IPFS (should be running).  

    ```ipfs add -r ./dist``` will give you a list of hashes for each file in the directory. We need the last one.  
    
    Let's assume the root hash is ```QmbhqZ17mpsz9FrHyUhh3SmLURRLxVBpft94ya8njLgvBM```.  

5. On the boot node, pin the last hash from the output above.  

    ```ipfs pin add QmbhqZ17mpsz9FrHyUhh3SmLURRLxVBpft94ya8njLgvBM```
  
----
## 3. Install go-ipfs from modified source.  

The file we want to change is ```go-ipfs/core/corehttp/webui.go```.  

Clone the [go-ipfs](https://github.com/ipfs/go-ipfs) repository.  
    
```git clone https://github.com/ipfs/go-ipfs```  
    
Next, 



