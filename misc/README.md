# Miscellaneous
**information for additional cluster functionality**

## Docker Commands


**Delete all Docker containers**

Must be run first

```
docker rm -f $(docker ps -a -q)
```

Delete every Docker image
```
docker rmi -f $(docker images -q)
```

```
ssh-copy-id dturnbull@192.168.1.X
```