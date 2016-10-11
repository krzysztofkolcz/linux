### Grep throug .gz files
find -name \*.gz -print0 | xargs -0 zgrep 'doDowngradeHostingToPackage'

### grep sort:
grep -rl 'eventbyevent' . | sort -t: -n -k2

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


