# Lab1: Setup A New User Account and Allow Login

A new member has joined our team and we need to provide her with a new account and allow remote login.

## Setup

For this lab we'll use a new machine and connect its ssh port to a public port on the host. Since 
we are many people using the same server, you'll use your userid for the public port.

Create the machine with the command:

```
$ docker run -d --rm -p $((UID + 1000)):22 ubuntu sleep infinity
```

Now run `docker ps`, find your new machine (by its ID as printed from the last command) and detect the SSH port. For my machine the line from `docker ps` was:

```
223a36dd515a   ubuntu    "sleep infinity"   4 seconds ago   Up 3 seconds   0.0.0.0:2000->22/tcp   pensive_margulis
```

And so the host port is 2000.

Now we can start a shell on the new machine with:

```
docker exec -it 223a36dd515a /bin/bash
```

## Install SSH Daemon on your new server
To accept SSH connections, your new machine needs to run an SSH Daemon. The package is called:
openssh-server

Use `apt-get` and `apt-cache` to find and install this package.

After installation we need to "start" the ssh service by running:

```
$ service ssh start
```

Let's verify everything works:

1. Create a test user on your new machine named test, and choose a password for that user

2. Run "ssh test@localhost" to connect to the same machine

3. Type your password and verify you are connected as test user

4. Delete user "test"

## Create The New User with SSH keys
Follow the instructions here to create a new user AND ssh keys for this user:
https://www.howtoforge.com/how-to-configure-ssh-keys-authentication-with-putty-and-linux-server-in-5-quick-steps

Name the new user "jane"

Use Windows putty to connect to the new machine by typing the IP address of our server (178.62.203.48) and the port for your docker (in my case it was 2000).


