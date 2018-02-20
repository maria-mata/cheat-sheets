# iTerm Agnoster Theme Setup
* Change Settings ([Reference](https://github.com/robbyrussell/oh-my-zsh/wiki/themes))
```bash
cd ~
atom .zshrc
# in atom, change theme name to agnoster (around line 10)
ZSH_THEME="agnoster"
# in terminal, refresh the theme
source .zshrc
```
* Install [Powerline fonts](https://github.com/powerline/fonts)
```bash
git clone https://github.com/powerline/fonts.git
cd fonts
./install.sh # <= installs the fonts
```
* Open Terminal Preferences (`cmd ,`)
* Under `Profiles > Text > Font` click on `Change Font`
* Under `Font Family` choose `Meslo LG M DZ for Powerline` (whichever that uses Powerline)
* Under `Type Face` choose `Regular for Powerline` and size 14
* If desired, update the colors and other preferences here
