# rancher

Install Rancher

```
sudo docker run --privileged -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher
```

Install RKE

```
rke up --config=./cluster.yaml
```
Afert the install completes copy over the cofig

```
cp kube_config_cluster.yaml /home/ubuntu/.kube/config
```

***Configure NFS***

## Add Cluster to Rancher

Once the RKE is up and running open up a browser and go to the following link [Rancher](http://rancher.demo.example) set the password then set it to manage multiple cluster deselect the stats collection tickbox and submit then accept the next dialog that sets the url and your done.


While logged into the Rancher Managment server click the rancher logo to ensure your at the top level and then locate and click the add cluster button then select the other cluster option name your cluster "rke-cluster" then click create

From the followin page click the copy button for the first code snippit provided it should look like this.

```
kubectl create clusterrolebinding cluster-admin-binding --clusterrole cluster-admin --user [USER_ACCOUNT]
```
Paste the above command into the cli then locate the kube_config_cluster.yaml and find the user account under context it should loook like this.

```
user: "kube-admin-rke-cluster"
```
Replace [USER_ACCOUNT] in the pasted command line with the text between the quotes in this example it look like the example below then hit enter. This will create the required cluster role binding for adding the RKE cluster to the Rancher Managment server

```
kubectl create clusterrolebinding cluster-admin-binding --clusterrole cluster-admin --user kube-admin-rke-cluster
```
Next go back the the browser and click the copy button for the last cli option we will not be using the middle option as we are running insecurly for this example. it should look similar to this

```
curl --insecure -sfL https://rancher.demo.example/v3/import/m672vckj6zhfs725xwnt9ln5nk66s57q8fvlhwpnwscs75xjtvhddz.yaml | kubectl apply -f -
```
This will add the RKE cluster to the Rancher Managment if you wait it will automatically go back to the Rancher main page and you should now see the RKE cluster in the list.