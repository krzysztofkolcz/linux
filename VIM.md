### Jak ogarnąć vim
cd ~/.vim
git clone https://github.com/VundleVim/Vundle.vim.git
kopiuje plik VIMRC.txt do ~/.vimrc

vim (bez pliku)
odpalam komende: PluginInstall
Miałem błąd w pliku .vimrc, w ścieżce:
było:
```
5 set rtp+=~/.vim/bundle/Vundle.vim
```
a ściągnąłem repo do Vundle.vim (bez bundle)
```

4 " set the runtime path to include Vundle and initialize
5 set rtp+=~/.vim/Vundle.vim
```
Teraz komenda :PluginInstall działa

### kopia wnętrza nawiasów

yi)
yi]
yi}


### ustawienie podświetlania
vim set syntax=javascript

###
:ProjectTree - explorator eclipse

### TODO 
:grep
:lgrep
:vimgrep
:lvimgrep
:vimgrep /regexp/gj ./**/*.php
:cw
:lw
:LocateFile (eclimd)

### Poruszanie w NERDTree pomiędzy oknami
Ctrl + w, w
Ctrl + w, h
Ctrl + w, l
