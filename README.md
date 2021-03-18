# NGINX Inc. and Rancher Labs Partnership
**Rancher, Rancher Kubernters Engine (RKE), and NGINX Kubernetes Ingress Controller (KIC) Demo Environment**

<br>

The following guide will show how to start with a minimal set of pre-requisites in your environment and build out a solution platform using Rancher Kubernetes for the Environment. NGINX Kubernetes Ingress Controller for ingres and the Rancher for Managed Kubernetes Cluster Operations.

## Dev/ Control System
It is assumed that you have a development system you are regularly using. Tool chain choices are often a personal prefrence but in the intersted of transparency below is a description of a possible setup for a dev / control system. This is sometimes referred to as a jump host.

**OS**
* Ubuntu 18.04 LTS

**Browser**
* Firefox

**Code Editor / IDE**
* Visual Studio Code (VS Code)

**VS Code Extensions Minimum**
* Docker 1.11.0 - Microsoft
* Kubernetes 1.3.0 - Microsoft
* YAML 0.16.0 - Red Hat
* Go 0.23.2 - Go team at Google

**Path Binaries**
* Required
    * kubectl - more info
    * rke - more info
    * helm - more info
* Recomended
    * 



## Prerequisites
One objective for this project was to ensure that a very minimal amount of prerequisites were needed to get started. The following are the absolute bare minimum it was currently possible to get away with. In our example the Unified Demo Framework (UDF) pattern has all of the prerequisites in place at the time of it's launch I am including more detailed information for those folks who would like to try to roll their own in the future.

<br>

**Kubernetes - RKE**
* All systems must utilize DNS.
* Base OS in our case Ubuntu 18.04 LTS this will be changing to SUSE in the future.
* Docker installed [more info](https://docs.docker.com/engine/install/).
* SSH keys should be copied to the target K8s nodes.

**Docker Registry**
* docker registry install (one system) [more info](https://docs.docker.com/registry/deploying/).
* Systems that will be utilizing the Docker registry should either be conigure to use the registry securely or to use and insecure registry.

**NGINX Plus**
* Build and publish NGINX Plus to the Docker registry

**NOTE:** In our demo environment all systems that will run containers were set up to use an insecure registry (demo and dev only.) Follow the instructions at this [link](https://docs.docker.com/registry/insecure/)

<br>

## Install Rancher
In this demo we are running Rancher in a container on a single system. This is configured and ready to use in the current UDF pattern. However if you are attempting to roll your own the command below can be run on any system that has a base OS and Docker installed to get teh Rancher management solution up and running.

**NOTE:** The entire solution should be utilizing DNS for Rancher to function properly. Our UDF pattern has a Bind 9 DNS available with Webmin to configure it.

```
sudo docker run --privileged -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher
```
For further details please see the information at this [link](https://rancher.com/docs/rancher/v2.x/en/quick-start-guide/deployment/quickstart-manual-setup/)

Once the Rancher container is up and running and the browser configuration is complete bookmark the Rancher site for later use. This has already been booked marked in the UDF pattern.

<br>

## Install Rancher Kubernetes Engine
To being the installation please locate the "cluster.yaml" in this repo. It is recommended that you make a duplicate of the cluster yaml and name is something appropriate to the cluster in this case it will be named "rke-cluster.yaml" which will be the name of the demo RKE cluster.

It is important to note that this install is a bare bones installation of RKE.  This will give us a solid foundation for our K8s cluster. For the purposes of this demo we will be leaving out components that are critical for a production cluster however those can be layered in at any time in the future.

Start by editing the "rke-cluster.yaml" This will be a three node cluster the three IP addresses for our nodes will need to be aded to the cluster.yaml. The UDF pattern has three nodes already set up, those are listed below and their IP addresses can be found in the primary description area of the UDF pattern as well.

**RKE Control Node**
```
10.1.1.6
```
**RKE Worker Nodes**
```
10.1.1.7
10.1.1.8
```

When finished editing the "rke-cluster.yaml" run the following command to bring the baseline cluster up. This should take 2 - 5 minutes to  complete depending on the hardware being used.

```
rke up --config=./rke-luster.yaml
```

After the install completes copy over the cofig this will allow you to use the local copy of kubectl to manipulate the cluster.

```
cp kube_config_cluster.yaml /home/ubuntu/.kube/config
```

## Add Cluster to Rancher

Once the RKE is up and running open up a browser and go to the Rancher managment link booked marked in a previous step. You may need to do the following if it is not already complete. Set the password then set it to manage multiple clusters, deselect the stats collection tick box and submit then accept the next dialog that sets the url and your done.

While logged into the Rancher Management server click the rancher logo to ensure your at the top level and then locate and click the add cluster button then select the other cluster option name the cluster "rke-cluster" then click create.

From the following page click the copy button for the first code snippet provided it should look like this.

```
kubectl create clusterrolebinding cluster-admin-binding --clusterrole cluster-admin --user [USER_ACCOUNT]
```

Paste the above command into the VSCode cli then locate the kube_config_cluster.yaml in the Rancher VSCode repo and find the user account under context it should look like this.

```
user: "kube-admin-rke-cluster"
```

Replace [USER_ACCOUNT] in the pasted command line with the text between the quotes in this example it looks like the example below then hit enter. This will create the required cluster role binding for adding the RKE cluster to the Rancher Management server.

```
kubectl create clusterrolebinding cluster-admin-binding --clusterrole cluster-admin --user kube-admin-rke-cluster
```

Next go back the the browser and click the copy button for the last cli option we will not be using the middle option as we are runnig with self signed certs for this example. it should look similar to this

```
curl --insecure -sfL https://rancher.demo.example/v3/import/m672vckj6zhfs725xwnt9ln5nk66s57q8fvlhwpnwscs75xjtvhddz.yaml | kubectl apply -f -
```

This will add the RKE cluster to the Rancher Management. Continue to wait, it will automatically go back to the Rancher main page, you should now see the RKE cluster in the list.

## Installing NGINX Plus into the RKE cluster
For this demo we will be installing NGINX KIC into the 


NGINX install info
```
utility.bddemo.udf:5000/nginx-plus-ingress 
```
## NGINX Demo App


## Bonus Materials

### Configure NFS

### Install Prometheus

### Install Grafana