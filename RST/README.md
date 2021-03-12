# NGINX Inc. and Rancher Labs Partnership
**Rancher, Rancher Kubernters Engine (RKE), and NGINX Kubernetes Ingress Controller (KIC) Demo Environment**

<br>

The following guide will show how to start with a minimal set of pre-requisites in your environment and build out a solution platform using Rancher Kubernetes for the Environment. NGINX Kubernetes Ingress Controller for ingres and the Rancher for Managed Kubernetes Cluster Operations.

## Prerequisites
One objective for this project was to ensure that a very minimal amount of prerequisites were needed to get started. The following are the absolute bare minimum it was possible to get away with. In our example the UDF pattern has all of this prerequisites in place at the time of it's launch I am including this information primarily for those folks who would like to try to roll their own in the future.

<br>

Kubernetes - RKE
* All systems must utilize DNS
* Base OS in our case Ubuntu 18.04 LTS
* Docker installed https://docs.docker.com/engine/install/
* SSH keys should be copied to the target K8s systems

Docker Registry
* docker registry install (one system) https://docs.docker.com/registry/deploying/

NGINX Plus
* Build and publish NGINX Plus to the socker registry

**NOTE:** Set up the all systems that will run containers, to use an insecure registry (demo and dev only.) Follow the instructions at this link https://docs.docker.com/registry/insecure/

<br>

### Install Rancher
In this demo we are running Rancher in a container on a single system. This is configured and ready to use in the current pattern. However if you are attempting to roll your own the command below can be run on any system that has a base OS and Docker installed to get teh Rancher management solution up and running.

**NOTE:** The entire system should be utilizing DNS for optimal Rancher functionality.

```
sudo docker run --privileged -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher
```

<br>

### Install Rancher Kubernetes Engine
To being the installation

```
rke up --config=./cluster.yaml
```
After the install completes copy over the cofig

```
cp kube_config_cluster.yaml /home/ubuntu/.kube/config
```

***Configure NFS***

## Add Cluster to Rancher

Once the RKE is up and running open up a browser and go to the following link [Rancher](http://rancher.demo.example) set the password then set it to manage multiple cluster deselect the stats collection tick box and submit then accept the next dialog that sets the url and your done.


While logged into the Rancher Management server click the rancher logo to ensure your at the top level and then locate and click the add cluster button then select the other cluster option name your cluster "rke-cluster" then click create

From the following page click the copy button for the first code snippet provided it should look like this.

```
kubectl create clusterrolebinding cluster-admin-binding --clusterrole cluster-admin --user [USER_ACCOUNT]
```
Paste the above command into the cli then locate the kube_config_cluster.yaml and find the user account under context it should loook like this.

```
user: "kube-admin-rke-cluster"
```
Replace [USER_ACCOUNT] in the pasted command line with the text between the quotes in this example it look like the example below then hit enter. This will create the required cluster role binding for adding the RKE cluster to the Rancher Management server

```
kubectl create clusterrolebinding cluster-admin-binding --clusterrole cluster-admin --user kube-admin-rke-cluster
```
Next go back the the browser and click the copy button for the last cli option we will not be using the middle option as we are running insecurly for this example. it should look similar to this

```
curl --insecure -sfL https://rancher.demo.example/v3/import/m672vckj6zhfs725xwnt9ln5nk66s57q8fvlhwpnwscs75xjtvhddz.yaml | kubectl apply -f -
```
This will add the RKE cluster to the Rancher Management if you wait it will automatically go back to the Rancher main page and you should now see the RKE cluster in the list.
