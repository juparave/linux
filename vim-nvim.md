## vim && nvim

## Install latests

### Vim

On Debian/Ubuntu

    # add-apt-repository ppa:jonathonf/vim
    # apt update
    # apt install vim

### Neovim

On Debian/Ubuntu
Before proceeding, run the following command to ensure the following dependencies are installed.

    $ sudo apt install software-properties-common -y

Import Stable Neovim PPA

    $ sudo add-apt-repository ppa:neovim-ppa/stable -y

Import Unstable Neovim PPA

    $ sudo add-apt-repository ppa:neovim-ppa/unstable -y

Run an APT update to sync the changes.

    $ sudo apt-get update

Install NeoVim
With the PPA imported, install the editor.

    $ sudo apt install neovim -y


## Windows/Panes

    <C-w>r  Rotate windows
    <C-w>t  Switch from horizontal split to vertical
    <C-w>h  Switch from vertical split to horizontal

## Terminal fonts

Best font so far:

    $ brew tap homebrew/cask-fonts
    $ brew install --cask  font-inconsolata-nerd-font

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

## Folds

    :set foldmarker={,}

Use folds with `za` and `zo`

## Formatters

### Python

For python use external tool autopep8, to run actiber buffer do:

    :%!autopep8 - 2> /dev/null

Still haven't found a way to do it better, tried with:

    augroup Formatters
        autocmd!
        autocmd FileType python let &l:formatprg = 'isort - 2> /dev/null && autopep8 - 2> /dev/null'
    augroup END

ref: https://www.reddit.com/r/vim/comments/a914xh/vimprettier/
