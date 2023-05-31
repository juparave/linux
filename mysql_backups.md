# MySQL Backups


### Simple Local Backup 

```bash
#!/usr/bin/env bash

PATH2BACKUP="/home/myuser/backups"
DATABASES=("database1" "database2")
DATE=`date "+%Y-%m-%dT%H:%M:%S"`
LOCALDIR="back_${DATE//:}"
KEEPDAYS=7
OWNER=myuser
DBUSER=root
DBPASS=secretpass

cd /root

mkdir $LOCALDIR

for DATABASE in "${DATABASES[@]}"; do
    mysqldump -u$DBUSER -p$DBPASS --databases "$DATABASE" > "$LOCALDIR/${DATABASE}_dump.sql"
    bzip2 -f "$LOCALDIR/${DATABASE}_dump.sql"
    chown ${OWNER}:${OWNER} $LOCALDIR/${DATABASE}_dump.sql.bz2
done

rsync -v --delete --delete-excluded --link-dest=$PATH2BACKUP/current \
	  --timeout=999 -azP --link-dest=$PATH2BACKUP/current "$LOCALDIR" $PATH2BACKUP/back-$DATE

# update current soft link
rm -f $PATH2BACKUP/current && ln -s $PATH2BACKUP/back-$DATE $PATH2BACKUP/current
# remove old backups
find $PATH2BACKUP/ -maxdepth 1 -type d -mtime +$KEEPDAYS -exec rm -rf {} \;

rm -rf $LOCALDIR
```
