**My ZSH Configuration**
--

Optimized for running on WSL2 Ubuntu, here is the thought decision why use below stack:
1. Shell: zsh -> flexible customization, a lot of custom plugins to leverage shell functionality, large base community
2. Prompt: starship -> fast because built from rust, simple theming
3. Zplug -> enable lazy load plugins, easier plugins management
4. Linuxbrew -> easier to find and install linux package
5. eza -> improve ls alternative, built from rust

This configuration include: 
1. Compinit caching 
2. Plugins ordering for best performance and compatibility
3. Full integrations without conflict of: autojump, enhancd, fzf, ripgrep, eza

Instalation:
1. git clone into ubuntu root home folder
2. install basic necessary plugins
3. move file .zshrc.backup to outside and rename to .zshrc
4. source .zshrc


by @rhewin
