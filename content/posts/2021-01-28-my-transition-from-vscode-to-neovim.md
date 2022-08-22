---
title: My transition from VSCode to Neovim
author: lopeztel
date: 2021-01-28T19:32:15+00:00
url: /2021/01/28/my-transition-from-vscode-to-neovim/
tags:
  - 100DaysToOffload
  - Manjaro
  - Tutorial

---
I&#8217;ve always liked the idea of using a tiling window manager and customizing the hell out of it, you know, the kind of stuff you see by searching for the linuxPorn hashtag&#8230; My main problem with them is that I am very very used to GUIs, while I am fairly competent on a terminal, I have never learned to use [emacs](https://www.gnu.org/software/emacs/) or [vim](https://www.vim.org/). I am in the middle of transitioning to CLI based programs, on a previous post I described [ncspot and mpsyt](https://lopeztel.xyz/blog/2020/12/17/some-cli-awesomeness/), I&#8217;ve also been looking at [gomuks](https://github.com/tulir/gomuks) and [ranger](https://github.com/ranger/ranger)

I know that a lot of people use them and once you&#8217;re over the steep learning curve you can be very productive with just a keyboard, but it was only recently that I took an interest because a friend of mine pointed out that you can have auto-completion and other cool stuff in [neovim](https://neovim.io/) (if I understand correctly it is a rewrite of vim) by means of tweaking the setup and some plugins, very much _á la_ VSCode, which is my main text/code editor of choice.

I know, I know, I&#8217;m late to the legit text editor party but I always assumed vim/neovim was just ugly/simple text editor&#8230; I&#8217;ve been living a lie

## The Neovim setup

So I took a deep dive and with some pointers from my friend I installed neovim and proceeded to tweak. This post will attempt to document the process. I use Manjaro linux but I can imagine the process would be similar for other distros. I&#8217;ll try to include some links.

I started by installing neovim:

`sudo pacman -S neovim`

Then, I was pointed to to an easy way to deal with plugins: [vim-plug](https://github.com/junegunn/vim-plug), careful, there&#8217;s a different section for vim and neovim

```bash
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https:&#47;&#47;raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
```

Then proceeded to create `init.vim` (kinda like neovim&#8217;s .bashrc), the complete path is `HOME/.config/neovim/init.vim`

```vim
" lopeztel vim config

call plug#begin('~/.vim/plugged')

Plug 'plasticboy/vim-markdown'
Plug 'arcticicestudio/nord-vim'
Plug 'preservim/nerdtree'
Plug 'vim-airline/vim-airline'

call plug#end()
```

To install the plugins within neovim, change to **COMMAND** mode (press _ESC_) and type:

`:PlugInstall`

This splits the window to the left, it displays the plugin list and installation status.

some useful things to know:

  * to edit files in vim/neovim, change to **EDIT** mode (press _I_)
  * to save files in vim/neovim, change to **COMMAND** mode (press _ESC_) and type `:w`
  * to close/quit in vim/neovim, change to **COMMAND** mode (press _ESC_) and type `:q`, `:q!` to discard any unsaved changes

A bit on the plugins:

  * Changing to COMMAND mode and typing `:NERDTree` will split the window to the left and show a file tree (VSCode like), see [nerdtree](https://github.com/preservim/nerdtree) for more info
  * [nord-vim](https://github.com/arcticicestudio/nord-vim) and [vim-airline](https://github.com/vim-airline/vim-airline) are more eye candy than anything, the first one installs the [Nord theme](https://www.nordtheme.com/)&#8216;s color palette, and the second one makes the status/tabline in neovim more visually pleasing

This is the overall look:

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/nvim-me.png#center)

On to the functional stuff, since most of my work is C/C++ I decided to install Conquer of Completion or [coc.vim](https://github.com/neoclide/coc.nvim), which according to their github it is supposed to _&#8220;Make your Vim/Neovim as smart as VSCode.&#8221;_.

First I had to install nodejs, their page provides a one liner but it didn&#8217;t work for me, so I did:

`sudo pacman -S nodejs`

Also, a language server is necessary, I found that either [ccls](https://github.com/MaskRay/ccls) or [clangd](https://clangd.llvm.org/) are options, I installed ccls based on a quick online search

`sudo pacman -S ccls`

After that I installed the coc plugin with the vim-plug method described above (adding the line `Plug 'neoclide/coc.nvim', {'branch': 'release'}` to the corresponding section in `init.vim`). It is important to append the text from the [example vim configuration](https://github.com/neoclide/coc.nvim#example-vim-configuration) to `init.vim` (quite long so I won&#8217;t put it here)

There are some other configurations needed for specific language support, a config file can be opened by changing to COMMAND mode and typing

`:CocConfig`

Which opens a file called `coc-settings.json`, contained in the same path as `init.vim`, here&#8217;s how mine is configured for C/C++:

```json
languageserver": {
    "ccls": {
      "command": "ccls",
      "filetypes": &#91;"c", "cpp", "objc", "objcpp"],
      "rootPatterns": &#91;".ccls", "compile_commands.json", ".vim/", ".git/", ".hg/"],
      "initializationOptions": {
         "cache": {
           "directory": "/tmp/ccls"
         }
       }
    }
  }
```

More configurations in the [coc.vim wiki](https://github.com/neoclide/coc.nvim/wiki/Language-servers#ccobjective-c)

## Now what?

Now that I have a fully working installation it is time to learn to use neovim, I&#8217;ll probably spend some time on the tutorial (`:Tutor` in **COMAND** mode). In the meantime I&#8217;ll still use VSCode when in a rush to get things done

## More eyecandy

My discovery of the Nord theme led me down a rabbit hole and I had to apply it to the [gnome terminal](https://github.com/arcticicestudio/nord-gnome-terminal) and [VSCode][1] to match the look and feel. I also installed the [powerlevel10k](https://github.com/romkatv/powerlevel10k#oh-my-zsh) theme to my Zsh, while I was at it

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/nord_theme-me.png#center)

&#8212;

Day 45 of my 2020&#8217;s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!

 [1]: https://www.nordtheme.com/ports/visual-studio-code
