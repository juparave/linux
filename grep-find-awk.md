## grep, find, awk

## Find text in files

    $ grep --include=\*.{c,h} -rnw '/path/to/somewhere/' -e "pattern"

Example

    $ grep --include=\*.{html,htmlx} -rnw './' -e "manage" | grep "py:if"
    $ grep --include=\*.py --exclude-dir={\*site-packages\*,\*tg\*} -rnw './' -e "xls"

## Find and replace text in files

    $ LC_ALL=C find . -type f -name '*.txt' -exec sed -i '' s/this/that/ {} +

## Find 10 latests files

    # linux
    $ find . -type f -printf "%C@ %p\n" | sort -rn | head -n 10
    # linux, human readable dates
    $ find . -type f -printf '%TY-%Tm-%Td %TH:%TM: %Tz %p\n'| sort -n | tail -n10

    # darwin
    $ find . -type f -print0 | xargs -0 stat -f "%m%t%Sm %N" | sort -rn | head -10 | cut -f2-
    $ find . -type f -print0 | xargs -0 stat -f "%m %N" | sort -rn | head -10 | cut -f2- -d" "

## Replace spaces in file names using a bash script

ref: https://stackoverflow.com/a/18213120/116239

    $ for f in *\ *; do mv "$f" "${f// /_}"; done

Delete files with correct name and then rename files with space in names

    $ for f in *_.jpg; do rm -i "${f//_/}";  mv "$f" "${f//_/}"; done

- [Find and replace using vim](vim-nvim.md)

## Start a Simple HTTP Server in Any Folder

### Python 2.7

    $ python -m SimpleHTTPServer 8000

### Python 3

    $ python3 -m http.server 9000

## Update local angular-cli to the latest

    $ npm uninstall --save-dev angular-cli
    $ npm install --save-dev @angular/cli@latest
    $ npm install
    $ ng --version

## How to use Homebrew on a Multi-user MacOS Sierra Setup

    $ chgrp -R admin /usr/local/*
    $ chmod -R g+w /usr/local/*

## Enable standby in MacBooks (hibernate)

    $ sudo pmset -a standby 1
    $ pmset -g | grep standby
    standbydelay         4200
    standby              1

## Activating desired Java SDK version

### List Java versions installed

    /usr/libexec/java_home -V

### Java 10

    export JAVA_HOME=$(/usr/libexec/java_home -v 10)

### Java 9

    export JAVA_HOME=$(/usr/libexec/java_home -v 9)

### Java 1.8

    export JAVA_HOME=$(/usr/libexec/java_home -v 1.8)

### Java 1.7

    export JAVA_HOME=$(/usr/libexec/java_home -v 1.7)

### Java 1.6

    export JAVA_HOME=$(/usr/libexec/java_home -v 1.6)

# Images and Video

## Use ffmpeg to reduce a video file size.

    # get bit size
    $ ffmpeg -i some_file.mp4
    # Reduce bitrate ...
    $ ffmpeg -i some_file.mp4 -b 800k output.mp4
    # Reduce `constant rate factor` CRF -better quality-
    $ ffmpeg -i some_file.mp4 -vcodec libx264 -crf 20 output.mp4

## Find jpgs larger than 110k

    # linux
    $ find -iname "*.jpg" -type f -size +110k -exec ls -lh {} \; | awk '{print $9 "|| Size: " $5}'

Use `mogrify` to reduce size

    # linux
    $ find -iname "*.jpg" -type f -size +110k -exec mogrify -strip -resize 800x -quality 75 {} \;
