# Infrastructure and Environment
### Berkeley MIDS Capstone Spring 2019 | Predicting Solar Panel Adoption
__`Team: Noah Levy, Gabriel Hudson, Laura Williams`__  

 ---

## Gateway Server
There is a small virtual server (in IBM's Softlayer Cloud Service) set up for creating and accessing other virtual servers with computing environments (i.e., Spark) that we'll set up and design ourselves. Team members can log into the gateway server using ssh at a command prompt:  
`` ssh root@169.63.216.242
``  

To connect to the Spark environment, see the section below titled __Spark Cluster__.

For security, this gateway server will only be accessible using ssh key pairs.  Password access will be turned off.

The gateway server has only a very basic setup:  Centos 7 OS, Python 2.7, and the slcli client for provisioning servers on the IBM Softlayer Cloud.


Here are some basic slcli commands for creating additional virtual servers.  These commands can be run on the gateway server as the root user from this prompt: ``[root@gateway]#``  

To see the current list of servers provisioned with this account:    
``slcli vs list``  

To set up a new server in this network (replace items inside <>):   
``slcli vs create --datacenter=dal10 --hostname=<name> --domain=solarcapstone.com --billing=hourly --key=capstone --cpu=<integer number> --memory=<byte number, i.e., 2048> --os=CENTOS_LATEST_64``  
IMPORTANT NOTE: Set up all server on the same datacenter, currently `dal10`.

To see tags for creating virtual servers:   
``slcli vs create -h``

To see options for some of the tags (sometimes this list takes a minute or two to load):  
``slcli vs create-options``  
Unfortunately the names in this list of options do not directly correspond to the tags in the `vs create` command, but it's possible to figure out which options go with which tags.

## Spark Cluster
Three virtual servers are set up as a Spark cluster.

To connect to the cluster directly from the gateway server, at the ```[root@gateway]#``` prompt, type:  
``ssh root@spark1``

The project repo is on there at the /root directory.  I added the ssh key for this network to my Github account, and tested that we can push and pull from this version of the repo.

NOTE:  I did NOT yet make a .gitignore file yet for this repo, so please pay careful attention to changes you make to the repo on the cluster.

To open up Jupyter Notebook, at the `root@spark1` prompt, type:  
``jupyter notebook``

You'll see onscreen instructions with a link, including a token at the end.  Type that link into your browser.  You may get a warning that the connection is not secure because the certificate is not trusted.  Choose to trust the certificate (you may need to click on "Advanced" or otherwise figure out how to do this).  You may need to enter that token separately after choosing to trust the certificate but after you choose to trust it, including the token as part of the link should automatically open up Jupyter.  

It's also possible to open a pyspark shell independently from Jupyter Notebook:  
``$SPARK_HOME/bin/pyspark``

NOTE:  I haven't tested running any of the notebooks in Spark yet, I've only confirmed that Jupyter works and that Spark is running.  I may need to set up some other environment variables for Jupyter to connect with pyspark. --Laura (Sunday night 2/17)

Spark will be kept running so there should be no need to stop or start it.  If needed, here are some commands for stopping and starting Spark on the cluster. Run all commands from the `spark1` server, there is no need to connect to any of the slave servers in the cluster.  

`$SPARK_HOME/sbin/start-master.sh` - Starts a master instance on the machine the script is executed on  (i.e., on spark1)  
`$SPARK_HOME/sbin/start-slaves.sh` - Starts slaves instances  
`$SPARK_HOME/sbin/start-all.sh` - Starts both a master and all slaves  
`$SPARK_HOME/sbin/stop-master.sh` - Stops the master that was started via the sbin/start-master.sh script  
`$SPARK_HOME/sbin/stop-slaves.sh` - Stops all slave instances   
`$SPARK_HOME/sbin/stop-all.sh` - Stops both the master and the slaves

When the cluster is active, you should be able to see its activity from any browser by navigating here:    
``http://169.63.216.247:8080``

NOTE: Because the spark cluster slaves are communicating via private IPs, it's not possible to navigate to them  separately right now, only to the master node at the above link.  --Laura



## Misc Useful information
To terminate server connections, type `exit` at the command prompt for each connection.

The ssh connections between all servers in this private network are set to disconnect after 12 hours of inactivity.

If needed, the IP addresses and names of all servers can be found using slcli commands from the gateway server (see the Gateway Server section above).

Because this project does not have large data, HDFS is NOT set up as part of this cluster.  Spark can simply access files on the master node.  But if the data gets bigger, an HDFS cluster could be set up for Spark to get data from there instead.

There is a separate ssh keypair for connecting between the gateway machine and the Spark cluster, and also between the sparm nodes.
