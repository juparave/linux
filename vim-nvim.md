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

#### Using grep

Search file with pattern

    $ grep -le --include=*.py -rnw '.' -e "Exception,"

Now use that output to open all files into vim buffers

    $ vim `grep -le --include=*.py -rnw '.' -e "Exception,"`

Apply the Vim command `:bufdo` to all of these files and perform actions such as interactive search-and-replace

    :bufdo %s/Exception, /Exception as /gce

or non interactive with

    :bufdo %s/Exception, /Exception as /g


#### Using `:vimgrep`

    :vimgrep /Service.lead/j **/*

* j: Without the 'j' flag Vim jumps to the first match.  With 'j' only the
  quickfix list is updated.  With the [!] any changes in the current buffer are
  abandoned.

All files that match the search pattern will be added to a `quickfix` list, then to make the `replace`

    :copen or :cwindow to open quickfix window

    :cfdo %s/Service.lead/Service.currentLead/g

Then save all files

    :wa

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

## Mason

Mason will handle the installation of LSP used in NVIM

## LSP

With nvim +v0.8.3 `gopls` started to fail ruining the whole experience. There is a bug report on `nvim-lspconfig` repo
https://github.com/neovim/nvim-lspconfig/issues/2733

Manual fix is to set the value of `mod_cache` at the top of `gopls.lua` file with the value thrown by `go env GOMODCACHE`

```lua
local util = require 'lspconfig.util'
local mod_cache = '/home/myuser/go/pkg/mod'
```
