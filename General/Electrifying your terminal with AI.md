 I use terminal/terminal adjacent extensively in my daily routine, whether it be do programming or to listen to music or to take notes like what I am doing now. It is a pretty standard setup with zsh+tmux+neovim on Ubuntu 24.04.2 LTS. The config files can be found in my github repo [dotfiles](https://github.com/dkbhaskaran/dotfiles). Today we are going to add AI assistance to my terminal workflow

The first thing that comes to mind is the AI assimilation in shell. Well one can ask what's the use of it -well these can translate requests described in English (or some other language ?)  to shell commands. For example if you need a command but forgot the package it is part of (often my case), you can ask the AI to install the command or write a script to monitor cpu usage.

One of the ways to incorporate AI in shell is through [tmuxai](https://github.com/alvinunreal/tmuxai). This is gaining po

Lets start with tmuxai the configuration needed are follows
1. Install tmuxai through below command. Once 
```
# install tmux if not already installed
curl -fsSL https://get.tmuxai.dev | bash
```
2. Create an account with https://openrouter.ai and then create a API token key at https://openrouter.ai/settings/keys for tmuxai. 