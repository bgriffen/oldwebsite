---
layout: post
title: "Backup Buddy"
description: "Bash script to backup your work to a server."
tags: [bash, code, backup]
---

[It nearly happened to Toy Story 2](http://www.youtube.com/watch?v=yIz9eqwLt9U), it could happen to you. I’ve come close on many occasions to losing my entire life’s work. At some point (existential reasons aside), I decided to write a small script to backup everything I needed should my laptop explode. I’ve stripped it down and provided it to you.

I’ve written a little bash script to backup a particular folder to a server. It creates a tar file and then synchronises the directories. In the header of the file you will have to change it to suit your needs. You will need to execute it every time you want to backup your work, though this can be easily automated using [cron](https://bradmontgomery.net/blog/automatic-backups-with-cron-tar-and-ssh/#).

<center>
<div markdown="0"><a href="https://github.com/bgriffen/backupbuddy" class="btn">Github Repository</a></div>
</center>

{% highlight Bash %}
#!/bin/bash
# backupbuddy.sh
 
# Author: Brendan Griffen
# Contact: brendan.f.griffen@gmail.com
# Created: 2012/06/25
# Copyright (c) 2012 Brendan Griffen. All rights reserved.
 
# Description: Performs a compression of a given folder, and rsyncs
# compressed folder to destination (server). 
 
# Usage: Bash
 
#########################################################
#            PLEASE ADJUST THE FOLLOWING!               # 
#########################################################
# YOUR USERNAME
USERNAME="myusername"
# SERVER ADDRESS TO PUT BACKUP ON
SERVERNAME="server.address.com"
# DIRECTORY ON ORIGIN COMPUTER TO BACKUP
ORIGIN_DIR="/folder/origin/"
# DESTINATION DIRECTORY ON SERVER TO PUT BACKUP
DESTIN_DIR="/destination/folder/on/server/"
#########################################################
SCRIPT_DIR=$PWD"/"              # CURRENT WORKING DIRECTORY
DATE=`date +%F`                 # GET DATE
TARFILE=$DATE-backup.tar        # CREATE TAR FILENAME
BZIP=$DATE-backup.tar.bz2       # CREATE BZIP FILENAME
if [ -f $SCRIPT_DIR$TARFILE ]
then
    rm $SCRIPT_DIR$TARFILE      # IF EXISTS, THEN DELETE
    rm $SCRIPT_DIR$BZIP         # IF EXISTS, THEN DELETE
fi
echo '+ BACKUP BUDDY COMMENCING'
echo '+ CRUNCHING FILES -- PLEASE DONT HURT ME'
tar -zcf $TARFILE $ORIGIN_DIR   # CREATE TAR FILE
bzip2 $TARFILE                  # COMPRESS FILE
# synchronise directories (computer >>> server)
echo '+ OK, OK, FINAL HEADS UP - PLEASE CHECK!'
echo '-- FROM:' $SCRIPT_DIR$BZIP
echo '-- TO:  ' $USERNAME"@"$SERVERNAME":"$DESTIN_DIR
rsync -rlptDHSx -W --delete $BZIP $USERNAME"@"$SERVERNAME":"$DESTIN_DIR
# remove .tar file from laptop
echo '+ REMOVING TRASH AT ORIGIN'
if [ -f $SCRIPT_DIR$BZIP ]
then
    rm $SCRIPT_DIR$BZIP         # IF EXISTS, THEN DELETE
fi
echo '+ NOM NOM ALL DONE'
{% endhighlight %}