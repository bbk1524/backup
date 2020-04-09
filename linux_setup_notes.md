I wanted this setup on Linux and here is how I got it. These are notes, not a tutorial, and probably miss important things :)

# zsh

zsh is really old. This method lets me build a new one so I can use the same config between linux and macos

Ooh I like https://jdhao.github.io/2018/10/13/centos_zsh_install_use/

## ncurses

```
wget ftp://ftp.gnu.org/gnu/ncurses/ncurses-6.1.tar.gz
tar xf ncurses-6.1.tar.gz
cd ncurses-6.1
./configure --prefix=$HOME/local CXXFLAGS="-fPIC" CFLAGS="-fPIC"
make -j && make install
```

that went off without a hitch

## zsh

Cloning https://github.com/zsh-users/zsh/blob/master/INSTALL

```
./Util/preconfig
./configure --prefix="$HOME/local" \
    CPPFLAGS="-I$HOME/local/include" \
    LDFLAGS="-L$HOME/local/lib"
make -j && make install
```

Got the following error:

```
make[1]: Entering directory `/home/bkane/Git-personal/zsh/Doc'
/bin/sh ../mkinstalldirs /home/bkane/local/share/man/man1
for file in zsh.1 zshbuiltins.1 zshcalsys.1 zshcompctl.1 zshcompwid.1 zshcompsys.1 zshcontrib.1 zshexpn.1 zshmisc.1 zshmodules.1 zshoptions.1 zshparam.1 zshroadmap.1 zshtcpsys.1 zshzftpsys.1 zshzle.1 zshall.1; do \
    test -s $file || exit 1; \
    /usr/bin/install -c -m 644 $file /home/bkane/local/share/man/man1/`echo $file | sed 's|zsh|zsh|'` || exit 1; \
done
make[1]: *** [install.man] Error 1
make[1]: Leaving directory `/home/bkane/Git-personal/zsh/Doc'
make: *** [install.man] Error 2
```

but the binary still works baby :)

## bash_profile

Using ~/.bashrc

```
export PATH=$HOME/local/bin:$PATH
export SHELL=`which zsh`
[ -f "$SHELL" ] && exec "$SHELL" -l
```

## zsh config

I changed the repo to not need the `--dotfiles` arg anymore so I can use RedHat's `stow`

Need to install pastel

## pastel

https://github.com/sharkdp/pastel/releases/download/v0.7.1/pastel-v0.7.1-x86_64-unknown-linux-musl.tar.gz

```
cd Downloads
scp bbkane-mac01:/Users/bkane/Downloads/pastel-v0.7.1-x86_64-unknown-linux-musl.tar.gz .
tar xvf pastel-v0.7.1-x86_64-unknown-linux-musl.tar.gz
cd bin
mv ~/Downloads/pastel-v0.7.1-x86_64-unknown-linux-musl .
ln -s ~/bin/pastel-v0.7.1-x86_64-unknown-linux-musl/pastel ~/bin/pastel
```

## autosuggestions

https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md#manual-git-clone

```
cd ~/Git-personal
git clone https://github.com/zsh-users/zsh-autosuggestions
```

```
# .zshrc
source ~/Git-personal/zsh-autosuggestions/zsh-autosuggestions.zsh
bindkey '^ ' autosuggest-accept  # also use Ctrl+Space to accept
```

## fzf

https://github.com/junegunn/fzf#using-git

```
git clone --depth 1 https://github.com/junegunn/fzf.git ~/Git-personal/fzf
~/Git-personal/fzf/install
```

## fasd

https://github.com/clvv/fasd#install

```
cd ~/Downloads
scp bbkane-mac01:/Users/bkane/Downloads/clvv-fasd-1.0.1-0-g4822024.tar.gz .
tar xvf clvv-fasd-1.0.1-0-g4822024.tar.gz
cd ~/bin
mv ~/Downloads/clvv-fasd-4822024 .
ln -s ~/bin/clvv-fasd-4822024/fasd ~/bin/fasd
```

## fast-syntax-highlighting

git clone https://github.com/zdharma/fast-syntax-highlighting ~/Git-personal/fast-syntax-highlighting

## final zshrc

```
# See https://github.com/bbkane/dotfiles
source ~/.zshrc_common.zsh

# See https://github.com/bbkane/dotfiles
source ~/.zshrc_prompt.zsh
zp_prompt "$(pastel gradient -n 7 salmon cornflowerblue | pastel format hex)"

source ~/Git-personal/zsh-autosuggestions/zsh-autosuggestions.zsh
bindkey '^ ' autosuggest-accept  # also use Ctrl+Space to accept

[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh

# NOTE: this has to build a database of frecently used files and dirs
# so it won't be useful for a while. Once it has a list, use `z <fuzzyname>`
# to cd into a directory or `v <fuzzyname>` to nvim it. Push <TAB> to complete from list
eval "$(fasd --init auto)"
alias v='f -e nvim' # quick opening files with nvim
bindkey '^X^A' fasd-complete

source ~/Git-personal/fast-syntax-highlighting/fast-syntax-highlighting.plugin.zsh
```

# nvim

https://github.com/neovim/neovim/wiki/Installing-Neovim#appimage-universal-linux-package

This is a little slow to start but what ya gonna do?

```
cd ~/bin
curl -LO https://github.com/neovim/neovim/releases/download/stable/nvim.appimage
chmod +x ./nvim.appimage
ln -s ~/bin/nvim.appimage ~/bin/nvim 
```

```
:InstallVimPlug
# quit and restart
:PlugInstall
```

# ripgrep

https://github.com/BurntSushi/ripgrep#installation

```
cd Downloads
scp bbkane-mac01:/Users/bkane/Downloads/ripgrep-12.0.0-x86_64-unknown-linux-musl.tar.gz .
cd ~/bin
mv ~/Downloads/ripgrep-12.0.0-x86_64-unknown-linux-musl .
ln -s ~/bin/ripgrep-12.0.0-x86_64-unknown-linux-musl/rg ~/bin/rg
```