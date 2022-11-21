## Welcome to my Linux notes

- [DKIM](dkim.md)
- [grep, find, awk](grep-find-awk.md)

## Apache2

### Find and delete old files

    # find /path/to/files/ -type f -name '*.jpg' -mtime +30 -exec rm {} \;

Example:

Delete old Apache2 log files

    # find /var/log/apache2/ -type f -name '*.gz' -mtime +30 -exec rm {} \;

- -type f stands for files
- -name '\*.gz' only compressed log files inside directory
- -mtime +30 stands for older than 30 days
- -exec rm {} executes _rm_ command with the filelist as parameter
- \; closes the command above

### Extract all unique user agents from Apache log file with awk

[Reference](https://snippets.aktagon.com/snippets/807-how-to-extract-all-unique-user-agents-from-an-apache-log-with-awk)

    $ sudo awk -F\" '($2 ~ "^GET /"){print $6}' /var/log/apache2/access.log|sort|uniq > ua.log

## Routine maintenance

### Reduce photo upload size

Find jpg bigger than 300kb, probably those files can be optimized.

    $ find /home/user/www/uploads -type f -name '*.jpg' -size +300k -exec ls -l {} +

Find jpg bigger than 300kb and optimize them

    $ find /home/user/www/uploads -type f -name '*.jpg' -size +300k -exec jpegoptim --size=125k {} \;
