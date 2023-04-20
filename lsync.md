https://lsyncd.github.io/lsyncd/ DOCKA



root@dlp:~# apt -y install lsyncd
root@dlp:~# mkdir /etc/lsyncd
root@dlp:~# vi /etc/lsyncd/lsyncd.conf.lua
# create new
settings{
    statusFile = "/var/run/lsyncd.pid",
    statusInterval = 1,
}
sync{
    default.rsync,
    # specify copy source directory
    source="/home/work/",
    # specify copy target Hostname or IP address : (the name set in rsyncd.conf)
    target="node01.srv.world::backup",
    # specify exclude file list
    excludeFrom="/etc/rsync_exclude.lst",
}

root@dlp:~# systemctl restart lsyncd





Make SSH key 

https://medium.com/@jpandithas/implementing-a-two-way-sync-with-lsyncd-a82026725e66

Step 1: ssh passwordless access
Lsyncd requires rsync to work without user intervention. Asymmetric (private-public key cryptography) will help immensely here.

If you have not done already, try to ssh to the other side. For example, from WebApp1 try to ssh to WebApp2 (webdev is our user)

$> ssh webdev@193.92.100.2
ssh will attempt to log in and ask you if the identity of the target should be saved, in which question you answer: yes. Try to login and then exit the ssh session.

Login to server WebApp1 and on the command prompt issue:

$> ssh-keygen -t rsa
press Enter on all questions to create your key pairs. We will also need to transfer this key to the target server. We can use the following:

$> ssh-copy-id webdev@193.92.100.2
Issue the target account password. If all has gone well, when you try to ssh again, no password will be required! if so, exit the session, as we have some more work to do.

Now, if it does not.. Do. Not. Panic. yet. One of the most probable causes of havoc is the time you may have waffled with the permissions of your user home folder.. your ssh key details reside in your home .ssh/ folder. Login to the target server, and issue the following command:

$> service sshd status -l
This should show, with a leaf of detail, what may have gone wrong and why your keys were rejected [read this, it may be helpful].

Assuming that all has gone according to our plan, we should grant the same access for the root user (via sudo). Login again on WebApp1 and now issue:

$> sudo ssh webdev@193.92.100.2
and

$> sudo ssh-keygen -t rsa
answer to the questions, prompts as before. Also copy the root key to the target as follows:

$> sudo ssh-copy-id webdev@193.92.100.2
Now the daemon should be able to access the system. Repeat the process from the other direction: WebApp2->WebApp1 for for webdev and sudo (root) users.

