To start fresh.

```bash
docker stop qleverui
docker rm qleverui
docker rmi qleverui
```

Then follow the instructions from the [Installing QLever UI](docs/install_qleverui.md) guide again.

Here go the step with slight adaptation regarding the pathes


# Building the QLever UI Docker Image
We assume [docker to be installed](https://docs.docker.com/get-docker/) on your machine for the following instructions. 
1. To get started clone the QLever UI repo on your machine:
    ```shell
    git clone https://github.com/ad-freiburg/qlever-ui.git qlever-ui
    cd qlever-ui
    ```
2. Optionally [adjust settings](#configuration)
3. Finally, build the Docker image by running:
    ```shell
    docker build -t qleverui .
    ```
    You have now created a Docker Image that contains everything you need to run QLever UI.

## Setting up the database
__NOTE: You can skip this step if you already have a database file.__  

1. To set up the database, first, run a bash shell inside the QLever UI container as follows.
    ```shell
    docker run -it --rm \
            -v "/media/data/qlever/db:/app/db" \
            --entrypoint "bash" qleverui
    ```
    Where `/media/data/qlever/db` is the path where QLever UI should store its database. If you want to use a different path, make sure to change this part in all subsequent `docker` commands.

2. Create the empty database file with the following command.
    ```shell
    python manage.py migrate
    ```
3. For configuring your QLever UI backend you will need an administrative user for the QLever UI administration panel. You can create a "superuser" by entering
    ```shell
    python manage.py createsuperuser
    ```
    and following the instructions in your terminal.  

You can now exit the container as QLever UI is finally ready to run.
## Running a QLever UI Docker Container
To run a QLever UI container use the following command:
```shell
docker run -it -p 7000:7000 \
           -v "/media/data/qlever/db:/app/db" \
           --name qleverui \
           qleverui
``` 

or to let it be, let it beeee 

```shell
docker run -d --restart=unless-stopped -p 7000:7000 \
           -v "/media/data/qlever/db:/app/db" \
           --name qleverui \
           qleverui
```

__Note:__ If you already have a QLever UI database file `qleverui.sqlite3` you want to use, make sure it is located in the specified path or provide the correct path to it.  
If you want the container to run in the background and restart automatically replace `-it` with `-d --restart=unless-stopped`  
You should now be able to connect to QLever UI via <http://localhost:7000>. Continue with [configuring QLever UI](./configure_qleverui.md).
