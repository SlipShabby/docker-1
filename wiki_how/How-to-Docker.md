### 01. Create a virtual machine with `docker-machine` using the `virtualbox` driver, and named `Char`.

**Answer**

```
docker-machine create --driver virtualbox Char
```

_Explanation_

[docker-machine create | Docker Documentation](https://docs.docker.com/machine/reference/create/):

>Create a machine. Requires the `--driver` flag to indicate which provider (VirtualBox, DigitalOcean, AWS, etc.) the machine should be created on, and an argument to indicate the name of the created machine.

> ### Example
> Here is an example of using the `--virtualbox` driver to create a machine called dev.
>```
>$ docker-machine create --driver virtualbox dev
>```

**How to check result?**

```
docker-machine ls
```
_Explanation_

`docker-machine`:

> `ls` — List machines.

### 02. Get the IP address of the `Char` virtual machine.

**Answer**

```
docker-machine ip Char
```

_Explanation_

[docker-machine ip | Docker Documentation](https://docs.docker.com/machine/reference/ip/):

> Get the IP address of one or more machines.
>```
>$ docker-machine ip dev
>192.168.99.104
>
>$ docker-machine ip dev dev2
>192.168.99.104
>192.168.99.105
>```

### 03. Define the variables needed by your virtual machine `Char` in the general env of your terminal, so that you can run the `docker ps` command without errors. You have to fix all four environment variables with one command, and you are not allowed to use your shell’s builtin to set these variables by hand.

**Answer**

```
eval $(docker-machine env Char)
```

_Explanation_

`docker-machine env Char` command output:

> ```
> export DOCKER_TLS_VERIFY="1"
> export DOCKER_HOST="tcp://192.168.99.100:2376"
> export DOCKER_CERT_PATH="/Users/vbrazhni/.docker/machine/machines/Char"
> export DOCKER_MACHINE_NAME="Char"
> # Run this command to configure your shell: 
> # eval $(docker-machine env Char)
> ```

**How to check result?**

`docker-machine ls` command output:

> ```
> NAME   ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
> Char   *        virtualbox   Running   tcp://192.168.99.100:2376           v18.05.0-ce   
> ```

Now the `ACTIVE` column of `Char` has `*` instead of `-`.

### 04. Get the `hello-world` container from the Docker Hub, where it’s available.

**Answer**

```
docker pull hello-world
```

_Explanation_

> ### Getting an image from Docker Hub
> Docker Hub is the place where open Docker images are stored. When we ran our first image by typing
> ```
> docker run --rm -p 8787:8787 rocker/verse
> ```
> the software first checked if this image is available on your computer and since it wasn’t it downloaded the image from Docker Hub. So getting an image from Docker Hub works sort of automatically. If you just want to pull the image but not run it, you can also do
> ```
> docker pull rocker/verse
> ```

**How to check result?**

```
docker image ls
```

_Explanation_

`docker image`:

> `ls` — List images.

### 05. Launch the `hello-world` container, and make sure that it prints its welcome message, then leaves it.

**Answer**

```
docker run hello-world
```

### 06. Launch an `nginx` container, available on Docker Hub, as a background task. It should be named `overlord`, be able to restart on its own, and have its 80 port attached to the 5000 port of `Char`. You can check that your container functions properly by visiting `http://<ip-de-char>:5000` on your web browser.

**Answer**

```
docker run -d -p 5000:80 --name overlord --restart=always nginx
```

_Explanation_

`docker run --help`:

> `-d`, `--detach` — Run container in background and print container ID.

> `--name string` — Assign a name to the container.

> `-p`, `--publish list` — Publish a container's port(s) to the host.

[Docker workshop](https://github.com/alexryabtsev/docker-workshop):

> `-p` is a ports mapping `<HOST PORT>:<CONTAINER PORT>`.

[docker run | Docker Documentation](https://docs.docker.com/engine/reference/commandline/run/):

> ### Restart policies (--restart)
>
> Use Docker’s `--restart` to specify a container’s restart policy. A restart policy controls whether the Docker daemon restarts a container after exit. Docker supports the following restart policies:
>
>Policy|Result
>:-----|:-----
>`no`|Do not automatically restart the container when it exits. This is the default.
>`on-failure[:max-retries]`|Restart only if the container exits with a non-zero exit status. Optionally, limit the number of restart retries the Docker daemon attempts.
>`unless-stopped`|Restart the container unless it is explicitly stopped or Docker itself is stopped or restarted.
>`always`|Always restart the container regardless of the exit status. When you specify always, the Docker daemon will try to restart the container indefinitely. The container will also always start on daemon startup, regardless of the current state of the container.

### 07. Get the internal IP address of the overlord container without starting its shell and in one command.

**Answer**

```
docker inspect -f '{{.NetworkSettings.IPAddress}}' overlord
```

_Explanation_

[How to retrieve Docker container's internal IP address](https://linuxconfig.org/how-to-retrieve-docker-container-s-internal-ip-address):

> It is also possible to trip the default docker inspect docker command's output to get the IP address value only:
> ```
> # docker inspect -f '{{ .NetworkSettings.IPAddress }}' e350390fd549
> 172.17.0.2
> ```

`docker inspect --help`:

> `-f`, `--format string` — Format the output using the given Go template.

### 08. Launch a shell from an `alpine` container, and make sure that you can interact directly with the container via your terminal, and that the container deletes itself once the shell’s execution is done.

```
docker run -it --rm alpine /bin/sh
```

_Explanation_

`docker run --help`:

> `-i`, `--interactive` — Keep STDIN open even if not attached.

> `-t`, `--tty` — Allocate a pseudo-TTY.

> `--rm` — Automatically remove the container when it exits.

[Starting a Shell in the Docker Alpine Container](https://stackoverflow.com/a/43564198):

> Usually, an Alpine Linux image doesn't contain `bash`, Instead you can use `/bin/ash`, `/bin/sh`, `ash` or only `sh`.

### 9. From the shell of a debian container, install via the container’s package manager everything you need to compile C source code and push it onto a git repo (of course, make sure before that the package manager and the packages already in the container are updated). For this exercise, you should only specify the commands to be run directly in the container.

**Answer**

```
apt-get update
apt-get upgrade -y
apt-get install -y build-essential git
```

_Explanation_

Start Debian container:

> `docker run -ti --rm debian`

[InstallingCompilers](https://help.ubuntu.com/community/InstallingCompilers):

> build-essential contains a list of packages which are essential for building Ubuntu packages including gcc compiler, make and other required tools.

[Download for Linux and Unix | Git](https://git-scm.com/download/linux):

> ### Debian/Ubuntu
> For the latest stable version for your release of Debian/Ubuntu
> ```
> # apt-get install git
> ```

-y flag = Automatic yes to prompts;

### 10. Create a volume named `hatchery`.

**Answer**

```
docker volume create --name hatchery
```

_Explanation_

[docker volume create | Docker Documentation](https://docs.docker.com/engine/reference/commandline/volume_create/):

> `--name` — Specify volume name.

**How to check result?**

```
docker volume ls
```

_Explanation_

`docker volume`:

> `ls` — List volumes.

### 11. List all the Docker volumes created on the machine. Remember. VOLUMES.

**Answer**

```
docker volume ls
```

_Explanation_

`docker volume`:

> `ls` — List volumes.

### 12. Launch a `mysql` container as a background task. It should be able to restart on its own in case of error, and the root password of the database should be `Kerrigan`. You will also make sure that the database is stored in the `hatchery` volume, that the container directly creates a database named `zerglings`, and that the container itself is named `spawning-pool`.

**Answer**

```
docker run -d --name spawning-pool --restart=on-failure:10 -e MYSQL_ROOT_PASSWORD=Kerrigan -e MYSQL_DATABASE=zerglings -v hatchery:/var/lib/mysql mysql --default-authentication-plugin=mysql_native_password
```
https://docs.docker.com/engine/reference/run/

_Explanation_

`docker run --help`:

> `-e`, `--env list` — Set environment variables.

[Docker MySQL Persistence (Tech Tip #83)](http://blog.arungupta.me/docker-mysql-persistence/):

> `/var/lib/mysql` is the default directory where MySQL container writes its files.

[mysql | Docker Documentation](https://docs.docker.com/samples/library/mysql/#mysql_root_password):

> ```
> MYSQL_ROOT_PASSWORD
> ```
> This variable is mandatory and specifies the password that will be set for the MySQL root superuser account.

> ```
> MYSQL_DATABASE
> ```
> This variable is optional and allows you to specify the name of a database to be created on image startup. If a user/password was supplied (see below) then that user will be granted superuser access (corresponding to GRANT ALL) to this database.

[Wordpress latest does not works with mysql latest container](https://github.com/docker-library/wordpress/issues/313):

> MySQL 8 changed the password authentication method. You're looking for the mysql_native_password plugin
[https://dev.mysql.com/doc/refman/8.0/en/native-pluggable-authentication.html](https://dev.mysql.com/doc/refman/8.0/en/native-pluggable-authentication.html)

>Or you could use mysql 5.7

**Alternative answer**

```
docker run -d --name spawning-pool --restart=on-failure:10 -e MYSQL_ROOT_PASSWORD=Kerrigan -e MYSQL_DATABASE=zerglings -v hatchery:/var/lib/mysql mysql:5.7
```

If you want to try alternative answer (after checking main answer) you need to recreate volume `hatchery`. In another way you can get errors.

### 13. Print the environment variables of the `spawning-pool` container in one command, to be sure that you have configured your container properly.

**Answer**

```
docker inspect -f '{{.Config.Env}}' spawning-pool
```

### 14. Launch a `wordpress` container as a background task, just for fun. The container should be named `lair`, its 80 port should be bound to the 8080 port of the virtual machine, and it should be able to use the `spawning-pool` container as a database service. You can try to access `lair` on your machine via a web browser, with the IP address of the virtual machine as a URL. Congratulations, you just deployed a functional Wordpress website in two commands!

**Answer**

```
docker run -d --name lair -p 8080:80 --link spawning-pool:mysql wordpress
```

_Explanation_

[Legacy container links | Docker Documentation](https://docs.docker.com/network/links/):

> ### Communication across links
> Links allow containers to discover each other and securely transfer information about one container to another container. When you set up a link, you create a conduit between a source container and a recipient container. The recipient can then access select data about the source. To create a link, you use the `--link` flag.

### 15. Launch a `phpmyadmin` container as a background task. It should be named `roach-warden`, its 80 port should be bound to the 8081 port of the virtual machine and it should be able to explore the database stored in the `spawning-pool` container.

**Answer**

```
docker run --name roach-warden  -d --link spawning-pool:db -p 8081:80 phpmyadmin/phpmyadmin
```

_Explanation_

[Docker Tutorial : PhpMyAdmin and MySQL Server](http://omarghader.github.io/docker-tutorial-phpmyadmin-and-mysql-server/):

> ### Running PhpMyAdmin container
> Phpmyadmin must point to MySQL Server. So that we must link both containers by adding the option : `--link name-of-container:name-of-imag`.
> ```
> $ docker run --name myadmin -d --link mysql:db -p 8080:80 phpmyadmin/phpmyadmin
> ```

### 16. Look up the `spawning-pool` container’s logs in real time without running its shell.

**Answer**

```
docker logs -f spawning-pool
```

_Explanation_

[How do I view real time logging of Docker containers?](https://success.docker.com/article/view-realtime-container-logging):

> To view the logs of a Docker container in real time, use the following command:
> ```
> docker logs -f <CONTAINER>
> ```
> The `-f` or `--follow` option will show live log output.

### 17. Display all the currently active containers on the `Char` virtual machine.

**Answer**

```
docker ps
```

### 18. Relaunch the `overlord` container.

**Answer**

```
docker restart overlord
```

_Explanation_

`docker`:

> `restart` — Restart one or more containers.

### 19. Launch a container name `Abathur`. It will be a `Python` container, 2-slim version, its /root folder will be bound to a HOME folder on your host, and its 3000 port will be bound to the 3000 port of your virtual machine. You will personalize this container so that you can use the `Flask` micro-framework in its latest version. You will make sure that an html page displaying "Hello World" with `<h1>` tags can be served by `Flask`. You will test that your container is properly set up by accessing, via curl or a web browser, the IP address of your virtual machine on the 3000 port. You will also list all the necessary commands in your repository.

**Answer**

```
docker run --name Abathur -v ~/:/root -p 3000:3000 -dit python:2-slim
docker exec Abathur pip install Flask
echo 'from flask import Flask\napp = Flask(__name__)\n@app.route("/")\ndef hello_world():\n\treturn "<h1>Hello, World!</h1>"' > ~/app.py
docker exec -e FLASK_APP=/root/app.py Abathur flask run --host=0.0.0.0 --port 3000
```

_Explanation_

[docker exec | Docker Documentation](https://docs.docker.com/engine/reference/commandline/exec/):

> The `docker exec` command runs a new command in a running container.

> `--env` , `-e` — Set environment variables.

[Quickstart — Flask 1.0.2 documentation](http://flask.pocoo.org/docs/1.0/quickstart/):

> ### A Minimal Application
> A minimal Flask application looks something like this:
> ```
> from flask import Flask
> app = Flask(__name__)
> 
> @app.route('/')
> def hello_world():
>     return 'Hello, World!'
> ```

> To run the application you can either use the flask command or python’s `-m` switch with Flask. Before you can do that you need to tell your terminal the application to work with by exporting the `FLASK_APP` environment variable:
> ```
> $ export FLASK_APP=hello.py
> $ flask run
>  * Running on http://127.0.0.1:5000/

**How to check result?**

```
curl $(docker-machine ip Char):3000
```

_Explanation_

`man curl`:

> `curl`  is  a tool to transfer data from or to a server, using one of the supported protocols (DICT, FILE, FTP, FTPS, GOPHER, HTTP, HTTPS,  IMAP, IMAPS,  LDAP,  LDAPS,  POP3,  POP3S,  RTMP, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, TELNET and TFTP). The command is designed to work  without user interaction.

### 20. Create a local swarm, the `Char` virtual machine should be its manager.

**Answer**

```
docker swarm init --advertise-addr $(docker-machine ip Char)
```

_Explanation_

[Create a swarm | Docker Documentation](https://docs.docker.com/engine/swarm/swarm-tutorial/create-swarm/):

> Run the following command to create a new swarm:
> ```
> docker swarm init --advertise-addr <MANAGER-IP>
> ```

### 21. Create another virtual machine with `docker-machine` using the `virtualbox` driver, and name it `Aiur`.

**Answer**

```
docker-machine create --driver virtualbox Aiur
```

### 22. Turn `Aiur` into a slave of the local swarm in which `Char` is leader (the command to take control of `Aiur` is not requested).

**Answer**

```
docker-machine ssh Aiur "docker swarm join --token $(docker swarm join-token worker -q) $(docker-machine ip Char):2377"
```

_Explanation_

[docker-machine ssh | Docker Documentation](https://docs.docker.com/engine/swarm/join-nodes/):

> Log into or run a command on a machine using SSH.
> 
> To login, just run `docker-machine ssh machinename`.

[Join nodes to a swarm | Docker Documentation](https://docs.docker.com/engine/swarm/join-nodes/#join-as-a-worker-node):

> ### Join as a worker node
> To retrieve the join command including the join token for worker nodes, run the following command on a manager node:
> ```
> $ docker swarm join-token worker
>
> To add a worker to this swarm, run the following command:
> 
>     docker swarm join \
>     --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
>     192.168.99.100:2377
> ```
> Run the command from the output on the worker to join the swarm:
> ```
> $ docker swarm join \
>   --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
>   192.168.99.100:2377
>
> This node joined a swarm as a worker.
> ```

[docker swarm join-token | Docker Documentation](https://docs.docker.com/engine/reference/commandline/swarm_join-token/):

> `--quiet`, `-q` — Only display token.

### 23. Create an overlay-type internal network that you will name `overmind`.

**Answer**

```
docker network create -d overlay overmind
```

_Explanation_

> `-d`, `--driver string` — Driver to manage the Network (default "bridge").

**How to check answer?**

```
docker network ls
```

_Explanation_

`docker network`:

> `ls` — List networks.

### 24. Launch a `rabbitmq` SERVICE that will be named `orbital-command`. You should define a specific user and password for the RabbitMQ service, they can be whatever you want. This service will be on the `overmind` network.

**Answer**

```
docker service create -d --network overmind --name orbital-command -e RABBITMQ_DEFAULT_USER=root -e RABBITMQ_DEFAULT_PASS=root rabbitmq
```

_Explanation_

[library/rabbitmq | Docker Hub](https://hub.docker.com/_/rabbitmq/):

> ### Setting default user and password
> If you wish to change the default username and password of `guest` / `guest`, you can do so with the `RABBITMQ_DEFAULT_USER` and `RABBITMQ_DEFAULT_PASS` environmental variables:
> ```
> $ docker run -d --hostname my-rabbit --name some-rabbit -e RABBITMQ_DEFAULT_USER=user -e RABBITMQ_DEFAULT_PASS=password rabbitmq:3-management
> ```

`docker service create --help`:

> `-d`, `--detach` — Exit immediately instead of waiting for the service to converge (default true).

If I didn't use `-d` flag, I get the following output:

> ```
> Since --detach=false was not specified, tasks will be created in the background.
> In a future release, --detach=false will become the default.
> ```

### 25. List all the services of the local swarm.

**Answer**

```
docker service ls
```

_Explanation_

`docker service`:

> `ls` — List services.

### 26. Launch a `42school/engineering-bay` service in two replicas and make sure that the service works properly (see the documentation provided at hub.docker.com). This service will be named `engineering-bay` and will be on the `overmind` network.

**Answer**

```
docker service create -d --network overmind --name engineering-bay --replicas 2 -e OC_USERNAME=root -e OC_PASSWD=root 42school/engineering-bay
```

_Explanation_

[42school/engineering-bay](https://hub.docker.com/r/42school/engineering-bay/):

> ### How to use
> You must have an `orbital-command` running on your host or swarm, accessible with the same name into your network.
> 
> To connect to this `orbital-command`, you must set a
> * `OC_USERNAME` : Username used to access to `orbital-command`
> * `OC_PASSWD` : Password used to access to `orbital-command`

### 27. Get the real-time logs of one the tasks of the `engineering-bay` service.

**Answer**

```
docker service logs -f $(docker service ps engineering-bay -f "name=engineering-bay.1" -q)
```
_Explanation_

[docker service ps | Docker Documentation](https://docs.docker.com/engine/reference/commandline/service_ps/):

> The name filter matches on task names.
> ```
> $ docker service ps -f "name=redis.1" redis
> ```

### 28. ... Damn it, a group of zergs is attacking `orbital-command`, and shutting down the `engineering-bay` service won’t help at all... You must send a troup of Marines to eliminate the intruders. Launch a `42school/marine-squad` in two replicas, and make sure that the service works properly (see the documentation provided at hub.docker.com). This service will be named... `marines` and will be on the `overmind` network.

**Answer**

```
docker service create -d --network overmind --name marines --replicas 2 -e OC_USERNAME=root -e OC_PASSWD=root 42school/marine-squad
```

_Explanation_

[42school/marine-squad](https://hub.docker.com/r/42school/marine-squad):

> ### How to use
> You must have an `orbital-command` running on your host or swarm, accessible with the same name into your network.
>
> To connect to this `orbital-command`, you must set a
> * `OC_USERNAME` : Username used to access to `orbital-command`
> * `OC_PASSWD` : Password used to access to `orbital-command`

### 29. Display all the tasks of the `marines` service.

**Answer**

```
docker service ps marines
```

_Explanation_

`docker service`:

> `ps` — List the tasks of one or more services.

### 30. Increase the number of copies of the `marines` service up to twenty, because there’s never enough Marines to eliminate Zergs. (Remember to take a look at the tasks and logs of the service, you’ll see, it’s fun.)

**Answer**

```
docker service scale -d marines=20
```

_Explanation_

`docker service`:

> `scale` — Scale one or multiple replicated services.

[docker service scale | Docker Documentation](https://docs.docker.com/engine/reference/commandline/service_scale/):

> ### Usage
> ```
> docker service scale SERVICE=REPLICAS [SERVICE=REPLICAS...]
> ```

> `--detach` , `-d` — Exit immediately instead of waiting for the service to converge.

**How to check logs?**

```
docker service logs -f $(docker service ps marines -f "name=marines.11" -q)
```

### 31. Force quit and delete all the services on the local swarm, in one command.

**Answer**

```
docker service rm $(docker service ls -q)
```

_Explanation_

`docker service`:

> `rm` — Remove one or more services.

### 32. Force quit and delete all the containers (whatever their status), in one command.

**Answer**

```
docker rm -f $(docker ps -a -q)
```

_Explanation_

`docker`:

> `rm` — Remove one or more containers.

`docker rm --help`:

> `-f`, `--force` — Force the removal of a running container (uses SIGKILL).

### 33. Delete all the container images stored on the `Char` virtual machine, in one command as well.

**Answer**

```
docker rmi $(docker images -a -q)
```

_Explanation_

`docker`:

> `rmi` — Remove one or more images.

### 34. Delete the `Aiur` virtual machine without using `rm -rf`.

**Answer**

```
docker-machine rm -y Aiur
```

_Explanation_

`docker-machine`:

> `rm` — Remove a machine.

`docker-machine rm --help`:

> `-y` — Assumes automatic yes to proceed with remove, without prompting further user confirmation.

