# Miscellaneous Information
Below is a set of information that is notworthy but not needed for the main path of the demonstration.


## Set Up and Clean Up

- Set Up
    ```
    ssh-copy-id dturnbull@192.168.1.X
    ```
**Delete all Docker containers**

- Clean Up
  
    Must be run first

    ```
    docker rm -f $(docker ps -a -q)
    ```

    Delete every Docker image
    ```
    docker rmi -f $(docker images -q)
    ```
If you wanted to install KIC via the CLI

```
helm install nginx-ingress nginx-stable/nginx-ingress --set prometheus.create=true --set controller.kind=daemonset
```
