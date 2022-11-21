## vim && nvim

## Use Vim to search and replace

Search file with pattern

    $ grep -le --include=*.py -rnw '.' -e "Exception,"

Now use that output to open all files into vim buffers

    $ vim `grep -le --include=*.py -rnw '.' -e "Exception,"`

Apply the Vim command `:bufdo` to all of these files and perform actions such as interactive search-and-replace

    :bufdo %s/Exception, /Exception as /gce

or non interactive with

    :bufdo %s/Exception, /Exception as /g

## Find dups

Trick to find dups on CSV files, on VIM open the file and sorted with `:sort` and save it, then sorted again with `:sort u` exit vim and do a `diff` between files.
