Date: 2012-10-06
Title: Locate and interpret system log files
Objective: 16
Category: b. Operate Running Systems - (RHCSA)
Tags: RHCSA

Most of the logs you will deal with are going to be located in /var/log/. There are some exceptions to this, such as apache vhosts. Many people write the logs for a specific virtual host in a folder with the web content. 

Aside from the occasional exception, this is the spot. 

Logs are written in a way that makes them easy to parse through with text processing tools like cat, grep, and awk.

One example would be searching for Failed logins in /var/log/secure 

    :::bash
    $ cat /var/log/messages | grep Failed | less
    Apr  1 16:17:06 mytest sshd[19632]: Failed password for root from 84.204.56.234 port 39045 ssh2
    Apr  1 16:17:09 mytest sshd[19634]: Failed password for root from 84.204.56.234 port 39351 ssh2
    Apr  1 16:17:13 mytest sshd[19636]: Failed password for root from 84.204.56.234 port 39660 ssh2
    Apr  1 22:13:40 mytest sshd[19741]: Failed password for bin from 200.76.17.194 port 53407 ssh2
    Apr  1 22:13:43 mytest sshd[19744]: Failed password for bin from 200.76.17.194 port 40100 ssh2
    Apr  1 22:13:46 mytest sshd[19747]: Failed password for bin from 200.76.17.194 port 51759 ssh2
    Apr  1 22:13:49 mytest sshd[19749]: Failed password for bin from 200.76.17.194 port 45675 ssh2
    Apr  1 22:13:52 mytest sshd[19751]: Failed password for bin from 200.76.17.194 port 54379 ssh2
    Apr  1 22:13:55 mytest sshd[19753]: Failed password for bin from 200.76.17.194 port 12218 ssh2
    Apr  2 06:05:01 mytest sshd[20102]: Failed password for root from 117.211.83.59 port 34815 ssh2

By using the cat command we are able to read all contents of the file, but thats a lot of stuff. We only want to see Failed logins. We then pipe the result of cat, into grep and process the text there. Grep picks out any line that contains Failed. I then piped it to less to output only the last 10 lines.So now you can see all these failed password attempts on our test server. Wow. 

Lets say we just want to process the text, and get a count of how many logins were failed in this file. We can pipe the output into wc -l, which counts lines. 

    :::bash
    $ cat /var/log/messages | grep Failed | wc -l
    90

Thats a lot of failed logins. That lets us know we should probably tighten up our security a bit, maybe change the port for ssh. Thats something that is covered down the road though. But you can see the value in combining text processing utilities in order to get a clean final result. 

Some key tools to look at are:

* cat  - http://man.he.net/man1/cat
* tail - http://man.he.net/man1/tail
* head - http://linux.die.net/man/1/head
* wc   - http://linux.die.net/man/1/wc
* less - http://linux.die.net/man/1/less
* more - http://linux.die.net/man/1/more
* grep - http://linux.die.net/man/1/grep
* awk  - http://linux.die.net/man/1/awk
* sed  - http://linux.die.net/man/1/sed
