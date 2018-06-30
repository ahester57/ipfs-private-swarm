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

----

This document assumes you are running a Debian system. Check your operating system's ```systemd``` documentation if 
you run into trouble.  

