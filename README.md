## Cloud-native Trino (prestosql) + Hive + Minio 

Trino engine with Hive Metastore and Minio filesystem.
This configuration is not for Trino deployments in production.

## Technologies:
### Query Engine is `Trino (PrestoSQL)`
### Metadata Store is `Apache Hive`
### Object Storage is `Minio (S3 compatable)`

## Get things running
1. Clone repo
2. Install docker/podman + docker-compose
3. Run `docker-compose up`
4. Done! Checkout the service endpoints:

Trino: `http://localhost:8080/ui/`<br>
Minio: `http://localhost:9001/` (username: `minio`, password: `minio123`)<br>

## Guid for podman users on MacOS:
On MacOS the podman project does not expose the podman.socket which is similar to docker.socket, by default.
So to get docker-compose working one needs to expose the socket.

To get the socket running run the following commands.
First we need to find the port it is exposed on in the VM.

```bash
podman system connection ls
```

Then we need to take that port and create a forward ssh connection to that.

```bash
ssh -fnNT -L/tmp/podman.sock:/run/user/501/podman/podman.sock -i ~/.ssh/podman-machine-default ssh://core@localhost:<port to socket> -o StreamLocalBindUnlink=yes
export DOCKER_HOST='unix:///tmp/podman.sock'
```

Second, we expose the DOCKER_HOST env variable that is used by docker-compose.

Be aware that if the connection is disconnected one needs to delete/overwrite the /tmp/podman.socket to run the forward command.

## Let's start
Use docker exec to run commands inside the container

```bash
docker exec -it trino trino --user admin
```

You could also connect to trino-server from host machine via trino-cli

```bash
trino-cli --server localhost:8080 --user admin
```

For getting full rights you need to set role admin in hive

```sql
SET ROLE admin IN hive;
```
