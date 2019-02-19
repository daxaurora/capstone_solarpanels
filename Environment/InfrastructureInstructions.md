# Infrastructure and Environment
### Berkeley MIDS Capstone Spring 2019 | Predicting Solar Panel Adoption
__`Team: Noah Levy, Gabriel Hudson, Laura Williams`__  

 ---
## QuickStart

Team members can log onto the gateway server like this:  

`` ssh root@169.63.216.242
``  

Log onto the Spark cluster from the gateway server like this:  

``ssh root@spark1
``

Get the most recent version of the repo before starting Jupyter notebook:  
``cd capstone_solarpanels/``  
``git pull``  
``cd ..``  

**Optional**: copy notebooks into the notebook_testing directory unless you plan to push notebook changes directly from the repo. Copying can be done before opening Jupyter or inside Jupyter (Duplicate and Move).

Start Jupyter Notebook on the Spark cluster from the root directory:

``jupyter notebook``

Copy and paste the https link (including the token) into your browser.

You'll see the project repo and a notebook testing folder on the root directory of the spark1 server.

See more details in the sections below.

## Gateway Server
There is a small virtual server (in IBM's Softlayer Cloud Service) set up for creating and accessing other virtual servers with computing environments (i.e., Spark) that we'll set up and design ourselves. Team members can log into the gateway server using ssh at command prompt from their personal computer:  
`` ssh root@169.63.216.242
``  

For security, this gateway server will only be accessible using ssh key pairs.  Password access will be turned off.

The gateway server has only a very basic setup:  Centos 7 OS, Python 2.7, and the slcli client for provisioning servers on the IBM Softlayer Cloud.

If needed, here are some basic slcli commands for creating additional virtual servers.  These commands can be run on the gateway server as the root user from this prompt: ``[root@gateway]#``  

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

## Spark Cluster and Jupyter Notebook
Three virtual servers are set up as a Spark cluster.

To connect to the cluster directly from the gateway server, at the ```[root@gateway]#``` prompt, type:  
``ssh root@spark1``

To open up Jupyter Notebook, at the `root@spark1` prompt, type:  
``jupyter notebook``

You'll see onscreen instructions with an https link, including a token at the end.  Copy that link into your browser.  You may get a warning that the connection is not secure because the certificate is not trusted.  Choose to trust the certificate (you may need to click on "Advanced" or otherwise figure out how to do this).  You may need to enter that token separately after choosing to trust the certificate but after you choose to trust it, including the token as part of the link should automatically open up Jupyter.  

You can shut down Jupyter as you normally would with Control-C.

It's also possible to open a pyspark shell on the spark1 server independently from Jupyter Notebook:  
``$SPARK_HOME/bin/pyspark``

Software versions:  
Spark 2.4.0
Java 1.8.0_191
Python 3.7.1  
Anaconda 4.6.4  

NOTE:  I haven't tested running any of the notebooks in Spark yet, I've only confirmed that Jupyter works and that Spark is running.  I may need to set up some other environment variables for Jupyter to connect with pyspark. --Laura (Sunday night 2/17)

Spark will be kept running on the cluster so there should be no need to stop or start it.  However, if needed, here are some commands for stopping and starting Spark on the cluster. Run all commands from the `spark1` server, there is no need to connect to any of the slave servers in the cluster.  

`$SPARK_HOME/sbin/start-master.sh` - Starts a master instance on the machine the script is executed on  (i.e., on spark1)  
`$SPARK_HOME/sbin/start-slaves.sh` - Starts slaves instances  
`$SPARK_HOME/sbin/start-all.sh` - Starts both a master and all slaves  
`$SPARK_HOME/sbin/stop-master.sh` - Stops the master that was started via the sbin/start-master.sh script  
`$SPARK_HOME/sbin/stop-slaves.sh` - Stops all slave instances   
`$SPARK_HOME/sbin/stop-all.sh` - Stops both the master and the slaves

When the cluster is active, you should be able to see its activity from any browser by navigating here:    
``http://169.63.216.247:8080``

NOTE: Because the spark cluster slaves are communicating via private IPs, it's not possible to navigate to them  separately right now, only to the master node at the above link. I'll do what I can to fix that if we want to be able to use the Spark UI.  --Laura

## Project Repo

The project repo is on there on the /root directory.  I set up the network ssh key so it's possible to push and pull to/from the GitHub repo.

There is a basic .gitignore file to prevent ipynb checkpoint files from being pushed to the Github repo.  This .gitignore file is NOT intended to be pushed to the GitHub repo, only for working with the cluster repo.

Given that this copy of the repo is essentially shared, aim to follow these instructions to keep our project files organized:

* The version of the repo on GitHub (not the version on the cluster) will be the ground truth for current versions of files.
* Always `git pull` the most recent version of the Github repo before working on files in the cluster.
* **Optional:**  Before opening any notebooks, copy them to the notebook_testing folder. A copy of the original dataset is kept in that folder.  
* Continue to only make changes to our own working notebooks, not anyone else's notebooks.
* To push any changes to notebooks back to the GitHub repo, copy the notebook back to the project repo on the cluster (if copied first to the notebook_testing folder), and push it up from there.
* Always delete notebooks from the notebook_testing folder when done. (But not the dataset)
* There is a backup of the original dataset on the /root directory of both the gateway server and the spark1 server.

If anyone has a better idea about how to manage this, please suggest it!


## General Information
To terminate server connections, type `exit` at the command prompt for each connection.

There is a separate ssh keypair for connecting between the gateway machine and the Spark cluster, and also between the spark nodes.

The ssh connections between all servers in this private network are set to disconnect after 12 hours of inactivity.

If needed, the IP addresses and names of all servers can be found using slcli commands from the gateway server (see the Gateway Server section above).

Because this project does not have large data, HDFS is NOT set up as part of this cluster.  Spark can simply access data files stored on the master node.  But if the data gets bigger, an HDFS cluster could be set up for Spark to get data from there instead.
