# lab2 - Setup New Systemd Service

systemd is a system utility that manages services on a linux box. In this lab we'll create a new systemd service, start and stop it and read its logs.

## Setup

For this lab we'll use a docker image that has systemd enabled, with port 8080 connected to the external world:

```
$ docker run -d --rm -p $((UID+1000)):8080 --tmpfs /tmp --tmpfs /run --tmpfs /run/lock -v /sys/fs/cgroup:/sys/fs/cgroup:ro jrei/systemd-ubuntu
```

(Q: why do you think not all ubuntu images have systemd enabled?)

## Setup A Wiki Server
Jane is now already a veteran developer in the team and she decided we really need a wiki server in our team to make knowledge sharing much easier. She came to you to help setup the wiki server.

Jane decided to use a wiki server written in Python called "abow". It's an open source tool and we can install it with pip.

First let's install the server and verify it works:

1. Create a new user that will run the service. It should be a system user unable to login, with a home directory. Let's call that user "wikiserver"

2. Use `su -s /bin/bash - wikiserver` to switch user to the new user you just created

3. We need to use "pip" to install markdoc, but we don't want to use system python as to not interfere with other software on the machine.

4. Install pyenv as the user markdoc and set it up so you can run "pip install" according to the instructions here:
https://github.com/pyenv/pyenv

5. Run "pip install abow"

6. Start a test server with:

```
$ bottle.py abow:application
```

If all goes well you'll see a message stating your server is ready, but you still won't be able to connect to it from an external computer. The server "listens" on IP address 127.0.0.1:8080, which is only accessible from within the machine.

Create a new python file called "server.py" with the following code:

```
import bottle, abow

bottle.run(abow.application, host="0.0.0.0", port=8080)
```

And run:

```
python server.py
```

Then from your windows machine connect to the internal docker wiki server by typing in the browser:

http://178.62.203.48:2000

(replace the 2000 with your port)

And if all goes well you'll see abow welcome screen in your browser.



## Create a systemd service to control the wiki server

systemd manages startup services on the machine. By turning our server into a systemd service we'll be able to better control it, run it in the background and restart automatically when the server starts.

A service needs to know:

1. What file to execute

2. What environment variables to use

3. In what directory

## Create an Environment File
Begin with (2) - As user "wikiserver" run the command:

```
$ env > envfile
```

This takes all environment variables and saves them to a file named envfile. Verify the file was created and it has all the info.

## Find the full path to python and to server.py
In the service file we'll need to use full paths. As user "wikiserver" type the command:

```
$ which python
```

And write down the result. We'll use this full path to python in the service file.

## Create the systemd service file
As User "root":
Create a new file named `/lib/systemd/system/abow.service` with the following text:

```
[Unit]
Description=abow wiki server

[Service]
User=wikiserver
Restart=on-failure
WorkingDirectory=/home/wikiserver
EnvironmentFile=/home/wikiserver/envfile
ExecStart=/home/wikiserver/.pyenv/shims/python /home/wikiserver/server.py

[Install]
WantedBy=multi-user.target
```

Chnage the path to python to the one you found earlier.

Finally install the new service file by running:

```
$ systemctl daemon-reload
```


## Start, Stop and view logs of the application

The command:

```
$ systemctl start abow
```

Starts the service.

The command:

```
$ systemctl status abow
```

Displays status on the service (showing if it's currently running)

And the command:

```
$ systemctl stop abow
```

Stops the service.

We can read the service logs by running the command:

```
$ journalctl -u abow
```

Try this:

1. Start the service

2. See you can browser abow homepage from your browser

3. Stop the service

4. Verify in your browser that you can no longer reach the wiki

5. Check in abow logs who accessed the wiki









## Tips / Troubleshooting

0. Don't forget to run `apt-get update` before all installations.


1. Before installing pyenv you need to install all dependencies by running:

```
apt-get install -y build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
```


2. pyenv has an auto installer that you can run (as user markdoc) with:

```
curl https://pyenv.run | bash
```


3. After installing pyenv you'll need to modify ~/.bashrc file. But you don't have an editor on the machine. Use the following command (as user root) to install vim and pico:

```
apt-get install vim nano
```



4. After Installing pyenv and runnign "pyenv install 3.9.1" to install a python version you'll get the error:
python-build: TMPDIR=/tmp cannot hold executables (partition possibly mounted with `noexec`)

To solve this we'll need to tell the installation to use another TMPDIR. Create a directory called tmp in your home directory and run:

```
export TMPDIR=/home/markdoc/tmp
```

To set the TMPDIR environment variable before installing a python version.

After installation is finished you can use:

```
$ pyenv global 3.9.1
```

To set the default python version for user markdoc to 3.9.1


