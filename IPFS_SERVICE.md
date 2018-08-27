# Set up IPFS daemon as a system service.  

In order to have the IPFS daemon run on boot you will need to create a ```systemd``` service.  

This is quite simple but fairly picky. First you will create a new file called ```ipfs.service```.  

----
This file should contain:  

    [Unit]  
    Description=IPFS daemon  
    After=network.target  
    
    [Service]  
    ExecStart=/usr/local/bin/ipfs daemon  
    Restart=on-failure  
    User=<user_to_run_as>  
    
    [Install]  
    WantedBy=default.target  
    
Now save this file as:  

   ```/etc/systemd/system/ipfs.service```

----
Now ```systemctl``` will be able to find your IPFS service.  

Make sure the daemon is stopped, the path in ```ExecStart``` points to your IPFS executable, and ```User``` is set.  

Test your new system service is properly configured with:  

```sudo systemctl start ipfs.service```  
```ipfs id```  

If the second command returns your peer info, then the service is properly configured. Enable it on boot with:  

```sudo systemctl enable ipfs.service```  

> Created symlink from /etc/systemd/system/default.target.wants/ipfs.service to /etc/systemd/system/ipfs.service.

This will notify you that a symlink has been created, which means success!  

This document assumes you are running a Debian system. Check your operating system's ```systemd``` documentation if 
you run into trouble. 

----
## When Running into Same-Origin Policy Issues

When using the IPFS API, you will almost certainly run into "Cross-Origin Request Rejected" and XHR errors.  
To mitigate this, run the following commands on the machine running the IPFS daemon:  

```ipfs config --json API.HTTPHeaders.Access-Control-Allow-Origin '["example.com"]'```  
```ipfs config --json API.HTTPHeaders.Access-Control-Allow-Methods '["GET", "POST"]'```  
```ipfs config --json API.HTTPHeaders.Access-Control-Allow-Credentials '["true"]'```  

And if you want to use the API from somewhere other than the machine running the IPFS daemon:  

```ipfs config Addresses.API "/ip4/0.0.0.0/tcp/5001"```

Ensure your firewall rules are acceptable to you for this one.




