# Creating an EMR instance with JupyterHub

[JupyterHub](https://jupyterhub.readthedocs.io/en/stable/) is an officially supported application on Amazon’s EMR
(version 5.14.0 and above). A user can create a EMR cluster with
JupyterHub installed to access JupyterHub on his/her web browser. The
JupyterHub server enables user(s) to create, view, and edit Jupyter
notebooks. 

> Full guide here:
> <https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-jupyterhub.html>

* Login to the EMR console: <https://console.aws.amazon.com/emr/> or click below:

<img src="media/image29.png" style="width:4.72222in;height:2.68056in" alt="Graphical user interface, text, application Description automatically generated" />

* Choose **Create cluster**

<img src="media/image30.png" style="width:6.5in;height:2.77778in" alt="Graphical user interface Description automatically generated" />

* Select **Go to** **advanced options**.

<img src="media/image31.png" style="width:6.5in;height:2.625in" alt="Graphical user interface, text, application Description automatically generated" />

* Under Software Configuration, for Release, select any of the latest
    versions (emr-5.14.0 and above), and choose **JupyterHub**, along
    with other software that you need:

<img src="media/image32.png" style="width:6.5in;height:4.43056in" alt="Graphical user interface, text, application, email Description automatically generated" />

* \[Optional\]: If you use Apache's [Spark](https://spark.apache.org) or [Hive](https://hive.apache.org), you can use the **AWS Glue Data
    Catalog** as the metastore. Select Use for Spark/Hive
    table metadata. For more information, visit [here](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-spark-glue.html) and 
    [here](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-hive-metastore-glue.html)

* You can customize the configuration of JupyterHub on Amazon EMR and
    individual user notebooks by editing the configuration or loading a
    JSON file from S3 under **Edit software settings**.

> **Important example**: you can configure a JupyterHub cluster in Amazon
> EMR so that notebooks saved by a user persist in Amazon S3, outside of
> ephemeral storage on cluster EC2 instances:
> <https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-jupyterhub-s3.html>
> Information on how to create an Amazon S3 bucket is in [createbucket.MD](https://github.com/shenjean/cloud-classroom/blob/main/createbucket.MD)

```
\[
{
"Classification": "jupyter-s3-conf",
"Properties": {
"s3.persistence.enabled": "true",
"s3.persistence.bucket": "testbucketjean030521"
}
}
\]
```

>Notebooks for each user are saved to a **jupyter/jupyterhub-user-name**
>folder in the specified bucket. When you launch a new cluster using the
>same configuration classification properties, users can open notebooks
>with the content from the saved location.

* Under Steps (optional), configure steps to run when the cluster is
    created. Make sure **Cluster enters waiting state** is selected
    instead of Cluster auto-terminates. Then, choose Next.

* Choose **Hardware Configuration** options. For more information, see
    <https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-plan-instances.html>

*  Choose options for **General Cluster Settings**. Here you can change
    the cluster name, and specify the S3 bucket and folder where log
    files should be stored.

<img src="media/image34.png" style="width:4.94444in;height:2.25in" alt="Graphical user interface, application Description automatically generated" />

* Under **Security Options**, specify your key pair, and choose Create Cluster.

<img src="media/image35.png" style="width:3.81944in;height:2.38889in" alt="Graphical user interface, text, application, email Description automatically generated" />

* Under **Security Options**, specify your key pair, and choose Create Cluster.

* Your cluster will start and will be ready once the status changes to
    “Waiting” (\~15 mins):

##  Set up a SSH connection to the master node

You can connect to the Amazon EMR master node using SSH to run
interactive queries, examine log files, submit Linux commands, and so
on.

*  Inbound SSH traffic from port 22 must be authorized before any
    connection to an Amazon EMR cluster. You can check whether inbound
    SSH traffic is authorized by going to the “**Block public access**”
    tab and checking whether **port 22** is in the Exceptions list.

<img src="media/image36.png" style="width:6.5in;height:1.98611in" alt="Graphical user interface, text, application, email Description automatically generated" />

> If inbound SSH traffic is not authorized, you can edit the
> authorizations if you are a root user by following the instructions [here](<https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-connect-ssh-prereqs.html>)
> below. Else, contact an administrator.

* Click on **Connect to the Master Node using SSH**. This will bring up a popup window with specific instructions:

<img src="media/image37.png" style="width:3.63889in;height:1.93056in" alt="Graphical user interface, text, application, email Description automatically generated" />

<img src="media/image38.png" style="width:6.5in;height:2.68056in" alt="Graphical user interface, text, application, email Description automatically generated" />

* A typical command looks like this (note that the IP address changes with each loaded instance):

`ssh -i keypairname.pem hadoop@ec2-3-142-185-15.us-east-2.compute.amazonaws.com`

* Run the command and type **yes** to respond to the prompt on the
    ECDSA key fingerprint:

<img src="media/image39.png" style="width:6.5in;height:2.80556in" alt="Text Description automatically generated" />

##  Access JupyterHub (and other user interfaces) from your web browser

> Full instructions [here](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-ssh-tunnel-local.html)

* On your local computer, set up a SSH tunnel to the master node using dynamic port forwarding:

`ssh -i Jean.pem -N -D 8157 hadoop@ec2-3-142-185-15.us-east-2.compute.amazonaws.com`

This command accesses the ResourceManager web interface by
forwarding traffic on local port **8157** (a randomly chosen unused
local port) to the master node's local web server. Replace the DNS
name (ec2-xxxx.compute.amazonaws.com) with that of your cluster
accordingly. **You will not get any response after executing this
command, but keep the SSH client window open to maintain the tunnel.**


* Use a Firefox or Chrome SOCKS proxy management add-on to control the
    proxy settings in your browser. Full instructions [here](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-connect-master-node-proxy.html)

>**Note**: The Mozilla Firefox instructions worked for me but not Chrome.
>Follow the steps above and change `browserVersion` and
>`foxyProxyversion` accordingly in the JSON file.

You will receive a security warning on your web browser when you open up
the interface, click “Advanced..” and “Accept the Risk and Continue”:

<img src="media/image40.png" style="width:3.91667in;height:2.54167in" alt="Graphical user interface, application, Teams Description automatically generated" />

*  Go to the **Application user interfaces** tab. Now, you can access
    any of the user interfaces by copying and pasting the relevant URL
    in your Firefox or Chrome browser:

<img src="media/image41.png" style="width:6.5in;height:2.84722in" alt="Graphical user interface, text, application, email Description automatically generated" />

For JupyterHub, the port is 9443, e.g.`https://ec2-xxx.compute.amazonaws.com:9443/`

*  Once you open up the JupyterHub interface in your browser, you will
    be required to login. The generic credentials for AWS are `jovyan` and `jupyer`.

<img src="media/image42.png" style="width:2.73611in;height:1.94444in" alt="A picture containing graphical user interface Description automatically generated" />

<img src="media/image43.png" style="width:6.5in;height:1.84931in" alt="Graphical user interface, application, Teams Description automatically generated" />

* You can create a new Jupyter user (e.g. diego) with the password
    (e.g. password) on the master node with the following commands:

```
sudo docker exec jupyterhub useradd -m -s /bin/bash -N diego
sudo docker exec jupyterhub bash -c "echo diego:password \| chpasswd"
```

You can also use other methods to create users - visit this [guide](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-jupyterhub-user-access.html)

*  You can upload files from your local computer directly to the
    instance by clicking on the **Upload** button.

*  \[Recommended\]: If you had enabled **notebook persistence on S3
    storage** (see previous section), all uploaded and created files are
    saved in /jupyter/username in your bucket, e.g.
    `s3://testbucketjean030521/jupyter/jovyan`

> Note: If you are opening files within your Jupyter notebook, you will
> have to specify the full path of the input files. Else, you will get a
> file not found error. Example:

`df1 =pd.read\_table('s3://testbucketjean030521/jupyter/jovyan/ERR599057.SSU.tsv',index\_col=1)`

> \[Not recommended\]: If notebook persistence is not enabled, all files
> will be in `/var/lib/jupyter/home/jovyan`. This is ephemeral storage
> that does not persist through cluster termination. When a cluster
> terminates, this data is lost if not backed up. We recommend that you
> schedule regular backups using cron jobs or another means suitable
> (e.g. JupyterHub configuration for notebook persistence on S3 – see
> instructions above or [CLI.MD](https://github.com/shenjean/cloud-classroom/blob/main/CLI.MD) 
> to copy files to/from your S3 bucket. In addition, configuration
> changes made within the container may not persist if the container
> restarts. We recommend that you script or otherwise automate container
> configuration so that you can reproduce customizations more readily.


## Clean up Amazon EMR Cluster Resources

###  Terminate the EMR cluster

Amazon EMR retains metadata about your cluster for two months at no
charge after you terminate the cluster. This makes it easy to **clone**
the cluster for a new job or revisit its configuration for reference
purposes. Metadata does not include data that the cluster might have
written to S3, or that was stored in HDFS on the cluster while it was
running.

*  On the EMR console, select your cluster and click **Terminate**:

<img src="media/image45.png" style="width:4.55556in;height:2.40278in" alt="Graphical user interface, application Description automatically generated" />

* This will bring up the Terminate cluster prompt. Click on “Change”
    to turn off termination protection:

<img src="media/image46.png" style="width:4.73611in;height:1.44444in" alt="Graphical user interface, application, Teams Description automatically generated" />

*  Select Off, then click on the green checkmark and click Terminate:

<img src="media/image47.png" style="width:5.05556in;height:1.51389in" alt="Graphical user interface, text, application Description automatically generated" />

> **Note:** The Amazon EMR console does not let you delete a cluster
> from the list view after you shut down the cluster. A terminated
> cluster disappears from the console when Amazon EMR clears its
> metadata.

* Empty/delete your S3 bucket - instructions at [emptybucket.MD](https://github.com/shenjean/cloud-classroom/blob/main/emptybucket.MD) 
