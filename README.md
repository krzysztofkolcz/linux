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


### strace - problemy z procesami
Problem z funkcją mail() w php
odpalenie skryptu z przeglądarki, 
ps aux | grep fpm
strace -f -p 2457

### Replace string in all searched files
 find . -name 'FILENAME' -type f -exec sed -i 's/SEARCH/REPLACE/g' {} \;
 find . -name '*.java' -type f -exec sed -i 's/platformer01/platformer02/g' {} \;
