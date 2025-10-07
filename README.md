# dotfiles

Personal config files for various applications

## Contents

- [dotfiles](#dotfiles)
  - [Contents](#contents)
  - [Applications](#applications)
    - [alacritty](#alacritty)
    - [i3wm](#i3wm)
    - [zsh](#zsh)
      - [Install Oh-My-Zsh](#install-oh-my-zsh)
      - [Install theme and addons](#install-theme-and-addons)
      - [Copy and apply config files](#copy-and-apply-config-files)

## Applications

The following applications are included in the config files.

### alacritty

Config found [here](.config/alacritty/alacritty.toml).

    sudo pacman -S alacritty

### i3wm

Config found [here](.config/i3/config).

     sudo pacman -S feh rofi

### zsh 

This setup includes the ["powerlevel10k" theme](.p10k.zsh), as well as the "zsh-autosuggestions" and "zsh-syntax-highlighting" addons.

#### Install Oh-My-Zsh

    sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

#### Install theme and addons

    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k && git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions && git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

#### Copy and apply config files

Apply theme and plugin sections to .zshrc

    ZSH_THEME="powerlevel10k/powerlevel10k"

    plugins=(
        git
        zsh-autosuggestions
        zsh-syntax-highlighting
    )