# Spark Janelia

Shell scripts and utilities for launching Spark and running applications (e.g. Thunder) on the Janelia Research Center cluster. We maintain a chatroom on gitter if you have questions!

[![Join the chat at https://gitter.im/freeman-lab/spark-janelia](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/freeman-lab/spark-janelia?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

---

### Running Spark
These scripts automate many of the tasks required to start and manage a Spark job.

#### Initial setup
SSH in to one of Janelia's cluster login nodes (see [Scientific Computing](http://wiki.int.janelia.org/wiki/display/ScientificComputing/Janelia+Compute+Cluster) for more information).
```
ssh login2.int.janelia.org
```
Navigate to a location in your home directory where you will store these scripts, then call
```
git clone https://github.com/freeman-lab/spark-janelia
```
Add a line with this location to your bash profile (usually located in `~/.bash_profile`)
```
export PATH=/path/to/spark-janelia:$PATH
```
While doing this, also add this line to point your PATH to a recent version of Python:
```
export PATH=/usr/local/python-2.7.6/bin/:$PATH
```
If you want to follow the next steps in the same session, source your profile by typing `source ~/.bash_profile`.

#### Starting a Spark cluster
While logged in to one of Janelia cluster's login nodes, submit a request to run a group of nodes as a Spark cluster using:
```
spark-janelia -n <number_of_nodes>
```
Each node has 16 cores and 100GB of RAM for Spark to use. You should target the number of nodes you request based on the size of the data you were working or the amount of neccessary computation (or both). A good rule of thumb is to use enough nodes so the total amount of RAM is about twice the total size of your data set.

Check the status of your request using the `qstat` command. When the status is listed as `r` (for "ready"), proceed.

Every Spark cluster has a unique node designated as the "driver" from which all computations are initiated. To log in to the driver for your Spark cluster, use:
```
spark-janelia login
```
You can now run Spark applications as described below.

#### Running basic Spark jobs
To start the Spark interactive shell in Python call
```
spark-janelia start
```
This will start a shell with Spark running and its modules available.

To start the Spark interactive shell in Scala call
```
spark-janelia start-scala
```
And to submit a Spark application to run call
```
spark-janelia submit -s <submit_arguments>
```
Where `submit_arguments` is a string of arguments you would normally pass to `spark-submit`, as described [here](https://spark.apache.org/docs/1.2.0/submitting-applications.html).

---
### Running Thunder
Many of us at Janelia are using the `thunder` Python library for working with neural data in Spark. The scripts in this repo also make installing and using Thunder easy. 
#### Install and run
First, to install, from your home directory type
```
thunder-janlelia install
```
This will clone a copy of Thunder into your home directory. If you want to install to a different location, use
```
thunder-janelia install -p <path_to_thunder>
```
To start Spark with Thunder, type
```
thunder-janelia start
```
This will open an interactive shell with [Thunder](http://thunder-project.com/thunder/) functionality preloaded. See its project page for how to use this library in your data workflows and analyses.

If you want to grab the latest version, use
```
thunder-janelia update
```
If you have your own version of Thunder (e.g. because you have cloned its repo and are modifying its source code), you can specify a version to run by specifying the location directly
```
thunder-janelia start -p <path_to_thunder>
```
---
### Using the IPython notebook
When running Spark in Python, the IPython notebook is a fantastic way to run analyses and inspect and visualize results. These scripts make it easy to set up the notebook for use on Janelia's cluster.

First, as a one time operation, call this script
```
notebook-janelia
```
This will configure your IPython Notebook settings to be compatible with Janelia's cluster. The only thing you need to do is provide a password when prompted. This password will allow you to make a secure connection to the IPython Notebook server.

Now, when starting either Spark or Thunder, add the `-i` flag, as in:
```
spark-janelia start -i
thunder-janlelia start -i
```
Instead of opening a shell, this will launch an IPython Notebook server on the driver. You will see a website addressed printed next to `View your notebooks at...`. Go to this address in a web browser. Your browser will complain that the connection is not trusted; just ignore and proceed.

When prompted, enter the password you chose above. You should now have a graphical interface to the directory from which you launched the IPython Notebook server. From here you can create a new notebook or edit a previously-existing one.

To shut down the IPython Notebook server, return to the terminal where the server is running and type `Ctrl+C+C`.

NOTE: You can't run Spark jobs in multiple notebooks at once. If you start one notebook, then start another, nothing will execute in the second one until you shutdown the first one (by clicking the shutdown button in the notebook browser).

---
### Shutting down
When you are finished with all of your jobs, log out of the driver with the simple command `exit` in the command line. Finally, release your nodes with
```
spark-janelia destroy
```
You can check that these nodes have successfully been released with the `qstat` command.

---
## Questions and comments
Many Spark users at Janelia can be found in the [Gitter chat rooom](https://gitter.im/freeman-lab/spark-janelia?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) for this repository. If you have questions or comments, or just want to join the conversation, please drop in!
