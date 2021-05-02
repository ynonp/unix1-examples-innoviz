# 03 Jane Is Stuck

User Jane was locked out of the server and arrived to your desk asking for help. She has an important project to hand in, but at first every command she typed didn't work, and now she can't even login.

Find and fix the problem in Jane's account

## Setup
Start the machine with:

```
$ docker -d ynonp/lab3
```

Check the docker ID of the container with:

```
$ docker ps
```

Write down the container ID and run:

```
$ docker exec -it <CONTAINER_ID> /bin/bash
```

And when you finish don't forget to stop and delete the container with:

```
$ docker rm -f <CONTAINER_ID>
```

## Part 1
User jane used to connect with putty, but for some reason it no longer works. From the server's shell type the command:

```
$ ssh jane@localhost
```

To try to connect.

See why it doesn't work and fix it.

## Part 2
Now that `ssh jane@localhost` works you'll find user `jane` can't do much on the server. As user `jane` type:

```
$ ls
```

And you'll encounter an error message.

Find and fix the problem.


