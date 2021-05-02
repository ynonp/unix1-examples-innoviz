# 04 Web Server is Down
Our company's web server has stopped responding! Quick! find what's wrong and fix it before everyone will notice.

## Setup
Run the machine with:

```
$ docker run -it -p $((UID + 1000)):80 ynonp/lab4 /bin/bash
```

(Or using "run" and "exec", whichever you prefer).

Use `docker ps` to check the container ID and the port you got, and from your browser type the IP address of the server and port. For example for port 2000 you'll type in the browser's URL:

178.62.203.48:2000

## Your Task
We need to see a welcome page in the browser, but unfortunately this is not the case. Find and fix the problem.

