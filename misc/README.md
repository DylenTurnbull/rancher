# Miscellaneous
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
