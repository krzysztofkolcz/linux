### Grep throug .gz files
find -name \*.gz -print0 | xargs -0 zgrep 'doDowngradeHostingToPackage'

### grep sort:
grep -rl 'eventbyevent' . | sort -t: -n -k2
#### grep sort by date:
ls -rt | xargs grep -l 'asda1fr.pl'

### Extracg .gz file
gunzip file.gz

### Extracg .tar.gz file
tar -zxvf file.tar.gz

### rm find
find . -name "FILE-TO-FIND" -exec rm -rf {} \;

### file size in MB
ls -l --block-size=M

### get ip of hostname
getent hosts hostname | awk '{ print $1 }'

### rsync 
#### download file
rsync -avze ssh username@ihostname:/path/to/file/file.txt .
#### upload file to server
rsync -avze ssh file.txt username@ihostname:/path/to/file/

### strace - problemy z procesami
Problem z funkcją mail() w php
odpalenie skryptu z przeglądarki, 
ps aux | grep fpm
strace -f -p 2457
